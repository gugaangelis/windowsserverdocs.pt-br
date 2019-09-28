---
title: Configurando o serviço guardião de host para Always Encrypted no SQL Server
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 11/03/2018
ms.openlocfilehash: 5d1396f609a425adcd87a41d3469f3aa55774851
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402298"
---
# <a name="setting-up-the-host-guardian-service-for-always-encrypted-with-secure-enclaves-in-sql-server"></a>Configurando o serviço guardião de host para Always Encrypted com Secure enclaves no SQL Server 

>Aplica-se a: Windows Server (canal semestral), Windows Server 2019, SQL Server 2019 Preview
 
[Always Encrypted com Secure enclaves](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-enclaves) na versão prévia do SQL Server 2019 é um recurso projetado para habilitar cálculos confidenciais em dados confidenciais armazenados em um banco de dado. O serviço guardião de host (HGS) desempenha um papel importante para manter seus dados seguros quando um enclave seguro, configurado para Always Encrypted, é uma enclave de memória de VBS (segurança baseada em virtualização). A segurança de um enclave de memória VBS depende da segurança do hipervisor do Windows e, mais amplamente, da segurança do computador que hospeda o SQL Server. 

Portanto, antes que um aplicativo cliente de banco de dados permita a memória VBS enclave usada para Always Encrypted executar cálculos em dados confidenciais, o aplicativo deve atestar com um HGS confiável. O atestado comprova o computador que hospeda o SQL Server, que contém o enclave, está no estado correto e pode ser confiável. Para o restante deste tópico, vamos nos referir ao computador que hospeda SQL Server como simplesmente o computador host.

Há duas maneiras mutuamente exclusivas para o aplicativo atestar: 

- O atestado de chave de host autoriza um host ao provar que ele possui uma chave privada conhecida e confiável. O atestado de chave de host e um computador host físico ou uma máquina virtual de geração 2 executando SQL Server é recomendado para ambientes de pré-produção.
- O atestado de TPM valida as medições de hardware para garantir que um host execute apenas os binários e as políticas de segurança corretos. O atestado de TPM e um computador host físico (não uma máquina virtual) em execução SQL Server é recomendado para ambientes de produção.

Para obter mais informações sobre o serviço guardião de host e o que ele pode medir, consulte [visão geral de malha protegida e VMs blindadas](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms). Observe que, embora os documentos falam sobre VMs blindadas, as mesmas proteções, arquitetura e práticas recomendadas se aplicam a SQL Server Always Encrypted usando VBS enclaves. 

Este artigo o ajudará a configurar o HGS em uma configuração recomendada. 

## <a name="prerequisites"></a>Pré-requisitos 

Esta seção aborda os pré-requisitos para HGS e máquinas host. 

### <a name="hgs-servers"></a>Servidores HGS

