---
title: Gerenciando o serviço guardião de Host
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: eecb002e-6ae5-4075-9a83-2bbcee2a891c
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.openlocfilehash: dab27e71e42970507f321271edda90f6d161c691
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447395"
---
# <a name="managing-the-host-guardian-service"></a>Gerenciando o serviço guardião de Host

> Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

O serviço de guardião de Host (HGS) é o ponto central da solução de malha protegida.
Ele é responsável por garantir que os hosts do Hyper-V na malha do são conhecidos como o enterprise ou o hoster e executando um software confiável e para gerenciar as chaves usadas para iniciar o backup de VMs blindadas.
Quando um locatário decide confiar a você para hospedar suas VMs blindadas, eles estão colocando a confiança em sua configuração e gerenciamento do serviço guardião de Host.
Portanto, é muito importante seguir as práticas recomendadas ao gerenciar o serviço guardião de Host para garantir a segurança, disponibilidade e confiabilidade de sua malha protegida.
As diretrizes nas seções a seguir resolve problemas operacionais mais comuns voltada para administradores do HGS.

## <a name="limiting-admin-access-to-hgs"></a>Limitando o acesso de administrador ao HGS
Devido à natureza confidencial de segurança de HGS, é importante garantir que os administradores são altamente confiáveis membros de sua organização e, idealmente, separam dos administradores de seus recursos de malha.
Além disso, é recomendável que você só gerenciar HGS de estações de trabalho seguras usando protocolos de comunicação segura, como o WinRM por HTTPS.

### <a name="separation-of-duties"></a>Separação de funções
Ao configurar o HGS, você tem a opção de criar uma floresta do Active Directory isolada para o HGS ou para unir HGS a um domínio existente e confiável.
Essa decisão, bem como as funções que você atribua os administradores em sua organização, determinam o limite de confiança para HGS.
Quem tem acesso ao HGS, seja diretamente como um administrador ou indiretamente como um administrador de outra coisa (por exemplo, Active Directory) que pode influenciar o HGS, tem controle sobre sua malha protegida.
Os administradores HGS escolher quais hosts Hyper-V estão autorizados a executar VMs blindadas e gerenciar os certificados necessários para iniciar o backup de VMs blindadas.
Um invasor ou administrador mal-intencionado quem tem acesso ao HGS pode usar esse poder para autorizar hosts comprometidos para executar VMs blindadas, iniciar um ataque de negação de serviço, removendo o material da chave e muito mais.

Para evitar esse risco, vale *fortemente* recomendável que você limite a sobreposição entre os administradores de sua HGS (incluindo o domínio ao qual o HGS está associado) e ambientes Hyper-V.
Ao garantir que não há um administrador tem acesso a ambos os sistemas, um invasor precisaria comprometer contas diferentes 2 de 2 indivíduos para concluir sua missão para alterar as políticas HGS.
Isso também significa que os administradores de domínio e de empresa para os dois ambientes do Active Directory não devem ser a mesma pessoa, nem HGS deve usar a mesma floresta do Active Directory como seus hosts Hyper-V.
Qualquer pessoa que pode conceder a mesmos acesso aos recursos mais representa um risco de segurança.

