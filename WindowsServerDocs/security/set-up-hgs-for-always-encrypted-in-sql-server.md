---
title: Configurando o serviço guardião de Host para Always Encrypted no SQL Server
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 11/03/2018
ms.openlocfilehash: 70f6f8c2db742361deecaa216b053d8b1d057a3d
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812607"
---
# <a name="setting-up-the-host-guardian-service-for-always-encrypted-with-secure-enclaves-in-sql-server"></a>Como configurar o serviço guardião de Host para o Always Encrypted com enclaves seguros no SQL Server 

>Aplica-se a: Windows Server (canal semestral), Windows Server de 2019, visualização de 2019 do SQL Server
 
[Always Encrypted com enclaves seguros](https://docs.microsoft.com/sql/relational-databases/security/encryption/always-encrypted-enclaves) no SQL Server 2019 preview é um recurso projetado para habilitar cálculos confidenciais em dados confidenciais armazenados em um banco de dados. O serviço de guardião de Host (HGS) desempenha um papel importante em manter seus dados seguros quando um enclave seguro, configurado para sempre criptografado, é um enclave de memória de segurança baseada em virtualização (VBS). A segurança de um enclave de memória VBS depende a segurança do hipervisor do Windows e, mais amplamente, a segurança do computador que hospeda o SQL Server. 

Portanto, antes do enclave de memória VBS usado para o Always Encrypted para executar cálculos em dados confidenciais permite que um aplicativo de cliente do banco de dados, o aplicativo deve atestar com um HGS confiável. Certificação comprova o computador que hospeda o SQL Server, que contém o enclave, está no estado correto e pode ser confiável. Para o restante deste tópico, nos referimos a máquina que hospeda o SQL Server que simplesmente a máquina host.

Há dois modos mutuamente exclusivos para o aplicativo atestar: 

- Atestado de chaves do host autoriza um host, provando que ela possui uma chave privada conhecida e confiável. Atestado de chaves do host e um computador host físico ou uma máquina virtual de geração 2 executando o SQL Server é recomendada para ambientes de pré-produção.
- Atestado de TPM valida as medidas de hardware para certificar-se de que um host executa somente os binários corretos e as políticas de segurança. Atestado de TPM e uma máquina de host físico (não uma máquina virtual) executando o SQL Server é recomendado para ambientes de produção.

Para obter mais informações sobre o serviço guardião de Host e o que ele pode medir, consulte [protegidos malha e blindadas visão geral de VMs](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms). Observe que embora os documentos de falarem sobre as VMs blindadas, as mesmas proteções, arquitetura e práticas recomendadas se aplicam ao SQL Server Always Encrypted usando o enclaves VBS. 

Este artigo ajudará você a configurar o HGS em uma configuração recomendada. 

## <a name="prerequisites"></a>Pré-requisitos 

Esta seção aborda os pré-requisitos para HGS e hospedam máquinas. 

### <a name="hgs-servers"></a>Servidores HGS

- servidores de 1 a 3 para executar o HGS. 

  > [!NOTE]
  > Servidor do HGS apenas 1 é necessário para um ambiente de teste ou de pré-produção.

  Esses servidores devem ser protegidos com cuidado, pois elas controlam quais computadores podem executar suas instâncias do SQL Server usando o Always Encrypted com enclaves seguras. 
  É recomendável que administradores diferentes gerenciam o cluster HGS e que você execute o HGS no hardware físico isolado do restante da sua infraestrutura ou em malhas de virtualização separado ou assinaturas do Azure.

  - 2019 do Windows Server Standard ou Datacenter edition.
  - 2 CPUs
  - 8GB DE RAM
  - 100GB de armazenamento

  Você pode usar o [canal de manutenção em longo prazo (LTSC)](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#long-term-servicing-channel-ltsc) ou o [canal semestral](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#semi-annual-channel). 
  Para registrar e baixar o Windows Server Insider Preview, consulte [guia de Introdução com o programa do Windows Insider](https://insider.windows.com/for-business-getting-started-server/).

- Escolha um nome para a nova floresta do Active Directory criado pelo serviço guardião de Host. 
  HGS não deve estar associado ao domínio corporativo existente e deve ter administradores separados gerenciá-lo.   

- Firewall e regras de roteamento para permitir que o HTTP de entrada (TCP 80) ou o tráfego HTTPS (TCP 443) em nós do serviço guardião de Host: 

  - Os computadores executando o SQL Server
  - Os computadores que executam aplicativos de cliente de banco de dados (como servidores web) que emitem consultas de banco de dados e usam o Always Encrypted com enclaves seguras. 

### <a name="sql-server-host-machines"></a>Máquinas de host do SQL Server

- Instância do SQL Server deve ser executado em um computador que atenda aos seguintes requisitos:

  - Windows: 
    - Windows 10 Enterprise, version 1809  
    - 2019 Datacenter do Windows Server (canal semestral), versão 1809
    - Windows Server 2019 Datacenter
  - Máquina física para produção, ou uma máquina virtual de geração 2 para teste (Observe que o Azure não dá suporte de VMs da geração 2)
  - Requisitos gerais listados na [Hardware and Software Requirements for Installing SQL Server](https://docs.microsoft.com/sql/sql-server/install/hardware-and-software-requirements-for-installing-sql-server?view=sql-server-2017).   

  Você pode usar o [canal de manutenção em longo prazo (LTSC)](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#long-term-servicing-channel-ltsc) ou o [canal semestral](https://docs.microsoft.com/windows-server/get-started/semi-annual-channel-overview#semi-annual-channel). 
  Para registrar e baixar o Windows Server Insider Preview, consulte [guia de Introdução com o programa do Windows Insider](https://insider.windows.com/for-business-getting-started-server/).

- Requisitos específicos para o modo de Atestado escolhido:
  - **Modo TPM** é o modo de Atestado mais forte e usará um confiável Platform Module (TPM) para validar criptograficamente que suas máquinas de host são conhecidas para o seu datacenter (usando uma ID exclusiva de cada TPM), em execução confiável de hardware e firmware configurações (usando uma linha de base TPM) e executando o kernel confiável e o código do modo de usuário (usando o Windows Defender Application Control). O seguinte hardware é necessária para usar o modo TPM: 
    - Módulo TPM 2.0 instalado e habilitado 
    - Inicialização segura habilitada com a política de inicialização segura do Microsoft (não habilitar a política de autoridade de certificação de inicialização segura de terceiros 3ª ou as políticas personalizadas)
    - IOMMU (Intel VT-d ou IOV AMD) para evitar ataques de acesso de memória direta 

  - **Modo de chave do host** usa um par de chaves assimétricas (muito semelhante as chaves SSH) para identificar e autorizar os computadores host que deseja executar o SQL Server. Esse modo é mais fácil de configurar e não tem requisitos de hardware específicos, mas não verificará o software ou firmware em execução nas máquinas host.  

Para verificar se o TPM é compatível, execute os seguintes comandos na máquina host em que você pretende executar o SQL Server usando o Always Encrypted com enclaves seguras. "2.0" deve aparecer na lista de SpecVersions com suporte para que você possa usar o atestado de TPM:

```powershell
Get-CimInstance -ClassName Win32_Tpm -Namespace root/cimv2/Security/MicrosoftTpm 
```

## <a name="set-up-the-first-hgs-node"></a>Configurar o primeiro nó HGS 

Serviço guardião de Host opera em uma configuração altamente disponível usando um cluster de 3 nós. É recomendável que você configure um nó completamente antes de adicionar outros nós. 

Execute todos os comandos a seguir em uma sessão do PowerShell com privilégios elevados.

1. [!INCLUDE [Install the HGS server role](../../includes/guarded-fabric-install-hgs-server-role.md)]

2. [!INCLUDE [Install HGS by default](../../includes/install-hgs-default.md)] 

3. [!INCLUDE [Determine a DNN](../../includes/guarded-fabric-initialize-hgs-default-step-one.md)]

   Para Always Encrypted com enclaves seguros, os computadores host que executam o SQL Server e máquinas que executam aplicativos de cliente de banco de dados, que ambos precisam entrar em contato com o HGS, embora somente os computadores host exigem Atestado.

4. Após reiniciar o computador, HGS será instalado e o servidor também será um controlador de domínio com o Active Directory configurado. 
   Durante a configuração do Active Directory, a conta de administrador do computador local é adicionada ao grupo Admins. do domínio e somente os membros desse grupo podem entrar para um controlador de domínio.
   Entrar usando a conta de administrador do domínio (por exemplo, administrator@bastion.local ou bastion.local\administrator) ou crie outra conta de administrador de domínio para entrar e, em seguida, configure o serviço de Atestado.
   Você precisará escolher atestado de chaves do TPM ou host e executar o comando correspondente. 
   Para o HgsServiceName, especifique a DNN escolhido.

   Para o modo TPM:

   ```powershell
   Initialize-HgsAttestation -HgsServiceName 'hgs' -TrustTpm
   ```

   Para o modo de chave do host:

   ```powershell
   Initialize-HgsAttestation -HgsServiceName 'hgs' -TrustHostKey 
   ```

5. Certifique-se de que as máquinas de host que executam o SQL Server será capazes de resolver os nomes DNS dos novos membros do domínio HGS ao configurar um encaminhador de seus servidores DNS corporativos para o novo controlador de domínio do HGS. Se você estiver usando o DNS do Windows Server, você pode configurar um encaminhador condicional, executando os seguintes comandos em um console do PowerShell com privilégios elevados em um servidor DNS em sua organização. Substitua os nomes e endereços na sintaxe do Windows PowerShell abaixo conforme necessário para o seu ambiente. Depois de adicionar mais nós HGS, execute este comando novamente para adicioná-los como servidores mestres.

   ```powershell
   Add-DnsServerConditionalForwarderZone -Name <HGS domain name> -ReplicationScope "Forest" -MasterServers <IP addresses of HGS servers>
   ```

## <a name="set-up-additional-hgs-nodes-for-production-deployments"></a>Configurar nós adicionais do HGS para implantações de produção

Para adicionar nós ao cluster, execute os seguintes comandos em uma sessão do PowerShell com privilégios elevados. 

1. [!INCLUDE [Install the HGS server role](../../includes/guarded-fabric-install-hgs-server-role.md)]

2. Defina o resolvedor de cliente DNS para apontar para seu servidor HGS primário para que ele possa resolver o nome de domínio do HGS. Se você estiver usando o servidor com experiência Desktop, você pode fazer isso na Central de rede e compartilhamento. No Server Core, você pode usar o **sconfig.exe** ferramenta, a opção 8, ou **Set-DnsClientServerAddress** para definir o endereço DNS. 

3. Em seguida, podemos será promover este servidor a um controlador de domínio. Você precisará de credenciais de administrador de domínio para concluir essa tarefa e será solicitado a inserir uma senha de modo de reparo de serviços de diretório depois de executar o comando. 

   ```powershell
   $HgsDomainName = 'bastion.local' 
   $HgsDomainCredential = Get-Credential 
 
   Install-HgsServer -HgsDomainName $HgsDomainName -HgsDomainCredential $HgsDomainCredential -Restart 
   ```

4. [!INCLUDE [Initialize HGS](../../includes/guarded-fabric-initialize-hgs-on-the-node.md)] 

## <a name="configure-hgs-for-https"></a>Configurar o HGS para HTTPS 

Por padrão, quando você inicializa o servidor HGS será configurar os sites do IIS para comunicações HTTP-only.

> [!NOTE]
> Configurar HTTPS usando um certificado de servidor HGS bem conhecido e confiável, é necessário para impedir ataques man-in-the-middle e, portanto, é recomendável para implantações de produção.

[!INCLUDE [Configure HTTPS](../../includes/configure-hgs-for-https.md)] 

> [!NOTE]
> Para o Always Encrypted com enclaves seguras, o certificado SSL precisa ser confiável em ambos os computadores host que executam o SQL Server e máquinas que executam aplicativos de cliente do banco de dados precisam entrar em contato com o HGS. 

## <a name="collect-attestation-info-from-the-host-machines"></a>Coletar informações de atestado de computadores host

Depois que o HGS estiver configurado, ele precisa ser configurado com as informações de atestado de suas máquinas de host para que ele saiba quais máquinas devem ser autorizadas a executar cálculos confidenciais usando sempre criptografados e VBS enclaves seguras. Essas etapas variam com base em qual modo de Atestado usado. 

### <a name="collect-tpm-attestation-artifacts"></a>Coletar os artefatos de atestado de TPM 

Se você estiver usando o modo TPM, execute os seguintes comandos em uma sessão elevada do PowerShell em cada computador host para instalar o suporte para Atestado e coletar as informações que você precisará registrar com o serviço guardião de Host. 

1. Para instalar o cliente HGS no seu computador host, instale o recurso de Host protegido, que também instalará o Hyper-V. 
   Embora você não estiver executando máquinas virtuais neste computador, o hipervisor é necessário para habilitar os recursos de segurança baseada em virtualização para isolar enclaves VBS.

   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName HostGuardian -All 
   ```

2. Reinicie o computador quando for solicitado a concluir a instalação do Hyper-V. 
3. Uma política de integridade de código para restringir qual software pode ser executado no sistema de composição. 
   Qualquer política de controle de aplicativo do Windows Defender é suficiente. 
   Se você estiver executando apenas o software da Microsoft no servidor, os comandos a seguir serão rapidamente criar uma política para você. 
   A política será no modo de auditoria, o que significa que ele registrará um evento sobre o código não autorizado, mas não impedirá que ele seja executado.  

   ```powershell
   mkdir C:\artifacts
   Copy-Item C:\Windows\Schemas\CodeIntegrity\ExamplePolicies\AllowMicrosoft.xml C:\artifacts\AllowMicrosoft-Audit.xml 
   Set-RuleOption -FilePath C:\artifacts\AllowMicrosoft-Audit.xml -Option 3 
   ConvertFrom-CIPolicy -XmlFilePath C:\artifacts\AllowMicrosoft-Audit.xml -BinaryFilePath C:\artifacts\AllowMicrosoft-Audit.bin 
   Copy-Item C:\artifacts\AllowMicrosoft-Audit.bin C:\Windows\System32\CodeIntegrity\SIPolicy.p7b 
   Restart-Computer 
   ```

   Windows Defender Application Control tem vários recursos para cobrir uma variedade de postures de segurança. 
   Se você precisar permitir que o software não Microsoft ou personalizar a política padrão, se o [guia de implantação do Windows Defender Application Control](https://docs.microsoft.com/windows/security/threat-protection/windows-defender-application-control/windows-defender-application-control-deployment-guide).   


4. Verifique se que a segurança baseada em virtualização está em execução em seu computador com o comando a seguir. 
   Você saberá que o VBS está em execução se o campo DeviceGuardSecurityServicesRunning "HypervisorEnforcedCodeIntegrity" listado nele. 
   Se ele não está em execução, baixe o [ferramenta de preparação do dispositivo Guard](https://www.microsoft.com/download/details.aspx?id=53337) e execute "DG_Readiness.ps1-habilitar - HVAC" para habilitá-lo.  
   
   ```powershell
   Get-ComputerInfo -Property DeviceGuard* 
   ```

5. Colete o identificador do TPM e a linha de base:

   ```powershell 
   (Get-PlatformIdentifier -Name $env:computername).Save("C:\artifacts\TpmID-$env:computername.xml") 
   Get-HgsAttestationBaselinePolicy -SkipValidation -Path "C:\artifacts\TpmBaseline-$env:computername.tcglog" 
   ```
   
6. Copie o xml, tcglog e arquivos binários do C:\artifacts ao seu servidor HGS.
7. Se esse for o primeiro host TPM que você está adicionando ao servidor HGS, você precisará instalar os certificados de raiz de TPM confiável em cada servidor HGS. 
   Siga as [orientações sobre a documentação do HGS](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-install-trusted-tpm-root-certificates) para concluir esta etapa.
8. No servidor HGS, autorize esse host ao passar o Atestado. 
   Os scripts a seguir pressupõem 3 arquivos foram copiados para C:\temp no servidor HGS e que o nome do computador do servidor é "O servidor". 
   Ajuste os caminhos e nomes para corresponder ao seu próprio ambiente. 

   ```powershell
   Add-HgsAttestationTpmHost -Name ServerA -Path C:\temp\TpmID-ServerA.xml 
   Add-HgsAttestationTpmPolicy -Name ServerA-Baseline -Path C:\temp\TpmBaseline-ServerA.tcglog 
   Add-HgsAttestationCiPolicy -Name AllowMicrosoft-Audit -Path C:\temp\AllowMicrosoft-Audit.bin 
   ```

9. Seu primeiro servidor agora está pronto para atestar! 
   No computador host, execute o seguinte comando para instruí-lo onde atestar (alterar que o nome DNS do seu cluster HGS, normalmente você usará o nome do serviço de HGS combinado com o nome de domínio do HGS). 
   Se você receber um erro de HostUnreachable, verifique se você pode resolver e executar o ping de nomes DNS dos seus servidores HGS. 

   ```powershell
   Set-HgsClientConfiguration -AttestationServerUrl https://hgs.bastion.local/Attestation -KeyProtectionServerUrl https://hgs.bastion.local/KeyProtection/ 
   ```

10. O resultado do comando acima deve mostrar que AttestationStatus = aprovado. Se isso não acontecer, consulte [falhas de Atestado](https://docs.microsoft.com/windows-server/virtualization/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-hosts#attestation-failures) para obter orientação sobre como resolver o erro.   
11. Repita as etapas 1 a 10 para cada computador host. 
    Se você estiver usando o hardware idêntico, não será preciso capturar uma nova linha de base ou a política de CI para cada computador. 
    A linha de base do seu primeiro servidor abrangerá todos os computadores configurados de forma idêntica e a política de CI pode ser reutilizada em vários computadores, desde que softwares da Microsoft é o único software no computador.

### <a name="collecting-host-keys"></a>Coletando chaves de Host 

> [!NOTE] 
> Atestado de chaves do host só é recomendado para uso em ambientes de teste. Atestado de TPM fornece as garantias mais fortes que enclaves VBS processando seus dados confidenciais no SQL Server estão executando código confiável e os computadores sejam configurados com as configurações de segurança recomendadas. 

Se você optar por configurar o HGS no modo de atestado de chaves do host, você precisará gerar e coletar as chaves de cada computador host e registrá-los com o serviço guardião de Host. 

1. Para obter o cliente HGS instalado no computador host, instale o recurso de Host protegido, que também instalará o Hyper-V. 
   Enquanto você não estiver executando máquinas virtuais neste computador, o hipervisor é necessário para habilitar os recursos de segurança baseada em virtualização isolar os enclaves VBS que executam consultas Always Encrypted. 

   ```powershell
   Enable-WindowsOptionalFeature -Online -FeatureName HostGuardian -All 
   ```

2. Reinicie o computador quando for solicitado a concluir a instalação do Hyper-V.
3. Gere uma chave de host exclusivo e exportar a chave pública resultante para um arquivo. 

   ```powershell
   Set-HgsClientHostKey 
   mkdir C:\artifacts 
   Get-HgsClientHostKey -Path C:\artifacts\$env:computername.cer 
   ```
   
   Como alternativa, você pode especificar uma impressão digital, se você quiser usar seu próprio certificado. 
   Isso pode ser útil se você quiser compartilhar um certificado em vários computadores ou usar um certificado associado a um TPM ou um HSM. Aqui está um exemplo de como criar um certificado de TPM associado (que impede de ter a chave privada roubado e usado em outro computador e requer um TPM 1.2):

   ```powershell
   $tpmBoundCert = New-SelfSignedCertificate -Subject “Host Key Attestation ($env:computername)” -Provider “Microsoft Platform Crypto Provider”
   Set-HgsClientHostKey -Thumbprint $tpmBoundCert.Thumbprint
   ```

4. Copie a chave de host para o serviço guardião de Host. 
5. Registre a chave do host com qualquer nó HGS, usando um nome relevante e o caminho para o seu ambiente: 

   ```powershell
   Add-HgsAttestationHostKey -Name 'ServerA' -Path C:\temp\ServerA.cer 
   ``` 

6. Seu primeiro servidor agora está pronto para atestar! 
   No computador host, execute o seguinte comando para instruí-lo onde atestar (alterar que o nome DNS do seu cluster HGS, normalmente você usará o nome do serviço de HGS combinado com o nome de domínio do HGS). 
   Se você receber um erro de HostUnreachable, verifique se você pode resolver e executar o ping de nomes DNS dos seus servidores HGS.    

   ```powershell
   Set-HgsClientConfiguration -AttestationServerUrl http://hgs.bastion.local/Attestation -KeyProtectionServerUrl http://hgs.bastion.local/KeyProtection/  
   ```

7. O resultado do comando acima deve mostrar que AttestationStatus = aprovado. 
   Se você receber um erro de HostUnreachable, isso significa que sua máquina host não pode se comunicar com o HGS. 
   Certifique-se de que a resolução de DNS é configurada entre o computador host e os servidores HGS e você pode executar ping nos servidores. 
   Um erro de UnauthorizedHost indica que a chave pública não foi registrada com o servidor HGS – Repita as etapas 4 e 5 para resolver o erro. 
   Se todo o resto falhar, execute o Clear-HgsClientHostKey e repita as etapas 3 a 6.   

8. Repita as etapas 1 a 7 para cada servidor que será executado com o SQL Server Always Encrypted usando o enclaves VBS.     