- 1-3 servidores para executar o HGS. 

  > [!NOTE]
  > Somente 1 servidor HGS é necessário para um ambiente de teste ou de pré-produção.

  Esses servidores devem ser protegidos cuidadosamente, pois controlam quais computadores podem executar suas instâncias de SQL Server usando Always Encrypted com Secure enclaves. 
  É recomendável que administradores diferentes gerenciem o cluster HGS e que você execute o HGS em hardware físico isolado do restante da sua infraestrutura ou em malhas de virtualização separadas ou assinaturas do Azure.

  - Windows Server 2019 Standard ou Datacenter Edition.
  - 2 CPUs
  - 8 GB DE RAM
  - armazenamento de 100 GB

  Você pode usar o [canal de manutenção de longo prazo (LTSC)](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#long-term-servicing-channel-ltsc) ou o [canal semestral](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#semi-annual-channel). 
  Para registrar e baixar o Windows Server Insider Preview, consulte [introdução ao programa Windows Insider](https://insider.windows.com/for-business-getting-started-server/).

- Escolha um nome para a nova floresta Active Directory criada pelo serviço guardião de host. 
  O HGS não deve ingressar em seu domínio corporativo existente e deve ter administradores separados Gerenciando-o.   

- Regras de roteamento e firewall para permitir tráfego HTTP de entrada (TCP 80) ou HTTPS (TCP 443) nos nós do serviço guardião de host de: 

  - Os computadores que executam o SQL Server
  - Os computadores que executam aplicativos cliente de banco de dados (como servidores Web) que emitem consultas de banco de dados e usam Always Encrypted com enclaves seguro. 

### <a name="sql-server-host-machines"></a>SQL Server máquinas host

- Sua instância de SQL Server deve ser executada em um computador que atenda aos seguintes requisitos:

  - Windows: 
    - Windows 10 Enterprise, versão 1809  
    - Windows Server 2019 datacenter (canal semianual), versão 1809
    - Windows Server 2019 Datacenter
  - Computador físico para produção ou uma máquina virtual de geração 2 para teste (Observe que o Azure não dá suporte a VMs de geração 2)
  - Requisitos gerais listados em [requisitos de hardware e software para a instalação do SQL Server](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2017).   

  Você pode usar o [canal de manutenção de longo prazo (LTSC)](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#long-term-servicing-channel-ltsc) ou o [canal semestral](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#semi-annual-channel). 
  Para registrar e baixar o Windows Server Insider Preview, consulte [introdução ao programa Windows Insider](https://insider.windows.com/for-business-getting-started-server/).

- Requisitos específicos para o modo de atestado escolhido:
  - O **modo TPM** é o modo de atestado mais forte e usará um Trusted Platform Module (TPM) para validar criptograficamente que suas máquinas host são conhecidas pelo seu datacenter (usando uma ID exclusiva de cada TPM), executando hardware e firmware confiáveis configurações (usando uma linha de base do TPM) e executando o código do kernel e do modo de usuário confiável (usando o controle de aplicativo do Windows Defender). O hardware a seguir é necessário para usar o modo TPM: 
    - Módulo TPM 2,0 instalado e habilitado 
    - Inicialização segura habilitada com a política de inicialização segura da Microsoft (não habilite a política de AC de inicialização segura de terceiros ou quaisquer políticas personalizadas)
    - IOMMU (Intel VT-d ou AMD IOV) para evitar ataques de acesso direto à memória 

  - O **modo de chave do host** usa um par de chaves assimétricas (muito parecido com as chaves SSH) para identificar e autorizar computadores host que desejam executar SQL Server. Esse modo é mais fácil de configurar e não tem requisitos de hardware específicos, mas não verificará o software ou firmware em execução nos computadores host.  

Para verificar se o TPM é compatível, execute os seguintes comandos no computador host em que você pretende executar SQL Server usando Always Encrypted com Secure enclaves. "2,0" deve aparecer na lista de SpecVersions com suporte para que você use o atestado de TPM:

```powershell
Get-CimInstance -ClassName Win32_Tpm -Namespace root/cimv2/Security/MicrosoftTpm 
```

## <a name="set-up-the-first-hgs-node"></a>Configurar o primeiro nó HGS 

O serviço guardião de host opera em uma configuração altamente disponível usando um cluster de 3 nós. É recomendável que você configure um nó completamente antes de adicionar outros nós. 

Execute todos os comandos a seguir em uma sessão do PowerShell com privilégios elevados.

1. [!INCLUDE [Install the HGS server role](../../includes/guarded-fabric-install-hgs-server-role.md)]

2. [!INCLUDE [Install HGS by default](../../includes/install-hgs-default.md)] 

3. [!INCLUDE [Determine a DNN](../../includes/guarded-fabric-initialize-hgs-default-step-one.md)]

   Para Always Encrypted com o Secure enclaves, as máquinas de host que executam SQL Server e computadores que executam aplicativos cliente de banco de dados precisam entrar em contato com o HGS, embora apenas as máquinas host exijam atestado.

4. Depois que o computador for reinicializado, o HGS será instalado e o servidor também será um controlador de domínio com Active Directory configurado. 
   Durante a configuração de Active Directory, a conta de administrador do computador local é adicionada ao grupo Admins. do domínio e somente os membros desse grupo podem entrar em um controlador de domínio.
   Entre usando a conta de administrador de domínio (por exemplo administrator@bastion.local , bastiões. local\administrator) ou crie outra conta de administrador de domínio para entrar e configure o serviço de atestado.
   Você precisará escolher atestado de chave de host ou TPM e executar o comando correspondente. 
   Para o HgsServiceName, especifique o DNN escolhido.

   Para o modo TPM:

   ```powershell
   Initialize-HgsAttestation -HgsServiceName 'hgs' -TrustTpm
   ```

   Para o modo de chave do host:

   ```powershell
   Initialize-HgsAttestation -HgsServiceName 'hgs' -TrustHostKey 
   ```

5. Verifique se os computadores host que executam SQL Server poderão resolver os nomes DNS de seus novos membros do domínio HGS Configurando um encaminhador de seus servidores DNS corporativos para o novo controlador de domínio HGS. Se você estiver usando o DNS do Windows Server, poderá configurar um encaminhador condicional executando os seguintes comandos em um console do PowerShell com privilégios elevados em um servidor DNS em sua organização. Substitua os nomes e endereços na sintaxe do Windows PowerShell abaixo conforme necessário para seu ambiente. Depois de adicionar mais nós HGS, execute este comando novamente para adicioná-los como servidores mestres.

   ```powershell
   Add-DnsServerConditionalForwarderZone -Name <HGS domain name> -ReplicationScope "Forest" -MasterServers <IP addresses of HGS servers>
   ```

## <a name="set-up-additional-hgs-nodes-for-production-deployments"></a>Configurar nós HGS adicionais para implantações de produção

Para adicionar nós ao cluster, execute os seguintes comandos em uma sessão do PowerShell com privilégios elevados. 

1. [!INCLUDE [Install the HGS server role](../../includes/guarded-fabric-install-hgs-server-role.md)]

2. Defina o resolvedor de cliente DNS para apontar para o servidor HGS primário para que ele possa resolver o nome de domínio HGS. Se você estiver usando o servidor com a experiência desktop, poderá fazer isso no centro de rede e compartilhamento. No Server Core, você pode usar a ferramenta **sconfig. exe** , a opção 8 ou o **set-DnsClientServerAddress** para definir o endereço DNS. 

3. Em seguida, promoveremos esse servidor para um controlador de domínio. Você precisará de credenciais de administrador de domínio para concluir essa tarefa e será solicitado a inserir uma senha do modo de reparo dos serviços de diretório depois de executar o comando. 

   ```powershell
   $HgsDomainName = 'bastion.local' 
   $HgsDomainCredential = Get-Credential 
 
   Install-HgsServer -HgsDomainName $HgsDomainName -HgsDomainCredential $HgsDomainCredential -Restart 
   ```

4. [!INCLUDE [Initialize HGS](../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

## <a name="configure-hgs-for-https"></a>Configurar o HGS para HTTPS 

Por padrão, quando você inicializar o servidor HGS, ele configurará os sites da Web do IIS para comunicações somente HTTP.

> [!NOTE]
> É necessário configurar o HTTPS usando um certificado de servidor HGS conhecido e confiável para evitar ataques man-in-the-middle e, portanto, é aconselhado para implantações de produção.

[!INCLUDE [Configure HTTPS](../../includes/configure-hgs-for-https.md)] 

> [!NOTE]
> Para Always Encrypted com o Secure enclaves, o certificado SSL precisa ser confiável em ambos os computadores host que executam SQL Server e os computadores que executam aplicativos cliente de banco de dados precisam entrar em contato com o HGS. 

## <a name="collect-attestation-info-from-the-host-machines"></a>Coletar informações de atestado dos computadores host

Quando o HGS é configurado, ele precisa ser configurado com informações de atestado de seus computadores host para que ele saiba quais computadores devem ser autorizados a executar computações confidenciais usando o Always Encrypted e VBS Secure enclaves. Essas etapas variam de acordo com o modo de atestado usado. 

### <a name="collect-tpm-attestation-artifacts"></a>Coletar artefatos de atestado de TPM 

Se você estiver usando o modo TPM, execute os seguintes comandos em uma sessão do PowerShell com privilégios elevados em cada máquina host para instalar o suporte para atestado e coletar as informações necessárias para se registrar no serviço guardião de host. 

1. Para instalar o cliente HGS em seu computador host, instale o recurso de host protegido, que também instalará o Hyper-V. 
   Embora você não esteja executando VMs neste computador, o hipervisor é necessário para habilitar os recursos de segurança baseados em virtualização que isolam VBS enclaves.

   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName HostGuardian -All 
   ```

2. Reinicie o computador quando for solicitado a concluir a instalação do Hyper-V. 
3. Compor uma política de integridade de código para restringir o software que pode ser executado no sistema. 
   Qualquer política de controle de aplicativos do Windows Defender é suficiente. 
   Se você estiver apenas executando software da Microsoft no servidor, os comandos a seguir criarão rapidamente uma política para você. 
   A política estará no modo de auditoria, o que significa que ele registrará um evento sobre o código não autorizado, mas não o impedirá de ser executado.  

   ```powershell
   mkdir C:\artifacts
   Copy-Item C:\Windows\Schemas\CodeIntegrity\ExamplePolicies\AllowMicrosoft.xml C:\artifacts\AllowMicrosoft-Audit.xml 
   Set-RuleOption -FilePath C:\artifacts\AllowMicrosoft-Audit.xml -Option 3 
   ConvertFrom-CIPolicy -XmlFilePath C:\artifacts\AllowMicrosoft-Audit.xml -BinaryFilePath C:\artifacts\AllowMicrosoft-Audit.bin 
   Copy-Item C:\artifacts\AllowMicrosoft-Audit.bin C:\Windows\System32\CodeIntegrity\SIPolicy.p7b 
   Restart-Computer 
   ```

   O controle de aplicativos do Windows Defender tem vários recursos para abranger uma variedade de posturas de segurança. 
   Se você precisar permitir software não Microsoft ou personalizar a política padrão, se o guia de [implantação do controle de aplicativos do Windows Defender](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control-deployment-guide).   


4. Verifique se a segurança baseada em virtualização está em execução no computador com o comando a seguir. 
   Você saberá que VBS está em execução se o campo DeviceGuardSecurityServicesRunning tiver "HypervisorEnforcedCodeIntegrity" listado nele. 
   Se não estiver em execução, baixe a [ferramenta de preparação do Device Guard](https://www.microsoft.com/download/details.aspx?id=53337) e execute "DG_Readiness. ps1-Enable-política HVAC" para habilitá-la.  
   
   ```powershell
   Get-ComputerInfo -Property DeviceGuard* 
   ```

5. Coletar o identificador e a linha de base do TPM:

   ```powershell 
   (Get-PlatformIdentifier -Name $env:computername).Save("C:\artifacts\TpmID-$env:computername.xml") 
   Get-HgsAttestationBaselinePolicy -SkipValidation -Path "C:\artifacts\TpmBaseline-$env:computername.tcglog" 
   ```
   
6. Copie os arquivos XML, tcglog e bin de C:\artifacts para o servidor HGS.
7. Se esse for o primeiro host do TPM que você está adicionando ao servidor HGS, você precisará instalar os certificados confiáveis de raiz do TPM em cada servidor HGS. 
   Siga as [orientações na documentação do HgS](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-install-trusted-tpm-root-certificates) para concluir esta etapa.
8. No servidor HGS, autorize este host a passar o atestado. 
   Os scripts a seguir pressupõem que os 3 arquivos foram copiados para C:\temp no servidor HGS e que o nome do computador do seu servidor seja "servername". 
   Ajuste os caminhos e nomes para corresponder ao seu próprio ambiente. 

   ```powershell
   Add-HgsAttestationTpmHost -Name ServerA -Path C:\temp\TpmID-ServerA.xml 
   Add-HgsAttestationTpmPolicy -Name ServerA-Baseline -Path C:\temp\TpmBaseline-ServerA.tcglog 
   Add-HgsAttestationCiPolicy -Name AllowMicrosoft-Audit -Path C:\temp\AllowMicrosoft-Audit.bin 
   ```

9. Seu primeiro servidor agora está pronto para atestar! 
   No computador host, execute o seguinte comando para dizer a ele onde atestar (alterar o nome DNS para o seu cluster HGS, normalmente, você usará o nome do serviço HGS combinado com o nome de domínio HGS). 
   Se você receber um erro HostUnreachable, certifique-se de que você possa resolver e executar ping nos nomes DNS dos seus servidores HGS. 

   ```powershell
   Set-HgsClientConfiguration -AttestationServerUrl https://hgs.bastion.local/Attestation -KeyProtectionServerUrl https://hgs.bastion.local/KeyProtection/ 
   ```

10. O resultado do comando acima deve mostrar que AttestationStatus = Passed. Se não tiver, consulte [falhas de atestado](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-hosts#attestation-failures) para obter orientação sobre como resolver o erro.   
11. Repita as etapas de 1-10 para cada máquina host. 
    Se você estiver usando um hardware idêntico, não será necessário capturar uma nova política de linha de base ou CI para cada computador. 
    A linha de base do seu primeiro servidor abrangerá todos os computadores configurados de forma idêntica e a política de CI poderá ser reutilizada em vários computadores, desde que o software da Microsoft seja o único software na máquina.

### <a name="collecting-host-keys"></a>Coletando chaves de host 

> [!NOTE] 
> O atestado de chave de host é recomendado apenas para uso em ambientes de teste. O atestado de TPM fornece as garantias mais fortes que VBS enclaves processando seus dados confidenciais em SQL Server estão executando código confiável e os computadores são configurados com as configurações de segurança recomendadas. 

Se você optar por configurar o HGS no modo de atestado de chave de host, você precisará gerar e coletar chaves de cada máquina host e registrá-las com o serviço guardião de host. 

1. Para obter o cliente HGS instalado no computador host, instale o recurso de host protegido, que também instalará o Hyper-V. 
   Embora você não esteja executando VMs neste computador, o hipervisor é necessário para habilitar os recursos de segurança baseados em virtualização que isolam os enclaves VBS que executam Always Encrypted consultas. 

   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName HostGuardian -All 
   ```

2. Reinicie o computador quando for solicitado a concluir a instalação do Hyper-V.
3. Gere uma chave de host exclusiva e exporte a chave pública resultante para um arquivo. 

   ```powershell
   Set-HgsClientHostKey 
   mkdir C:\artifacts 
   Get-HgsClientHostKey -Path C:\artifacts\$env:computername.cer 
   ```
   
   Como alternativa, você pode especificar uma impressão digital se quiser usar seu próprio certificado. 
   Isso pode ser útil se você quiser compartilhar um certificado em vários computadores ou usar um certificado associado a um TPM ou a um HSM. Aqui está um exemplo de criação de um certificado associado ao TPM (que impede que ele tenha a chave privada roubada e usada em outro computador e requer apenas um TPM 1,2):

   ```powershell
   $tpmBoundCert = New-SelfSignedCertificate -Subject “Host Key Attestation ($env:computername)” -Provider “Microsoft Platform Crypto Provider”
   Set-HgsClientHostKey -Thumbprint $tpmBoundCert.Thumbprint
   ```

4. Copie a chave de host para o serviço guardião de host. 
5. Registre a chave de host com qualquer nó HGS, usando um nome e um caminho relevantes para o seu ambiente: 

   ```powershell
   Add-HgsAttestationHostKey -Name 'ServerA' -Path C:\temp\ServerA.cer 
   ``` 

6. Seu primeiro servidor agora está pronto para atestar! 
   No computador host, execute o seguinte comando para dizer a ele onde atestar (alterar o nome DNS para o seu cluster HGS, normalmente, você usará o nome do serviço HGS combinado com o nome de domínio HGS). 
   Se você receber um erro HostUnreachable, certifique-se de que você possa resolver e executar ping nos nomes DNS dos seus servidores HGS.    

   ```powershell
   Set-HgsClientConfiguration -AttestationServerUrl http://hgs.bastion.local/Attestation -KeyProtectionServerUrl http://hgs.bastion.local/KeyProtection/  
   ```

7. O resultado do comando acima deve mostrar que AttestationStatus = Passed. 
   Se você receber um erro HostUnreachable, isso significa que a máquina host não pode se comunicar com o HGS. 
   Verifique se a resolução DNS está configurada entre o computador host e os servidores HGS e se você pode executar o ping nos servidores. 
   Um erro UnauthorizedHost indica que a chave pública não foi registrada com o servidor HGS – Repita as etapas 4 e 5 para resolver o erro. 
   Se todos os outros falharem, execute Clear-HgsClientHostKey e repita as etapas 3-6.   

8. Repita as etapas 1-7 para cada servidor que executará SQL Server Always Encrypted usando VBS enclaves.     