### <a name="using-just-enough-administration"></a>Usando a administração Just Enough
Acompanha o HGS [Just Enough Administration](https://aka.ms/JEAdocs) funções (JEA) integradas para ajudá-lo a gerenciá-lo com mais segurança.
JEA ajuda, permitindo que você a delegar tarefas de administração para usuários não administradores, que significa que as pessoas que gerenciam as políticas de HGS, na verdade, não precisam ser administradores do domínio ou computador inteiro.
JEA funciona por meio de limitar quais comandos um usuário pode executar em uma sessão do PowerShell e usando uma conta local temporária em segundo plano (exclusivo para cada sessão de usuário) para executar os comandos que normalmente exigem elevação.

HGS é fornecido com 2 funções JEA pré-configurada:
- **Os administradores de HGS** que permite aos usuários gerenciar todas as políticas HGS, incluindo autorizando novos hosts para executar VMs blindadas.
- **Os revisores de HGS** apenas que permite aos usuários o direito de auditoria de políticas existentes. Eles não podem fazer todas as alterações na configuração de HGS.

Para usar o JEA, você precisa primeiro criar um novo usuário padrão e torná-los um membro do grupo de revisores HGS ou admins. do HGS.
Se você usou `Install-HgsServer` para configurar uma nova floresta para HGS, esses grupos serão nomeados "*servicename*administradores" e "*servicename*revisores", respectivamente, onde *servicename*  é o nome de rede do cluster HGS.
Se você tiver ingressado HGS a um domínio existente, você deve consultar os nomes de grupo que você especificou no `Initialize-HgsServer`.

**Criar usuários padrão para as funções de administrador e revisor HGS**

```powershell
$hgsServiceName = (Get-ClusterResource HgsClusterResource | Get-ClusterParameter DnsName).Value
$adminGroup = $hgsServiceName + "Administrators"
$reviewerGroup = $hgsServiceName + "Reviewers"

New-ADUser -Name 'hgsadmin01' -AccountPassword (Read-Host -AsSecureString -Prompt 'HGS Admin Password') -ChangePasswordAtLogon $false -Enabled $true
Add-ADGroupMember -Identity $adminGroup -Members 'hgsadmin01'

New-ADUser -Name 'hgsreviewer01' -AccountPassword (Read-Host -AsSecureString -Prompt 'HGS Reviewer Password') -ChangePasswordAtLogon $false -Enabled $true
Add-ADGroupMember -Identity $reviewerGroup -Members 'hgsreviewer01'
```

**Políticas de auditoria com a função do revisor**

Em um computador remoto que tenha conectividade de rede com o HGS, execute os seguintes comandos do PowerShell para inserir a sessão JEA com as credenciais do revisor.
É importante observar que, como a conta de revisor é apenas um usuário padrão, ele não pode ser usado para comunicação remota regular do Windows PowerShell, acesso de área de trabalho remota para HGS, etc.

```powershell
Enter-PSSession -ComputerName <hgsnode> -Credential '<hgsdomain>\hgsreviewer01' -ConfigurationName 'microsoft.windows.hgs'
```

Você então poderá marcar os comandos que são permitidos na sessão com `Get-Command` e execute os comandos permitidos para a configuração de auditoria.
No exemplo abaixo, estamos verificando quais políticas estão habilitadas no HGS.

```powershell
Get-Command

Get-HgsAttestationPolicy
```

Digite o comando `Exit-PSSession` ou seu alias, `exit`, quando você terminar de trabalhar com a sessão JEA. 

**Adicionar uma nova política para usar a função de administrador HGS**

Para alterar uma política, você precisa para se conectar ao ponto de extremidade JEA com uma identidade que pertence ao grupo 'hgsAdministrators'.
No exemplo abaixo, mostramos como você pode copiar uma nova política de integridade de código para HGS e registrá-lo usando o JEA.
A sintaxe pode ser diferente do que você está acostumado.
Isso é para acomodar algumas das restrições no JEA como não ter acesso ao sistema de arquivos completo.

```powershell
$cipolicy = Get-Item "C:\temp\cipolicy.p7b"
$session = New-PSSession -ComputerName <hgsnode> -Credential '<hgsdomain>\hgsadmin01' -ConfigurationName 'microsoft.windows.hgs'
Copy-Item -Path $cipolicy -Destination 'User:' -ToSession $session

# Now that the file is copied, we enter the interactive session to register it with HGS
Enter-PSSession -Session $session
Add-HgsAttestationCiPolicy -Name 'New CI Policy via JEA' -Path 'User:\cipolicy.p7b'

# Confirm it was added successfully
Get-HgsAttestationPolicy -PolicyType CiPolicy

# Finally, remove the PSSession since it is no longer needed
Exit-PSSession
Remove-PSSession -Session $session
```

## <a name="monitoring-hgs"></a>Monitoramento de HGS
### <a name="event-sources-and-forwarding"></a>Origens de eventos e encaminhamento
Eventos de HGS serão exibida no log de eventos do Windows em 2 fontes:
- **HostGuardianService-Attestation**
- **HostGuardianService-KeyProtection**

Você pode exibir esses eventos, abrindo o Visualizador de eventos e navegando até Microsoft-Windows-HostGuardianService-Atestado e Microsoft-Windows-HostGuardianService-KeyProtection.

Em um ambiente grande, geralmente é preferível para encaminhar eventos para um coletor de eventos do Windows central para facilitar a análise dos eventos.
Para obter mais informações, confira a [documentação do encaminhamento de eventos do Windows](https://msdn.microsoft.com/library/windows/desktop/bb427443.aspx).

### <a name="using-system-center-operations-manager"></a>Usando o System Center Operations Manager
Você também pode usar o System Center 2016 – Operations Manager para monitorar o HGS e seus hosts protegidos.
O pacote de gerenciamento de malha protegida tem monitores de eventos para verificar se há configurações incorretas comuns que podem levar a tempo de inatividade do datacenter, incluindo hosts não passando Atestado e servidores HGS relatando erros.

Para começar, [instalar e configurar o SCOM 2016](https://technet.microsoft.com/system-center-docs/om/welcome-to-operations-manager) e [Baixe o pacote de gerenciamento de malha protegida](https://www.microsoft.com/download/details.aspx?id=52764).
O guia do pacote de gerenciamento incluído explica como configurar o pacote de gerenciamento e compreender o escopo de seus monitores.

## <a name="backing-up-and-restoring-hgs"></a>Fazendo backup e restaurando HGS
### <a name="disaster-recovery-planning"></a>Planejamento de recuperação de desastre
Ao rascunhar seus planos de recuperação de desastres, é importante considerar os requisitos específicos do serviço guardião de Host em sua malha protegida.
Você perde alguns ou todos os nós de HGS, você pode enfrentar problemas de disponibilidade imediata que impedirão que os usuários de iniciar o backup de suas VMs blindadas.
Em um cenário onde você perder todo o cluster HGS, você precisará ter backups completos de configuração do HGS disponível para restaurar seu cluster HGS e retomar as operações normais.
Esta seção aborda as etapas necessárias para preparar para esse cenário.

Primeiro, é importante entender o que dizer sobre HGS é importante fazer backup.
HGS mantém várias partes de informações que o ajudarão a determinam quais hosts estão autorizados a executar VMs blindadas.
Isso inclui:
1. Identificadores de segurança de diretório Active Directory para os grupos que contêm trusted hosts (ao usar o Atestado do Active Directory).
2. Identificadores exclusivos de TPM de cada host no seu ambiente;
3. Políticas do TPM para cada configuração exclusiva do host; e
4. Políticas de integridade de código que determinam qual software pode ser executado nos hosts.

Esses artefatos de Atestado exigem a coordenação com os administradores da malha da hospedagem obter, potencialmente, dificultando a obter essa informação novamente após um desastre.

Além disso, o HGS requer acesso a 2 ou mais certificados usados para criptografar e assinar as informações necessárias para iniciar uma VM blindada (o protetor de chave).
Esses certificados são conhecidos (usado pelos proprietários de VMs blindadas para autorizar sua malha para executar suas VMs) e devem ser restaurados após um desastre para uma experiência perfeita de recuperação.
Você não deve restaurar HGS com os mesmos certificados após um desastre, cada VM precisa ser atualizado para autorizar seu novas chaves para descriptografar as informações.
Por motivos de segurança, somente o proprietário da VM pode atualizar a configuração da VM para autorizar a essas novas chaves, a falha de significado para restaurar as chaves após um desastre resultará em cada proprietário da VM precisar tomar medidas para obter suas VMs em execução novamente.

#### <a name="preparing-for-the-worst"></a>Preparando para o pior
Para preparar para a perda completa de HGS, existem 2 etapas que você deve executar:
1. Fazer backup das diretivas de Atestado HGS
2. Fazer backup das chaves HGS

Orientação sobre como executar essas duas etapas é fornecida na [fazendo backup de HGS](#backing-up-hgs) seção.

Ele é recomendado Além disso, mas não obrigatório, que você faça backup da lista de usuários autorizados para gerenciar HGS em seu domínio do Active Directory ou do Active Directory em si.

Backups devem ser feitos regularmente para garantir que as informações são atualizadas e armazenados com segurança para evitar violação ou roubo.

Vale **não recomendável** para fazer backup ou tente restaurar uma imagem do sistema inteiro de um nó de HGS.
No caso de você tiver perdido o seu cluster inteiro, a prática recomendada é definir um novo nó HGS e restaurar apenas o estado do HGS, não todo o servidor do sistema operacional.

#### <a name="recovering-from-the-loss-of-one-node"></a>Recuperação da perda de um nó
Se você perder um ou mais nós (mas não todos os nós) em seu cluster HGS, você pode simplesmente [adicionar nós ao cluster](guarded-fabric-configure-additional-hgs-nodes.md) seguir as orientações descritas no guia de implantação.
As políticas de Atestado sincronizará automaticamente, assim como todos os certificados que foram fornecidos para HGS como arquivos PFX acompanhada de senhas.
Para certificados adicionados ao HGS usando uma impressão digital (não exportável e com suporte de hardware certificados, comumente), você precisará garantir que cada novo nó tem acesso à chave privada de cada certificado.

#### <a name="recovering-from-the-loss-of-the-entire-cluster"></a>Recuperação da perda de todo o cluster
Se todo o cluster HGS fica inativo e não será possível colocá-lo online novamente, você precisará restaurar o HGS de um backup.
Restaurando o HGS de um backup envolve a primeira configuração de um novo cluster de HGS pela [orientação no guia de implantação](guarded-fabric-setting-up-the-host-guardian-service-hgs.md).
Ele é altamente recomendável, mas não obrigatórios, para usar o mesmo nome de cluster ao configurar o ambiente de recuperação do HGS para ajudar na resolução de nomes de hosts.
Usando o mesmo nome evita a necessidade de reconfigurar os hosts com novo URLs do Atestado e proteção de chave.
Se você restaurou os objetos para fazer o HGS de domínio do Active Directory, é recomendável que você remova os objetos que representam o cluster HGS, computadores, conta de serviço e grupos JEA antes de inicializar o servidor HGS.

Depois que você configurar o primeiro nó HGS (por exemplo, ele foi instalado e inicializado), siga os procedimentos em [HGS a restauração de um backup](#restoring-hgs-from-a-backup) para restaurar as políticas de Atestado e metades públicas da proteção de chave certificados.
Você precisará restaurar as chaves privadas para seus certificados manualmente de acordo com as diretrizes do seu provedor de certificado (por exemplo, importar o certificado no Windows, ou configurar o acesso a certificados de backup de HSM).
Após o primeiro nó é configurado, você pode continuar [instalar nós adicionais ao cluster](guarded-fabric-configure-additional-hgs-nodes.md) até atingir a capacidade e a resiliência desejada.

### <a name="backing-up-hgs"></a>Fazendo backup de HGS
O administrador HGS deve ser responsável pelo backup HGS regularmente.
Um backup completo irá conter o material da chave confidencial que deve ser protegida adequadamente.
Uma entidade não confiável deve ter acesso a essas chaves, eles podiam usar que material para configurar um ambiente de HGS mal-intencionado com a finalidade de comprometer a VMs blindadas.

**Fazendo backup das diretivas de Atestado** para fazer as políticas de Atestado HGS, execute o comando a seguir em qualquer nó de servidor do HGS de trabalho.
Você será solicitado a fornecer uma senha.
Essa senha é usada para criptografar todos os certificados adicionados ao HGS usando um arquivo PFX (em vez de uma impressão digital do certificado).

```powershell
Export-HgsServerState -Path C:\temp\HGSBackup.xml
```

> [!NOTE]
> Se você estiver usando o atestado de admin confiável, você deve backup separadamente associação nos grupos de segurança usados pelo HGS para autorizar os hosts protegidos.
> HGS fará backup somente o SID dos grupos de segurança, não a associação dentro deles.
> No caso desses grupos são perdidos durante um desastre, você precisará recriar os grupos e adicionar cada host protegido-los novamente.

**Fazendo backup de certificados**

O `Export-HgsServerState` comando fará backup de qualquer PFX certificados baseados em adicionado ao HGS no momento o comando é executado.
Se você adicionou certificados ao HGS usando uma impressão digital do (típica para certificados não exportável e com suporte de hardware), você precisará fazer backup das chaves privadas de seus certificados manualmente.
Para identificar quais certificados estão registrados com HGS e precisam ser submetido ao backup manualmente, execute o seguinte comando do PowerShell em qualquer nó de servidor do HGS de trabalho.

```powershell
Get-HgsKeyProtectionCertificate | Where-Object { $_.CertificateData.GetType().Name -eq 'CertificateReference' } | Format-Table Thumbprint, @{ Label = 'Subject'; Expression = { $_.CertificateData.Certificate.Subject } }
```

Para cada um dos certificados listados, você precisará fazer manualmente a chave privada.
Se você estiver usando um certificado baseado em software que é não exportável, entre em contato com sua autoridade de certificação para garantir que eles têm um backup do seu certificado e/ou podem reemitir sob demanda.
Para certificados criados e armazenados em módulos de segurança de hardware, você deve consultar a documentação para seu dispositivo para obter orientação sobre o planejamento de recuperação de desastres.

Você deve armazenar os backups do certificado junto com seus backups de diretiva de Atestado em um local seguro para que ambas as partes podem ser restaurados juntos.

**Configuração adicional para fazer backup**

O backup dos backup HGS o estado do servidor não incluirá o nome do cluster HGS, todas as informações do Active Directory, ou todos os certificados SSL usado para proteger as comunicações com as APIs de HGS.
Essas configurações são importantes para manter a consistência, mas não é crítica para colocar seu cluster HGS online novamente após um desastre.

Para capturar o nome do serviço HGS, execute `Get-HgsServer` e anote o nome simples no Atestado e URLs de proteção de chave.
Por exemplo, se a URL de Atestado é "<http://hgs.contoso.com/Attestation>", "hgs" é o nome do serviço HGS.

Domínio do Active Directory usado pelo HGS deve ser gerenciado como qualquer outro domínio do Active Directory.
Ao restaurar o HGS após um desastre, você não necessariamente precisará recriar os objetos exatos que estão presentes no domínio atual.
No entanto, ele facilitará recuperação se você fizer backup do Active Directory e manter uma lista de usuários JEA autorizada a gerenciar o sistema, bem como a associação de quaisquer grupos de segurança usados pelo atestado de admin confiável para autorizar os hosts protegidos.

Para identificar a impressão digital dos certificados SSL configurado para HGS, execute o seguinte comando no PowerShell.
Em seguida, você pode fazer backup desses certificados SSL acordo com as instruções do provedor de certificado.

```powershell
Get-WebBinding -Protocol https | Select-Object certificateHash
```

### <a name="restoring-hgs-from-a-backup"></a>Restauração de um backup de HGS
As etapas a seguir descrevem como restaurar as configurações de HGS de um backup.
As etapas são relevantes para ambas as situações em que você está tentando desfazer as alterações feitas para suas instâncias HGS já em execução e quando você estiver aguardando um novo cluster HGS após uma perda completa de seu um anterior.

#### <a name="set-up-a-replacement-hgs-cluster"></a>Configurar um cluster HGS de substituição
Antes de poder restaurar HGS, você precisa ter um cluster HGS inicializado para o qual você pode restaurar a configuração.
Se você simplesmente estiver importando as configurações que foram excluídas acidentalmente a um cluster existente de (em execução), você pode ignorar esta etapa.
Se você estiver recuperando de uma perda completa de HGS, você precisará instalar e inicializar o nó após de pelo menos um HGS os [orientação no guia de implantação](guarded-fabric-setting-up-the-host-guardian-service-hgs.md).

Especificamente, você precisará:
1. [Configurar o domínio HGS](guarded-fabric-choose-where-to-install-hgs.md) ou HGS Junte-se a um domínio existente
2. [Inicializar o servidor HGS](guarded-fabric-initialize-hgs.md) usando suas chaves existentes *ou* um conjunto de chaves temporárias. Você pode [remover as chaves temporárias](#renewing-or-replacing-keys) depois de importar as chaves reais do HGS arquivos de backup.
3. [Importar configurações de HGS](#import-settings-from-a-backup) do backup para restaurar os grupos de hosts confiáveis, políticas de integridade de código, linhas de base de TPM e identificadores TPM

> [!TIP]
> O novo cluster HGS não precisa usar os mesmos certificados, nome do serviço ou domínio como a instância HGS do qual o arquivo de backup foi exportado.

#### <a name="import-settings-from-a-backup"></a>Importar configurações de um backup

Para restaurar o atestado de políticas, certificados de PFX e as chaves públicas de certificados de não-PFX para o nó do HGS de um arquivo de backup, execute o seguinte comando em um nó de servidor HGS inicializado.
Você será solicitado a inserir a senha que você especificou ao criar o backup.

```powershell
Import-HgsServerState -Path C:\Temp\HGSBackup.xml
```

Se você deseja importar as políticas de atestado de admin confiável ou políticas de atestado de TPM confiável, você pode fazer isso especificando a `-ImportActiveDirectoryModeState` ou `-ImportTpmModeState` sinaliza ao [HgsServerState importação](https://technet.microsoft.com/library/mt652168.aspx).

Verifique se a atualização cumulativa mais recente para Windows Server 2016 está instalado antes de executar `Import-HgsServerState`.
Falha ao fazer isso pode resultar em um erro de importação.

> [!NOTE]
> Se você restaurar políticas em um nó HGS existente que já tem um ou mais dessas políticas instalado, o comando de importação mostrará um erro para cada política duplicada.
> Isso é um comportamento esperado e pode ser ignorado na maioria dos casos.

#### <a name="reinstall-private-keys-for-certificates"></a>Reinstale as chaves privadas para certificados
Se qualquer um dos certificados usados no partir do qual o backup foi criado de HGS foram adicionados usando as impressões digitais, somente a chave pública desses certificados será incluída no arquivo de backup.
Isso significa que você precisará manualmente instalar e/ou conceder acesso às chaves particulares para cada um desses certificados antes de HGS pode atender a solicitações de hosts Hyper-V.
As ações necessárias para concluir a etapa varia, dependendo de como seu certificado foi emitido originalmente.
Com suporte de software certificados emitidos por uma autoridade de certificação, você precisará entrar em contato com sua autoridade de certificação para obter a chave privada e instalá-lo no **cada** nó HGS por suas instruções.
Da mesma forma, se os certificados estiverem com suporte de hardware, você precisará consultar a documentação do fornecedor hardware security module para instalar os drivers necessários em cada nó HGS para conectar-se para o HSM e conceder acesso de cada máquina para a chave privada.

Como lembrete, foram adicionados para HGS usando as impressões digitais de certificados exigem replicação manual das chaves privadas para cada nó.
Você precisará repetir esta etapa em cada nó adicional que você adiciona ao cluster HGS restaurado.

#### <a name="review-imported-attestation-policies"></a>Analise as políticas de Atestado importados
Depois de importar as configurações de um backup, é recomendável intimamente examinar todas as diretivas importadas usando `Get-HgsAttestationPolicy` para garantir que somente os hosts que você confia para executar VMs blindadas poderá atestar com êxito.
Se você encontrar quaisquer políticas que não correspondem mais a sua postura de segurança, você pode [desabilitar ou removê-los](#review-attestation-policies).

#### <a name="run-diagnostics-to-check-system-state"></a>Execute o diagnóstico para verificar o estado do sistema
Depois de concluir a configuração e restaurar o estado do seu nó HGS, você deve executar a ferramenta de diagnóstico HGS para verificar o estado do sistema.
Para fazer isso, execute o seguinte comando no nó de HGS, onde você restaurou a configuração:

```powershell
Get-HgsTrace -RunDiagnostics
```

Se o "resultado geral" não "Pass", são necessárias etapas adicionais para concluir a configuração do sistema.
Verifique as mensagens relatadas no subtest(s) falha para obter mais informações.

## <a name="patching-hgs"></a>Aplicação de patch HGS
É importante manter seus nós de serviço guardião de Host atualizado ao instalar a atualização cumulativa mais recente quando ele sai. Se você estiver configurando um novo nó HGS, é altamente recomendável que você instale as atualizações disponíveis antes de instalar a função HGS ou configurá-lo.
Isso garantirá que qualquer novo ou alterada funcionalidade entrarão em vigor imediatamente.

Quando a aplicação de patches sua malha protegida, é altamente recomendável que você primeiro atualize *todos os* hosts Hyper-V **antes de atualizar o HGS**.
Isso é para garantir que sejam feitas alterações para as políticas de Atestado do HGS *depois de* os hosts Hyper-V foram atualizados para fornecer as informações necessárias para eles.
Se uma atualização é alterar o comportamento de políticas, eles serão não automaticamente habilitados para evitar interromper sua malha.
Essas atualizações exigem que você siga as diretrizes na seção a seguir para ativar as diretivas de Atestado novos ou alterados.
Recomendamos que você leia as notas de versão para Windows Server e as atualizações cumulativas que instalar para verificar se as atualizações de política são necessárias.

### <a name="updates-requiring-policy-activation"></a>Atualizações que exigem ativação de política
Se uma atualização para HGS apresenta ou altera significativamente o comportamento de uma política de Atestado, uma etapa adicional é necessária para ativar a política alterada.
Alterações de política são impostas somente depois de exportar e importar o estado HGS.
Você deve ativar apenas as políticas novas ou alteradas depois de aplicar a atualização cumulativa para todos os hosts e todos os nós do HGS no seu ambiente.
Depois que tiver sido atualizado a cada computador, execute os seguintes comandos em qualquer nó HGS para disparar o processo de atualização:

```powershell
$password = Read-Host -AsSecureString -Prompt "Enter a temporary password"
Export-HgsServerState -Path .\temporaryExport.xml -Password $password
Import-HgsServerState -Path .\temporaryExport.xml -Password $password
```

Se foi introduzida uma nova política, ela será desabilitada por padrão.
Para habilitar a nova política, primeiro encontrá-lo na lista de políticas do Microsoft (prefixado com 'HGS_') e, em seguida, habilitá-lo usando os seguintes comandos:

```powershell
Get-HgsAttestationPolicy

Enable-HgsAttestationPolicy -Name <Hgs_NewPolicyName>
```

## <a name="managing-attestation-policies"></a>Gerenciando políticas de Atestado
HGS mantém várias políticas de Atestado que definem o conjunto mínimo de requisitos de que um host deve atender para ser considerado "Íntegro" e permissão para executar VMs blindadas.
Algumas dessas políticas definidas pela Microsoft, outras são adicionadas por você para definir as políticas de integridade de código permitidos, linhas de base de TPM e hosts em seu ambiente.
A manutenção regular dessas diretivas é necessária para garantir a hosts possam continuar Atestado corretamente conforme você atualiza e substituí-los, e para garantir que quaisquer hosts não confiáveis ou configurações serão impedidas de Atestado com êxito.

Para atestado de admin confiável, há apenas uma política que determina se um host está íntegro: associação em um grupo de segurança conhecidas e confiáveis.
Atestado de TPM é mais complicado e envolve várias políticas para medir o código e configuração de um sistema antes de determinar se ele estiver íntegro.

Um único HGS pode ser configurado com as políticas do Active Directory e o TPM ao mesmo tempo, mas o serviço apenas verificará as políticas para o modo atual que está configurado para quando um host tenta Atestado.
Para verificar o modo do servidor HGS, execute `Get-HgsServer`.

### <a name="default-policies"></a>Políticas padrão
Para atestado de TPM confiável, há várias políticas internas configuradas no HGS.
Algumas dessas políticas são "bloqueadas" – que significa que não pode ser desabilitados por motivos de segurança.
A tabela a seguir explica a finalidade de cada política padrão.

Nome da política                    | Finalidade
-------------------------------|-----------------------------------------------------
Hgs_SecureBootEnabled          | Requer a hosts tem inicialização segura habilitada. Isso é necessário medir os binários de inicialização e outras configurações de bloqueio de UEFI.
Hgs_UefiDebugDisabled          | Assegura que hosts não têm um depurador de kernel habilitado. Depuradores de modo de usuário são bloqueados com as políticas de integridade de código.
Hgs_SecureBootSettings         | Diretiva negativa para garantir que os hosts corresponder a pelo menos uma linha TPM (definida pelo administrador).
Hgs_CiPolicy                   | Diretiva negativa para garantir que os hosts estão usando uma das políticas de CI definida pelo administrador.
Hgs_HypervisorEnforcedCiPolicy | Requer a política de integridade de código para ser imposta pelo hipervisor. Desabilitar esta política enfraquece suas proteções contra ataques de política de integridade de código de modo kernel.
Hgs_FullBoot                   | Garante que o host não retomou o funcionamento da suspensão ou hibernação. Hosts devem ser reiniciados ou desligados para passar essa política corretamente.
Hgs_VsmIdkPresent              | Requer segurança de virtualização com base em execução no host. O IDK representa a chave necessária para criptografar informações enviadas de volta para o espaço de memória segura do host.
Hgs_PageFileEncryptionEnabled  | Requer arquivos de paginação sejam criptografados no host. Desabilitar esta política pode resultar em exposição de informações se um arquivo de paginação não criptografado é inspecionado para segredos de locatário.
Hgs_BitLockerEnabled           | Exige que o BitLocker esteja habilitado no host Hyper-V. Essa política é desabilitada por padrão por motivos de desempenho e não é recomendada a ser habilitado. Essa política não tem nenhuma relevância na criptografia das VMs blindadas em si.
Hgs_IommuEnabled               | Requer que o host tenha um dispositivo IOMMU em uso para evitar ataques de acesso de memória direta. Desabilitar esta política e hosts sem um IOMMU habilitado podem expor os segredos de VM de locatário para direcionar a ataques de memória.
Hgs_NoHibernation              | Requer a hibernação será desabilitada no host Hyper-V. Desabilitar esta política pode permitir que os hosts economizar memória VM blindada para um arquivo de hibernação não criptografados.
Hgs_NoDumps                    | Requer que os despejos de memória a ser desabilitada no host Hyper-V. Se você desabilitar essa política, é recomendável que você configure a criptografia de despejo para impedir que a memória VM blindada sejam salvos em arquivos de despejo corrompidos não criptografada.
Hgs_DumpEncryption             | Requer despejos de memória, se habilitadas no host Hyper-V, a serem criptografados com uma chave de criptografia confiável pelo HGS. Essa política não se aplica se despejos de memória não estão habilitados no host. Se essa política e *Hgs\_NoDumps* serão desabilitados, memória VM blindada puderam ser salvos em um arquivo de despejo sem criptografia.
Hgs_DumpEncryptionKey          | Diretiva negativa para garantir que os hosts configurados para permitir que os despejos de memória estiver usando uma chave de criptografia do arquivo de despejo definida pelo administrador conhecida para HGS de memória. Essa política não se aplica quando *Hgs\_DumpEncryption* está desabilitado.

### <a name="authorizing-new-guarded-hosts"></a>Autorizando novos hosts protegidos
Para autorizar um novo host se torne um host protegido (por exemplo, atestar com êxito), HGS deve confiar o host e (quando configurado para usar o atestado de TPM confiável) o software em execução nele.
As etapas para autorizar um novo host diferem com base no modo de Atestado para o qual o HGS está atualmente configurado.
Para verificar o modo de Atestado para sua malha protegida, execute `Get-HgsServer` em qualquer nó do HGS.

#### <a name="software-configuration"></a>Configuração de software
No novo host do Hyper-V, certifique-se de que o Windows Server 2016 Datacenter edition está instalado.
Windows Server 2016 Standard não é possível executar VMs blindadas em uma malha protegida.
O host pode ser instalado a experiência de área de trabalho ou Server Core.

No servidor com experiência desktop e Server Core, você precisará instalar as funções de servidor Hyper-V e o suporte de Hyper-V de guardião de Host:

```powershell
Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
```

#### <a name="admin-trusted-attestation"></a>Atestado de Admin confiável
Para registrar um novo host em HGS ao usar o atestado de admin confiável, você deve primeiro adicionar o host de um grupo de segurança no domínio ao qual ele está associado.
Normalmente, cada domínio terá um grupo de segurança para hosts protegidos.
Se você já tiver registrado esse grupo com o HGS, a única ação que você precisa tomar é reiniciar o host para atualizar sua associação de grupo.

Você pode verificar quais grupos de segurança são confiáveis pelo HGS, executando o seguinte comando:

```powershell
Get-HgsAttestationHostGroup
```

Para registrar um novo grupo de segurança com o HGS, capturar o identificador de segurança (SID) do grupo no domínio do host e registrar o SID com HGS.

```powershell
Add-HgsAttestationHostGroup -Name "Contoso Guarded Hosts" -Identifier "S-1-5-21-3623811015-3361044348-30300820-1013"
```

Instruções sobre como configurar a relação de confiança entre o domínio do host e o HGS estão disponíveis no guia de implantação.

#### <a name="tpm-trusted-attestation"></a>Atestado de TPM confiável
Quando HGS é configurado no modo TPM, hosts devem passar todas as diretivas bloqueadas e as políticas de "habilitado" prefixadas com "Hgs_", bem como pelo menos uma linha de base do TPM, identificador do TPM e política de integridade de código.
Cada vez que você adiciona um novo host, você precisará registrar o novo identificador do TPM com HGS.
Desde que o host está executando o mesmo software (e tem a mesma política de integridade de código aplicadas) e a linha de base TPM como outro host no seu ambiente, você não precisará adicionar novas políticas de CI ou linhas de base.

**Adicionando o identificador do TPM para um novo host** no novo host, execute o seguinte comando para capturar o identificador do TPM.
Certifique-se de especificar um nome exclusivo para o host que ajudarão você a consultar o HGS.
Você precisará dessas informações se você encerrar o host ou impedir a VMs blindadas em execução no HGS.

```powershell
(Get-PlatformIdentifier -Name "Host01").InnerXml | Out-File C:\temp\host01.xml -Encoding UTF8
```

Copie esse arquivo ao seu servidor HGS, em seguida, execute o seguinte comando para registrar o host com o HGS.

```powershell
Add-HgsAttestationTpmHost -Name 'Host01' -Path C:\temp\host01.xml
```

**Adicionando uma nova linha de base TPM** se o novo host estiver executando um novo hardware ou configuração de firmware para o seu ambiente, talvez você precise executar uma nova linha de base do TPM.
Para fazer isso, execute o seguinte comando no host.

```powershell
Get-HgsAttestationBaselinePolicy -Path 'C:\temp\hardwareConfig01.tcglog'
```

> [!NOTE]
> Se você receber um erro informando que seu host e Falha na validação com êxito não serão atestar, não se preocupe.
> Isso é para garantir que o host pode executar uma verificação de pré-requisitos de VMs blindadas e, provavelmente, significa que você ainda não aplicaram uma política de integridade de código ou outra configuração exigida.
> Ler a mensagem de erro, faça as alterações sugeridas por ele e tente novamente.
> Como alternativa, você poderá ignorar a validação no momento, adicionando o `-SkipValidation` sinalizador para o comando.

Copiar a linha de base do TPM no servidor HGS e, em seguida, registrá-lo com o comando a seguir.
Recomendamos que você use uma convenção de nomenclatura que ajuda você a entender a configuração de hardware e firmware dessa classe de host Hyper-V.

```powershell
Add-HgsAttestationTpmPolicy -Name 'HardwareConfig01' -Path 'C:\temp\hardwareConfig01.tcglog'
```

**Adicionando uma nova política de integridade de código** se você tiver alterado a política de integridade de código em execução em seus hosts Hyper-V, você precisará registrar a nova política com o HGS antes que esses hosts podem atestar com êxito.
Em um host de referência, que serve como uma imagem mestra para as máquinas do Hyper-V confiáveis em seu ambiente, capturar uma nova CI política usando o `New-CIPolicy` comando.
Incentivamos você a usar o **FilePublisher** nível e **Hash** fallback para políticas de CI do host do Hyper-V.
Você deve primeiro criar uma política de CI no modo de auditoria para garantir que tudo está funcionando conforme o esperado.
Depois de validar uma carga de trabalho de exemplo no sistema, você pode impor a política e copie a versão de imposto para HGS.
Para obter uma lista completa das opções de configuração de política de integridade de código, consulte o [documentação do Device Guard](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-deploy-code-integrity-policies).

```powershell
# Capture a new CI policy with the FilePublisher primary level and Hash fallback and enable user mode code integrity protections
New-CIPolicy -FilePath 'C:\temp\ws2016-hardware01-ci.xml' -Level FilePublisher -Fallback Hash -UserPEs

# Apply the CI policy to the system
ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\ws2016-hardware01-ci.xml' -BinaryFilePath 'C:\temp\ws2016-hardware01-ci.p7b'
Copy-Item 'C:\temp\ws2016-hardware01-ci.p7b' 'C:\Windows\System32\CodeIntegrity\SIPolicy.p7b'
Restart-Computer

# Check the event log for any untrusted binaries and update the policy if necessary
# Consult the Device Guard documentation for more details

# Change the policy to be in enforced mode
Set-RuleOption -FilePath 'C:\temp\ws2016-hardare01-ci.xml' -Option 3 -Delete

# Apply the enforced CI policy on the system
ConvertFrom-CIPolicy -XmlFilePath 'C:\temp\ws2016-hardware01-ci.xml' -BinaryFilePath 'C:\temp\ws2016-hardware01-ci.p7b'
Copy-Item 'C:\temp\ws2016-hardware01-ci.p7b' 'C:\Windows\System32\CodeIntegrity\SIPolicy.p7b'
Restart-Computer
```

Depois que a política criada, testada e imposta, copiar o arquivo binário (. p7b) no servidor HGS e registre-se a política.

```powershell
Add-HgsAttestationCiPolicy -Name 'WS2016-Hardware01' -Path 'C:\temp\ws2016-hardware01-ci.p7b'
```

**Adicionando uma chave de criptografia de despejo de memória**

Quando o *Hgs\_NoDumps* política está desabilitada e *Hgs\_DumpEncryption* política estiver habilitada, os hosts protegidos podem ter despejos de memória (incluindo os despejos de memória) para ser habilitado, desde os despejos são criptografados. Hosts protegidos só passará Atestado se eles têm despejos de memória desabilitados ou criptografando-os com uma chave conhecida HGS. Por padrão, nenhuma chave de criptografia de despejo de memória é configurado no HGS.

Para adicionar uma chave de criptografia de despejo para HGS, use o `Add-HgsAttestationDumpPolicy` cmdlet para fornecer HGS com o hash da sua chave de criptografia de despejo.
Se você capturar uma linha de base do TPM em um host Hyper-V configurado com a criptografia de despejo, o hash é incluído no tcglog e pode ser fornecido para o `Add-HgsAttestationDumpPolicy` cmdlet.

```powershell
Add-HgsAttestationDumpPolicy -Name 'DumpEncryptionKey01' -Path 'C:\temp\TpmBaselineWithDumpEncryptionKey.tcglog'
```

Como alternativa, você pode fornecer diretamente a representação de cadeia de caracteres de hash para o cmdlet.

```powershell
Add-HgsAttestationDumpPolicy -Name 'DumpEncryptionKey02' -PublicKeyHash '<paste your hash here>'
```

Certifique-se de adicionar cada chave de criptografia exclusivo de despejo para HGS, se você optar por usar teclas diferentes em sua malha protegida.
Hosts que estão criptografando despejos de memória com uma chave que não se sabe HGS não passa Atestado.

Consulte a documentação do Hyper-V para obter mais informações sobre [Configurando a criptografia em hosts de despejo](https://technet.microsoft.com/windows-server-docs/virtualization/hyper-v/manage/about-dump-encryption).

#### <a name="check-if-the-system-passed-attestation"></a>Verifique se o sistema passado Atestado
Depois de registrar as informações necessárias com o HGS, você deve verificar se o host transmite o Atestado.
No host do Hyper-V recém-adicionado, execute `Set-HgsClientConfiguration` e fornecer as URLs corretas para o seu cluster HGS.
Essas URLs podem ser obtidos pela execução `Get-HgsServer` em qualquer nó do HGS.

```powershell
Set-HgsClientConfiguration -KeyProtectionServerUrl 'http://hgs.bastion.local/KeyProtection' -AttestationServerUrl 'http://hgs.bastion.local/Attestation'
```

Se o status resultante não indicar "IsHostGuarded: True", você precisará solucionar problemas de configuração.
No host que falharam Atestado, execute o seguinte comando para obter um relatório detalhado sobre os problemas que podem ajudá-lo a resolver o Atestado falhou.

```powershell
Get-HgsTrace -RunDiagnostics -Detailed
```

> [!IMPORTANT]
> Se você estiver usando o Windows Server 2019 ou Windows 10, versão 1809 e estiver usando políticas de integridade de código `Get-HgsTrace` pode retornar uma falha para o **ativa de política de integridade código** diagnóstico.
> Você pode ignorar com segurança esse resultado, quando se trata de apenas falhando diagnóstico.

### <a name="review-attestation-policies"></a>Analise as políticas de Atestado
Para examinar o estado atual de políticas configuradas no HGS, execute os seguintes comandos em qualquer nó HGS:

```powershell
# List all trusted security groups for admin-trusted attestation
Get-HgsAttestationHostGroup

# List all policies configured for TPM-trusted attestation
Get-HgsAttestationPolicy
```

Se você encontrar uma diretiva habilitada que não atenda às suas necessidades de segurança (por exemplo, uma antigo política integridade de código que agora é considerada não segura), você pode desabilitá-lo, substituindo o nome da política no comando a seguir:

```powershell
Disable-HgsAttestationPolicy -Name 'PolicyName'
```

Da mesma forma, você pode usar `Enable-HgsAttestationPolicy` para habilitar novamente uma política.

Se você não precisa de uma política e deseja removê-lo de todos os nós HGS, execute `Remove-HgsAttestationPolicy -Name 'PolicyName'` para excluir permanentemente a política.

## <a name="changing-attestation-modes"></a>Alterando modos de Atestado
Se você iniciou sua malha protegida usando o atestado de admin confiável, provavelmente você desejará atualizar para o modo de atestado de TPM mais forte muito assim que você tem suficiente hosts de 2.0 compatível com TPM no seu ambiente.
Quando estiver pronto para mudar, você pode carregar todos os artefatos de Atestado (políticas de CI, linhas de base de TPM e identificadores TPM) em HGS previamente enquanto continua a executar HGS com o atestado de admin confiável.
Para fazer isso, basta seguir as instruções na [autorizando um novo host protegido](#authorizing-new-guarded-hosts) seção.

Depois de adicionar todas as suas políticas para HGS, a próxima etapa é executar uma tentativa de Atestado sintético em seus hosts para ver se eles passaria Atestado no modo TPM.
Isso não afeta o estado operacional atual do HGS.
Os comandos a seguir devem ser executados em um computador que tem acesso a todos os hosts no ambiente e pelo menos um nó HGS.
Se seu firewall ou outras políticas de segurança evitar isso, você pode ignorar esta etapa.
Quando possível, recomendamos que você executar o Atestado sintético para lhe dar uma boa indicação de se "inversão" para o modo TPM fará com que o tempo de inatividade para suas VMs. 

```powershell
# Get information for each host in your environment
$hostNames = 'host01.contoso.com', 'host02.contoso.com', 'host03.contoso.com'
$credential = Get-Credential -Message 'Enter a credential with admin privileges on each host'
$targets = @()
$hostNames | ForEach-Object { $targets += New-HgsTraceTarget -Credential $credential -Role GuardedHost -HostName $_ }

$hgsCredential = Get-Credential -Message 'Enter an admin credential for HGS'
$targets += New-HgsTraceTarget -Credential $hgsCredential -Role HostGuardianService -HostName 'HGS01.bastion.local'

# Initiate the synthetic attestation attempt
Get-HgsTrace -RunDiagnostics -Target $targets -Diagnostic GuardedFabricTpmMode 
```

Depois de diagnóstico completo, examine as informações resultantes para determinar se todos os hosts falharia Atestado no modo TPM.
Execute o diagnóstico novamente até que você obtenha "pass" de cada host, continue para alterar o HGS para o modo TPM.

**Alterar para o modo TPM** leva apenas alguns segundos para concluir.
Execute o seguinte comando em qualquer nó HGS para atualizar o modo de Atestado.

```powershell
Set-HgsServer -TrustTpm
```

Se você enfrentar problemas e precisar alternar para o modo do Active Directory, você pode fazer isso executando `Set-HgsServer -TrustActiveDirectory`.

Depois de confirmar que tudo está funcionando conforme o esperado, você deve remover todos os grupos de hosts confiáveis do Active Directory do HGS e remova a relação de confiança entre os domínios HGS e fabric.
Se você deixar a relação de confiança do Active Directory em vigor, você corre o risco alguém habilitar novamente a relação de confiança e alternando o HGS para o modo de Active Directory, que pode permitir que código não confiável para execução não verificado em seus hosts protegidos.

## <a name="key-management"></a>Gerenciamento de chaves
A solução de malha protegida usa vários pares de chaves pública/privada para validar a integridade dos vários componentes da solução e criptografar segredos de locatário.
Serviço guardião de Host é configurado com pelo menos dois certificados (com chaves públicas e privadas), que é usado para assinar e criptografar as chaves usadas para iniciar o backup de VMs blindadas.
Essas chaves devem ser gerenciadas com cuidado.
Se a chave privada é adquirida por um adversário, eles poderão unshield todas as VMs em execução em sua malha ou configurar um cluster HGS impostor que usa políticas de Atestado mais fracas para ignorar as proteções colocadas em vigor.
Você perderá as chaves privadas durante um desastre e não encontrá-los em um backup, você precisará configurar um novo par de chaves e ter cada VM com a chave novamente para autorizar seus novos certificados.

Esta seção aborda tópicos gerais de gerenciamento de chaves para ajudá-lo a configurar as chaves para que sejam funcionais e seguras.

### <a name="adding-new-keys"></a>Adição de novas chaves
Enquanto o HGS deve ser inicializado com um conjunto de chaves, você pode adicionar mais de uma criptografia e assinatura de chave HGS.
Os dois motivos mais comuns por que você adicionaria novas chaves para HGS são:
1. Para dar suporte a cenários "Traga sua própria chave" em que os locatários copiar suas chaves privadas para seu módulo de segurança de hardware e autorizam somente suas chaves para iniciar o backup de suas VMs blindadas.
2. Para substituir as chaves existentes para HGS primeiro adicionando novas chaves e manter os dois conjuntos de chaves até que cada VM configuração foi atualizada para usar as novas chaves.

O processo para adicionar as novas chaves varia de acordo com o tipo de certificado que você está usando.

**Opção 1: Adicionando um certificado armazenado em um HSM**

Nossa abordagem recomendada para proteger chaves de HGS é usar certificados criados em um módulo de segurança de hardware (HSM).
Os HSMs garantem o uso de suas chaves é vinculado ao acesso físico a um dispositivo sensível à segurança em seu datacenter.
Cada HSM é diferente e tem um processo exclusivo para criar certificados e registrá-los com o HGS.
As etapas a seguir destinam-se a fornecer orientação aproximada para usar um HSM com suporte a certificados.
Consulte a documentação do fornecedor do HSM para recursos e as etapas exatas.

1. Instale o software HSM em cada nó do HGS no seu cluster. Dependendo se você tiver uma rede ou dispositivo HSM local, talvez você precise configurar o HSM para conceder o acesso de máquina para seu repositório de chaves.
2. Criar 2 certificados no HSM com **chaves RSA de 2048 bits** para criptografia e assinatura
    1. Criar um certificado de criptografia com o **codificação de dados** propriedade de uso no seu HSM da chave
    2. Criar um certificado de autenticação com o **Assinatura Digital** propriedade de uso no seu HSM da chave
3. Instale os certificados no repositório de certificados local do cada nó HGS segundo as diretrizes do fornecedor do HSM.
4. Se o seu HSM usa permissões granulares para conceder permissão para usar a chave privada a aplicativos ou usuários específicos, você precisará conceder seu acesso de conta de serviço do HGS gerenciado de grupo para o certificado. Você pode encontrar o nome da conta gMSA HGS executando `(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName`
5. Adicione os certificados de assinatura e criptografia para HGS, substituindo as impressões digitais com aqueles dos seus certificados nos comandos a seguir:

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899"
    Add-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint "99887766554433221100FFEEDDCCBBAA"
    ```

**Opção 2: A adição de certificados de software não exportável**

Se você tiver um certificado com suporte de software emitido por sua empresa ou uma autoridade de certificação pública que tem uma chave privada não exportável, você precisará adicionar o certificado a usar sua impressão digital do HGS.
1. Instale o certificado em seu computador de acordo com as instruções da autoridade de certificação.
2. Conceda o HGS gerenciado de grupo serviço conta acesso de leitura para a chave privada do certificado. Você pode encontrar o nome da conta gMSA HGS executando `(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName`
3. Registrar o certificado com o HGS usando o comando a seguir e substituindo sua impressão digital de certificado (altere *criptografia* à *Signing* para certificados de assinatura):

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899"
    ```

> [!IMPORTANT]
> Você precisará instalar a chave privada e conceder acesso de leitura para a conta gMSA em cada nó HGS manualmente.
> HGS automaticamente não pode duplicar as chaves privadas para *qualquer* registrado por sua impressão digital do certificado.

**Opção 3: Adicionando certificados armazenados em arquivos PFX**

Se você tiver um certificado com suporte de software com uma chave privada exportável que pode ser armazenado no formato de arquivo PFX e protegido com uma senha, HGS pode gerenciar seus certificados automaticamente para você.
Certificados adicionados com arquivos PFX são replicados automaticamente para todos os nós do cluster do HGS e HGS protege o acesso às chaves particulares.
Para adicionar um novo certificado usando um arquivo PFX, execute os seguintes comandos em qualquer nó HGS (altere *criptografia* à *Signing* para certificados de assinatura):

```powershell
$certPassword = Read-Host -AsSecureString -Prompt "Provide the PFX file password"
Add-HgsKeyProtectionCertificate -CertificateType Encryption -CertificatePath "C:\temp\encryptionCert.pfx" -CertificatePassword $certPassword
```

**Identificando e alterando os certificados primários** enquanto HGS pode dar suporte a vários certificados de assinatura e criptografia, ele usa um par como seus certificados "principais".
Esses são os certificados que serão usados se alguém baixa os metadados do guardião para esse cluster HGS.
Para verificar quais certificados estão marcados como os certificados primários, execute o seguinte comando:

```powershell
Get-HgsKeyProtectionCertificate -IsPrimary $true
```

Para definir uma nova criptografia primária ou o certificado de autenticação, encontrar a impressão digital do certificado desejado e marcá-la como primário usando os seguintes comandos:

```powershell
Get-HgsKeyProtectionCertificate
Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899" -IsPrimary
Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint "99887766554433221100FFEEDDCCBBAA" -IsPrimary
```

### <a name="renewing-or-replacing-keys"></a>Renovar ou substituir chaves
Quando você cria os certificados usados pelo HGS, os certificados serão atribuídos uma data de expiração acordo com a política da autoridade de certificação e suas informações de solicitação.
Normalmente, em cenários em que a validade do certificado é importante, como proteger as comunicações HTTP, certificados devem ser renovados antes de expirarem para evitar uma interrupção do serviço ou uma mensagem de erro preocupante.
HGS não usa certificados nesse sentido.
Simplesmente o HGS está usando certificados como uma maneira conveniente de criar e armazenar um par de chaves assimétricas.
Criptografia expirada ou certificado de autenticação em HGS não indica um ponto fraco ou perda de proteção para VMs blindadas.
Além disso, as verificações de revogação de certificado não são realizadas pelo HGS.
Se o certificado da autoridade emissora ou um certificado HGS é revogado, ele não terá impacto uso HGS do certificado.

A única vez em que você precisa se preocupar sobre um certificado HGS é se você tiver motivos para acreditar que sua chave privada tenha sido roubada.
Nesse caso, a integridade de suas VMs blindadas está em risco porque a posse de metade privada do par de chaves de assinatura e a criptografia de HGS é suficiente para remover as proteções de blindagem em uma VM ou coloque um servidor HGS falso que tem políticas mais fracas de Atestado.

Se você encontrar nessa situação ou é necessárias para padrões de conformidade de atualização regular dos chaves de certificado, as etapas a seguir descrevem o processo para alterar as chaves em um servidor HGS.
Observe que as diretrizes a seguir representam uma realização significativa que irá resultar em uma interrupção do serviço para cada VM no servidas pelo cluster HGS.
O planejamento adequado para alterar as chaves HGS é necessário para minimizar as interrupções de serviço e garantir a segurança de VMs do locatário.

Em um nó HGS, execute as seguintes etapas para registrar um novo par de certificados de assinatura e criptografia.
Consulte a seção sobre [adição de novas chaves](#adding-new-keys) para obter informações detalhadas de várias maneiras de adicionar novas chaves para HGS.
1. Crie um novo par de criptografia e certificados de autenticação para o servidor HGS. O ideal é que eles serão criados em um módulo de segurança de hardware.
2. Registre-se a nova criptografia e assinatura de certificados com **HgsKeyProtectionCertificate adicionar**

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint>
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint>
    ```
3. Se você usou as impressões digitais, você precisará ir para cada nó no cluster para instalar a chave privada e conceder acesso à chave gMSA HGS.
4. Tornar os certificados de padrão de novos certificados em HGS

    ```powershell
    Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint> -IsPrimary
    Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint> -IsPrimary
    ```

Neste ponto, os dados criados com os metadados obtidos a partir do nó HGS de blindagem usará os novos certificados, mas VMs existentes continuarão funcionando porque os certificados mais antigos ainda estão lá.
Para garantir que todas as VMs existentes funcionarão com as novas chaves, você precisará atualizar o protetor de chave em cada VM.
Isso é uma ação que requer que o proprietário da VM (pessoa ou entidade em posse do guardião de "proprietário") seja envolvido.
Para cada VM blindada, execute as seguintes etapas:
5. Desligar a VM. A VM não pode ser ativada novamente até que as etapas restantes sejam concluídas ou então você precisará iniciar o processo novamente.
6. Salve o protetor de chave atual em um arquivo: `Get-VMKeyProtector -VMName 'VM001' | Out-File '.\VM001.kp'`
7. Transferir o KP para o proprietário da VM
8. Ter as informações atualizadas guardião do HGS de download do proprietário e importá-lo em seu sistema local
9. Ler o KP atual na memória, conceda o acesso de guardião de novo o KP e salvá-lo em um novo arquivo executando os comandos a seguir:

    ```powershell
    $kpraw = Get-Content -Path .\VM001.kp
    $kp = ConvertTo-HgsKeyProtector -Bytes $kpraw
    $newGuardian = Get-HgsGuardian -Name 'UpdatedHgsGuardian'
    $updatedKP = Grant-HgsKeyProtectorAccess -KeyProtector $kp -Guardian $newGuardian
    $updatedKP.RawData | Out-File .\updatedVM001.kp
    ```
10. Copiar o KP atualizado para a malha de hospedagem
11. Aplique o KP para a VM original:

   ```powershell
   $updatedKP = Get-Content -Path .\updatedVM001.kp
   Set-VMKeyProtector -VMName VM001 -KeyProtector $updatedKP
   ```
12. Finalmente, inicie a VM e verifique se que ele é executado com êxito.

> [!NOTE]
> Se o proprietário da VM define um protetor de chave incorreto na VM e não autorizar sua malha para executar a VM, não será possível iniciar a máquina virtual blindada.
> Para retornar à última protetor de chave bom conhecido, execute `Set-VMKeyProtector -RestoreLastKnownGoodKeyProtector`

Depois que todas as VMs foram atualizadas para autorizar as novas chaves de guardião, você pode desabilitar e remover as chaves antigas.

13. Obter as impressões digitais dos certificados do antigos `Get-HgsKeyProtectionCertificate -IsPrimary $false`

14. Desabilite cada certificado executando os comandos a seguir:  

   ```powershell
   Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint> -IsEnabled $false
   Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint> -IsEnabled $false
   ```

15. Depois de garantir que as VMs são ainda pode começar com os certificados desabilitados, remova os certificados de HGS, executando os comandos a seguir:

   ```powershell
   Remove-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint>`
   Remove-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint>`
   ```

> [!IMPORTANT]
> Backups de VM conterá informações antigas do protetor de chave que permitem que os antigos certificados a ser usado para iniciar o backup da VM.
> Se você estiver ciente de que sua chave privada foi comprometida, você deve presumir que os backups VM podem ser comprometidos, também e tomar as devidas providências.
> Destruindo a configuração da VM dos backups (. vmcx) removerá os protetores de chave, às custas da necessidade de usar a senha de recuperação do BitLocker para inicializar a VM na próxima vez.

### <a name="key-replication-between-nodes"></a>Replicação entre os nós principais
Todos os nós no cluster HGS devem ser configurado com a mesma criptografia, assinatura e (quando configurado) certificados SSL.
Isso é necessário para garantir que os hosts Hyper-V contatando a qualquer nó no cluster podem ter suas solicitações atendidas com êxito.

**Se você inicializou servidor HGS com certificados baseados em PFX** e em seguida, HGS serão replicadas automaticamente a chave pública e privada desses certificados em todos os nós no cluster.
Você só precisará adicionar as chaves em um nó.

**Se você inicializou o servidor HGS com referências de certificado** ou as impressões digitais e, em seguida, HGS só replicará as *público* chaves no certificado para cada nó.
Além disso, não é possível conceder HGS próprio o acesso à chave privada em qualquer nó nesse cenário.
Portanto, é sua responsabilidade:
1. Instale a chave privada em cada nó HGS
2. Conceda o acesso de conta (gMSA) de serviço do HGS gerenciado de grupo para a chave privada em cada nó que essas tarefas adicione carga extra operacional, no entanto, eles são necessários para chaves protegidas por HSM e certificados com chaves privadas não exportável.

**Certificados SSL** nunca são replicados em qualquer formulário.
É sua responsabilidade para inicializar cada servidor HGS com o mesmo certificado SSL e atualizar cada servidor sempre que você opta por renovar ou substituir o certificado SSL.
Ao substituir o certificado SSL, é recomendável que você faça isso usando o [HgsServer conjunto](https://technet.microsoft.com/library/mt652180.aspx) cmdlet.

## <a name="unconfiguring-hgs"></a>HGS unconfiguring

Se você precisar encerrar ou significativamente reconfigurar um servidor HGS, você pode fazer isso usando o [Clear HgsServer](https://technet.microsoft.com/library/mt652176.aspx) ou [desinstalar HgsServer](https://technet.microsoft.com/library/mt652182.aspx) cmdlets.

### <a name="clearing-the-hgs-configuration"></a>Limpando a configuração de HGS

Para remover um nó de cluster HGS, use o [HgsServer Clear](https://technet.microsoft.com/library/mt652176.aspx) cmdlet.
Esse cmdlet fará as seguintes alterações no servidor onde ele é executado:

- Cancela o registro de serviços de proteção de Atestado e chave
- Remove o ponto de extremidade de gerenciamento de JEA "microsoft.windows.hgs"
- Remove o computador local do cluster de failover do HGS

Se o servidor for o último nó do HGS no cluster, o cluster e seu recurso de nome de rede distribuída correspondente também serão destruídos.

```powershell
# Removes the local computer from the HGS cluster
Clear-HgsServer
```

Depois que a operação de limpeza for concluído, o servidor HGS pode ser reinicializado com [Initialize HgsServer](https://technet.microsoft.com/library/mt652185.aspx).
Se você usou [Install-HgsServer](https://technet.microsoft.com/library/mt652169.aspx) para configurar um domínio do Active Directory Domain Services, esse domínio permanecerá configurado e operacional após a operação de limpeza.

### <a name="uninstalling-hgs"></a>Desinstalando o HGS

Se você quiser remover um nó de cluster de HGS **e** rebaixar o controlador de domínio do Active Directory em execução, use o [desinstalar HgsServer](https://technet.microsoft.com/library/mt652182.aspx) cmdlet.
Esse cmdlet fará as seguintes alterações no servidor onde ele é executado:

- Cancela o registro de serviços de proteção de Atestado e chave
- Remove o ponto de extremidade de gerenciamento de JEA "microsoft.windows.hgs"
- Remove o computador local do cluster de failover do HGS
- Rebaixa o controlador de domínio do Active Directory, se configurado

Se o servidor for o último nó do HGS no cluster, o domínio, o cluster de failover e o recurso de nome de rede distribuída do cluster também serão destruídos.

```powershell
# Removes the local computer from the HGS cluster and demotes the ADDC (restart required)
$newLocalAdminPassword = Read-Host -AsSecureString -Prompt "Enter a new password for the local administrator account"
Uninstall-HgsServer -LocalAdministratorPassword $newLocalAdminPassword -Restart
```

Depois que a operação de desinstalação for concluída e o computador for reiniciado, você pode reinstalar ADDC e uso de HGS [Install HgsServer](https://technet.microsoft.com/library/mt652169.aspx) ou ingressar o computador em um domínio e inicializar o servidor HGS nesse domínio com [ Inicializar HgsServer](https://technet.microsoft.com/library/mt652185.aspx).

Se você não pretende usar o computador como um nó HGS, você pode remover a função do Windows.

```powershell
Uninstall-WindowsFeature HostGuardianServiceRole
```
