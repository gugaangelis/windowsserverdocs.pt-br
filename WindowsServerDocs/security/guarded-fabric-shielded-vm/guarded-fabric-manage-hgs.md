---
title: Gerenciando o serviço guardião de host
ms.topic: article
ms.assetid: eecb002e-6ae5-4075-9a83-2bbcee2a891c
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.openlocfilehash: 851ea4a57068c1544f290c48f370e04b96857cf6
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87989162"
---
# <a name="managing-the-host-guardian-service"></a>Gerenciando o serviço guardião de host

> Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

O serviço guardião de host (HGS) é o ponto central da solução de malha protegida.
É responsável por garantir que os hosts Hyper-V na malha sejam conhecidos pelo hoster ou pela empresa e pela execução de software confiável e pelo gerenciamento das chaves usadas para iniciar as VMs blindadas.
Quando um locatário decide confiar que você hospeda suas VMs blindadas, eles estão fazendo sua confiança em sua configuração e gerenciamento do serviço guardião de host.
Portanto, é muito importante seguir as práticas recomendadas ao gerenciar o serviço guardião de host para garantir a segurança, a disponibilidade e a confiabilidade da sua malha protegida.
As diretrizes nas seções a seguir abordam os problemas operacionais mais comuns enfrentados pelos administradores do HGS.

## <a name="limiting-admin-access-to-hgs"></a>Limitando o acesso de administrador ao HGS
Devido à natureza sensível à segurança do HGS, é importante garantir que seus administradores sejam membros altamente confiáveis da sua organização e, idealmente, sejam separados dos administradores de seus recursos de malha.
Além disso, é recomendável que você gerencie somente o HGS de estações de trabalho seguras usando protocolos de comunicação seguros, como o WinRM por HTTPS.

### <a name="separation-of-duties"></a>Separação de funções
Ao configurar o HGS, você terá a opção de criar uma floresta Active Directory isolada apenas para o HGS ou para unir o HGS a um domínio confiável existente.
Essa decisão, bem como as funções que você atribui os administradores em sua organização, determinam o limite de confiança para o HGS.
Quem tem acesso ao HGS, seja diretamente como um administrador ou indiretamente como administrador de outra coisa (por exemplo, Active Directory) que possa influenciar o HGS, tem controle sobre sua malha protegida.
Administradores HGS escolha quais hosts Hyper-V estão autorizados a executar VMs blindadas e gerenciar os certificados necessários para iniciar as VMs blindadas.
Um invasor ou administrador mal-intencionado que tem acesso ao HGS pode usar esse poder para autorizar os hosts comprometidos a executarem VMs blindadas, iniciar um ataque de negação de serviço removendo material da chave e muito mais.

Para evitar esse risco, é *altamente* recomendável limitar a sobreposição entre os administradores do seu HgS (incluindo o domínio ao qual o HgS está ingressado) e os ambientes do Hyper-V.
Garantindo que nenhum administrador tenha acesso a ambos os sistemas, um invasor precisaria comprometer duas contas diferentes de duas pessoas para concluir sua missão de alterar as políticas HGS.
Isso também significa que o domínio e os administradores corporativos para os dois ambientes de Active Directory não devem ser a mesma pessoa, nem que o HGS use a mesma floresta de Active Directory que seus hosts Hyper-V.
Qualquer pessoa que possa conceder acesso a mais recursos representa um risco de segurança.

### <a name="using-just-enough-administration"></a>Usando apenas a administração suficiente
O HGS vem com funções de administração Jea ( [apenas suficientes](https://aka.ms/JEAdocs) ) internas para ajudá-lo a gerenciá-lo com mais segurança.
O JEA ajuda a permitir que você delegue tarefas de administração para usuários não administradores, o que significa que as pessoas que gerenciam políticas HGS não precisam realmente ser administradores de todo o computador ou domínio.
O JEA funciona limitando os comandos que um usuário pode executar em uma sessão do PowerShell e usando uma conta local temporária nos bastidores (exclusivos para cada sessão de usuário) para executar os comandos que normalmente exigem elevação.

O HGS é fornecido com duas funções JEA pré-configuradas:
- **Administradores HgS** que permitem que os usuários gerenciem todas as políticas HgS, incluindo a autorização de novos hosts para executar VMs blindadas.
- **Revisores HgS** que permitem aos usuários o direito de auditar as políticas existentes. Eles não podem fazer nenhuma alteração na configuração do HGS.

Para usar o JEA, primeiro você precisa criar um novo usuário padrão e torná-lo um membro do grupo Administradores do HGS ou dos revisores HGS.
Se você usou `Install-HgsServer` para configurar uma nova floresta para o HgS, esses grupos serão nomeados "*ServiceName*Administrators" e "*ServiceName*revisores", respectivamente, em que *ServiceName* é o nome de rede do cluster HgS.
Se você tiver ingressado em HGS em um domínio existente, deverá consultar os nomes de grupo especificados em `Initialize-HgsServer` .

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

**Auditar políticas com a função revisor**

Em um computador remoto que tenha conectividade de rede com o HGS, execute os seguintes comandos no PowerShell para inserir a sessão JEA com as credenciais do revisor.
É importante observar que, como a conta do revisor é apenas um usuário padrão, ela não pode ser usada para comunicação remota regular do Windows PowerShell, Área de Trabalho Remota acesso ao HGS, etc.

```powershell
Enter-PSSession -ComputerName <hgsnode> -Credential '<hgsdomain>\hgsreviewer01' -ConfigurationName 'microsoft.windows.hgs'
```

Em seguida, você pode verificar quais comandos são permitidos na sessão com `Get-Command` e executar os comandos permitidos para auditar a configuração.
No exemplo abaixo, estamos verificando quais políticas estão habilitadas no HGS.

```powershell
Get-Command

Get-HgsAttestationPolicy
```

Digite o comando `Exit-PSSession` ou seu alias, `exit` quando terminar de trabalhar com a sessão Jea.

**Adicionar uma nova política ao HGS usando a função de administrador**

Para realmente alterar uma política, você precisa se conectar ao ponto de extremidade JEA com uma identidade que pertença ao grupo ' hgsAdministrators '.
No exemplo abaixo, mostramos como você pode copiar uma nova política de integridade de código para HGS e registrá-la usando JEA.
A sintaxe pode ser diferente da qual você está acostumado.
Isso é para acomodar algumas das restrições em JEA, como não ter acesso ao sistema de arquivos completo.

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

## <a name="monitoring-hgs"></a>Monitorando HGS
### <a name="event-sources-and-forwarding"></a>Origens e encaminhamento de eventos
Os eventos do HGS aparecerão no log de eventos do Windows em 2 fontes:
- **HostGuardianService-atestado**
- **HostGuardianService – keyprotection**

Você pode exibir esses eventos abrindo Visualizador de Eventos e navegando para Microsoft-Windows-HostGuardianService-atestation e Microsoft-Windows-HostGuardianService-keyprotection.

Em um ambiente grande, geralmente é preferível encaminhar eventos para um coletor de eventos do Windows central para tornar o análise dos eventos mais fácil.
Para obter mais informações, confira a [documentação de encaminhamento de eventos do Windows](/windows/win32/wec/windows-event-collector).

### <a name="using-system-center-operations-manager"></a>Usando System Center Operations Manager
Você também pode usar o System Center 2016-Operations Manager para monitorar o HGS e seus hosts protegidos.
O pacote de gerenciamento de malha protegida tem monitores de eventos para verificar se há configurações incorretas comuns que podem levar ao tempo de inatividade do datacenter, incluindo hosts que não passam por atestado e servidores HGS que relatam erros.

Para começar, [Instale e configure o SCOM 2016](https://technet.microsoft.com/system-center-docs/om/welcome-to-operations-manager) e [Baixe o pacote de gerenciamento de malha protegida](https://www.microsoft.com/download/details.aspx?id=52764).
O guia do pacote de gerenciamento incluído explica como configurar o pacote de gerenciamento e entender o escopo de seus monitores.

## <a name="backing-up-and-restoring-hgs"></a>Fazendo backup e restaurando o HGS
### <a name="disaster-recovery-planning"></a>Planejamento de recuperação de desastre
Ao rascunhar seus planos de recuperação de desastres, é importante considerar os requisitos exclusivos do serviço guardião de host em sua malha protegida.
Se você perder alguns ou todos os seus nós HGS, poderá enfrentar problemas de disponibilidade imediatos que impedirão que os usuários iniciem suas VMs blindadas.
Em um cenário em que você perder o cluster HGS inteiro, você precisará ter backups completos da configuração do HGS disponíveis para restaurar seu cluster HGS e retomar as operações normais.
Esta seção aborda as etapas necessárias para se preparar para esse cenário.

Primeiro, é importante entender o que é importante para fazer backup do HGS.
O HGS retém várias informações que ajudam a determinar quais hosts estão autorizados a executar VMs blindadas.
Isso inclui:
1. Active Directory identificadores de segurança para os grupos que contêm hosts confiáveis (ao usar o atestado Active Directory);
2. Identificadores de TPM exclusivos para cada host em seu ambiente;
3. Políticas do TPM para cada configuração exclusiva do host; e
4. Políticas de integridade de código que determinam qual software pode ser executado em seus hosts.

Esses artefatos de atestado exigem a coordenação com os administradores de sua malha de hospedagem para obter, potencialmente, dificultando a obtenção dessas informações novamente após um desastre.

Além disso, o HGS requer acesso a dois ou mais certificados usados para criptografar e assinar as informações necessárias para iniciar uma VM blindada (o protetor de chave).
Esses certificados são bem conhecidos (usados pelos proprietários de VMs blindadas para autorizar sua malha a executar suas VMs) e devem ser restaurados após um desastre para uma experiência de recuperação direta.
Se você não restaurar o HGS com os mesmos certificados após um desastre, cada VM precisará ser atualizada para autorizar suas novas chaves a descriptografar suas informações.
Por motivos de segurança, somente o proprietário da VM pode atualizar a configuração da VM para autorizar essas novas chaves, o que significa que a falha ao restaurar suas chaves após um desastre resultará em cada proprietário da VM que precisa tomar medidas para que suas VMs sejam executadas novamente.

#### <a name="preparing-for-the-worst"></a>Preparando para o pior
Para se preparar para uma perda completa do HGS, há duas etapas que devem ser seguidas:
1. Fazer backup das políticas de atestado HGS
2. Fazer backup das chaves HGS

A orientação sobre como executar essas duas etapas é fornecida na seção [fazendo backup do HgS](#backing-up-hgs) .

Além disso, é recomendável, mas não obrigatório, que você faça backup da lista de usuários autorizados a gerenciar o HGS em seu Active Directory domínio ou Active Directory em si.

Os backups devem ser feitos regularmente para garantir que as informações estejam atualizadas e armazenadas com segurança para evitar falsificação ou roubo.

Não é **recomendável** fazer backup ou tentar restaurar uma imagem de sistema inteira de um nó HgS.
Caso você tenha perdido todo o seu cluster, a prática recomendada é configurar um nó HGS totalmente novo e restaurar apenas o estado do HGS, não todo o sistema operacional do servidor.

#### <a name="recovering-from-the-loss-of-one-node"></a>Recuperando da perda de um nó
Se você perder um ou mais nós (mas não todos os nós) em seu cluster HGS, poderá simplesmente [adicionar nós ao cluster](guarded-fabric-configure-additional-hgs-nodes.md) seguindo as orientações no guia de implantação.
As políticas de atestado serão sincronizadas automaticamente, assim como quaisquer certificados que foram fornecidos para HGS como arquivos PFX com senhas que o acompanham.
Para certificados adicionados ao HGS usando uma impressão digital (certificados não exportáveis e com suporte a hardware, normalmente), você precisará garantir que cada novo nó tenha acesso à chave privada de cada certificado.

#### <a name="recovering-from-the-loss-of-the-entire-cluster"></a>Recuperando da perda do cluster inteiro
Se o cluster HGS inteiro ficar inativo e você não conseguir colocá-lo online novamente, será necessário restaurar o HGS a partir de um backup.
Restaurar o HGS de um backup envolve primeiro configurar um novo cluster HGS de acordo com as [diretrizes no guia de implantação](guarded-fabric-setting-up-the-host-guardian-service-hgs.md).
É altamente recomendável, mas não obrigatório, usar o mesmo nome de cluster ao configurar o ambiente de recuperação HGS para auxiliar na resolução de nomes dos hosts.
Usar o mesmo nome evita a reconfiguração de hosts com novas URLs de atestado e de proteção de chave.
Se você restaurou objetos para o Active Directory de backup de domínio HGS, é recomendável remover os objetos que representam o cluster HGS, computadores, conta de serviço e grupos de JEA antes de inicializar o servidor HGS.

Depois de configurar seu primeiro nó HGS (por exemplo, ele foi instalado e inicializado), você seguirá os procedimentos em [restaurando o HgS de um backup](#restoring-hgs-from-a-backup) para restaurar as políticas de atestado e as metades públicas dos certificados de proteção de chave.
Você precisará restaurar as chaves privadas para seus certificados manualmente de acordo com as diretrizes do seu provedor de certificado (por exemplo, importar o certificado no Windows ou configurar o acesso a certificados com suporte a HSM).
Depois que o primeiro nó for configurado, você poderá continuar [instalando nós adicionais no cluster](guarded-fabric-configure-additional-hgs-nodes.md) até atingir a capacidade e a resiliência que desejar.

### <a name="backing-up-hgs"></a>Fazendo backup de HGS
O administrador HGS deve ser responsável por fazer o backup de HGS regularmente.
Um backup completo conterá material de chave confidencial que deve ser adequadamente protegido.
Se uma entidade não confiável tiver acesso a essas chaves, ela poderá usar esse material para configurar um ambiente HGS mal-intencionado com a finalidade de comprometer VMs blindadas.

**Fazendo backup das políticas de atestado** Para fazer backup das políticas de atestado HGS, execute o comando a seguir em qualquer nó de servidor HGS em funcionamento.
Você será solicitado a fornecer uma senha.
Essa senha é usada para criptografar todos os certificados adicionados ao HGS usando um arquivo PFX (em vez de uma impressão digital de certificado).

```powershell
Export-HgsServerState -Path C:\temp\HGSBackup.xml
```

> [!NOTE]
> Se você estiver usando o atestado de administrador confiável, deverá fazer backup separadamente da Associação nos grupos de segurança usados pelo HGS para autorizar hosts protegidos.
> O HGS só fará o backup do SID dos grupos de segurança, não da Associação dentro deles.
> Caso esses grupos sejam perdidos durante um desastre, você precisará recriar os grupos e adicionar cada host protegido a eles novamente.

**Fazendo backup de certificados**

O `Export-HgsServerState` comando fará backup de qualquer certificado baseado em pfx adicionado ao HgS no momento em que o comando for executado.
Se você adicionou certificados ao HGS usando uma impressão digital (típica para certificados não exportáveis e com suporte a hardware), será necessário fazer backup manual das chaves privadas para seus certificados.
Para identificar quais certificados são registrados com o HGS e precisar fazer backup manual, execute o seguinte comando do PowerShell em qualquer nó de servidor HGS em funcionamento.

```powershell
Get-HgsKeyProtectionCertificate | Where-Object { $_.CertificateData.GetType().Name -eq 'CertificateReference' } | Format-Table Thumbprint, @{ Label = 'Subject'; Expression = { $_.CertificateData.Certificate.Subject } }
```

Para cada um dos certificados listados, será necessário fazer backup manual da chave privada.
Se você estiver usando um certificado baseado em software que não seja exportável, entre em contato com sua autoridade de certificação para garantir que eles tenham um backup de seu certificado e/ou possam refazê-lo sob demanda.
Para certificados criados e armazenados em módulos de segurança de hardware, você deve consultar a documentação do seu dispositivo para obter orientação sobre o planejamento da recuperação de desastre.

Você deve armazenar os backups de certificado junto com seus backups de política de atestado em um local seguro para que ambas as partes possam ser restauradas juntas.

**Configuração adicional para fazer backup**

O estado do servidor HGS com backup não incluirá o nome do seu cluster HGS, todas as informações de Active Directory ou quaisquer certificados SSL usados para proteger as comunicações com as APIs HGS.
Essas configurações são importantes para consistência, mas não são críticas para colocar seu cluster HGS online novamente após um desastre.

Para capturar o nome do serviço HGS, execute `Get-HgsServer` e anote o nome simples nas URLs atestado e proteção de chave.
Por exemplo, se a URL de atestado for " <http://hgs.contoso.com/Attestation> ", "HgS" é o nome do serviço HgS.

O domínio de Active Directory usado pelo HGS deve ser gerenciado como qualquer outro domínio de Active Directory.
Ao restaurar o HGS após um desastre, você não precisará necessariamente recriar os objetos exatos que estão presentes no domínio atual.
No entanto, isso facilitará a recuperação se você fizer backup Active Directory e manter uma lista dos usuários JEA autorizados a gerenciar o sistema, bem como a associação de todos os grupos de segurança usados pelo atestado confiável de administrador para autorizar hosts protegidos.

Para identificar a impressão digital dos certificados SSL configurados para HGS, execute o seguinte comando no PowerShell.
Em seguida, você pode fazer backup desses certificados SSL de acordo com as instruções do provedor de certificado.

```powershell
Get-WebBinding -Protocol https | Select-Object certificateHash
```

### <a name="restoring-hgs-from-a-backup"></a>Restaurando o HGS de um backup
As etapas a seguir descrevem como restaurar as configurações do HGS de um backup.
As etapas são relevantes para as duas situações em que você está tentando desfazer as alterações feitas em suas instâncias de HGS já em execução e quando você está acumulando um cluster HGS totalmente novo após uma perda completa do anterior.

#### <a name="set-up-a-replacement-hgs-cluster"></a>Configurar um cluster HGS de substituição
Antes de restaurar o HGS, você precisa ter um cluster HGS inicializado no qual você pode restaurar a configuração.
Se você estiver simplesmente Importando configurações que foram excluídas acidentalmente para um cluster existente (em execução), poderá ignorar esta etapa.
Se estiver recuperando de uma perda completa do HGS, será necessário instalar e inicializar pelo menos um nó HGS seguindo as [orientações no guia de implantação](guarded-fabric-setting-up-the-host-guardian-service-hgs.md).

Especificamente, você precisará:
1. [Configurar o domínio HgS](guarded-fabric-choose-where-to-install-hgs.md) ou ingressar no HgS para um domínio existente
2. [Inicialize o servidor HgS](guarded-fabric-initialize-hgs.md) usando as chaves existentes *ou* um conjunto de chaves temporárias. Você pode [remover as chaves temporárias](#renewing-or-replacing-keys) depois de importar as chaves reais dos arquivos de backup do HgS.
3. [Importar configurações do HgS](#import-settings-from-a-backup) do backup para restaurar os grupos de hosts confiáveis, as políticas de integridade de código, as linhas de base do TPM e os identificadores do TPM

> [!TIP]
> O novo cluster HGS não precisa usar os mesmos certificados, nome de serviço ou domínio que a instância HGS da qual o arquivo de backup foi exportado.

#### <a name="import-settings-from-a-backup"></a>Importar configurações de um backup

Para restaurar políticas de atestado, certificados baseados em PFX e as chaves públicas de certificados não PFX para o nó HGS de um arquivo de backup, execute o seguinte comando em um nó de servidor HGS inicializado.
Você será solicitado a inserir a senha que você especificou ao criar o backup.

```powershell
Import-HgsServerState -Path C:\Temp\HGSBackup.xml
```

Se você quiser importar políticas de atestado confiáveis do administrador ou políticas de atestado confiáveis do TPM, poderá fazer isso especificando os `-ImportActiveDirectoryModeState` `-ImportTpmModeState` sinalizadores ou para [Import-HgsServerState](https://technet.microsoft.com/library/mt652168.aspx).

Verifique se a atualização cumulativa mais recente do Windows Server 2016 está instalada antes de executar `Import-HgsServerState` .
A falha em fazer isso pode resultar em um erro de importação.

> [!NOTE]
> Se você restaurar políticas em um nó HGS existente que já tenha uma ou mais dessas políticas instaladas, o comando de importação mostrará um erro para cada política duplicada.
> Esse é um comportamento esperado e pode ser ignorado com segurança na maioria dos casos.

#### <a name="reinstall-private-keys-for-certificates"></a>Reinstalar chaves privadas para certificados
Se qualquer um dos certificados usados no HGS a partir do qual o backup foi criado foi adicionado usando impressões digitais, somente a chave pública desses certificados será incluída no arquivo de backup.
Isso significa que você precisará instalar manualmente e/ou conceder acesso às chaves privadas para cada um desses certificados antes que o HGS possa atender a solicitações de hosts Hyper-V.
As ações necessárias para concluir essa etapa variam dependendo de como o certificado foi emitido originalmente.
Para certificados com suporte de software emitidos por uma autoridade de certificação, você precisará entrar em contato com sua autoridade de certificação para obter a chave privada e instalá-la em **cada** nó HgS de acordo com suas instruções.
Da mesma forma, se os certificados tiverem suporte de hardware, você precisará consultar a documentação do fornecedor do módulo de segurança de hardware para instalar os drivers necessários em cada nó do HGS para se conectar ao HSM e conceder a cada computador acesso à chave privada.

Como lembrete, os certificados adicionados ao HGS usando impressões digitais exigem a replicação manual das chaves privadas para cada nó.
Você precisará repetir essa etapa em cada nó adicional que adicionar ao cluster HGS restaurado.

#### <a name="review-imported-attestation-policies"></a>Examinar as políticas de atestado importadas
Depois de importar as configurações de um backup, é recomendável examinar de forma minuciosa todas as políticas importadas usando o para garantir que `Get-HgsAttestationPolicy` apenas os hosts nos quais você confia para executar VMs blindadas sejam capazes de atestar com êxito.
Se você encontrar quaisquer políticas que não correspondam mais à sua postura de segurança, poderá [desabilitá-las ou removê-las](#review-attestation-policies).

#### <a name="run-diagnostics-to-check-system-state"></a>Executar o diagnóstico para verificar o estado do sistema
Depois de concluir a configuração e a restauração do estado do nó HGS, você deverá executar a ferramenta de diagnóstico HGS para verificar o estado do sistema.
Para fazer isso, execute o seguinte comando no nó HGS em que você restaurou a configuração:

```powershell
Get-HgsTrace -RunDiagnostics
```

Se o "resultado geral" não for "Pass", serão necessárias etapas adicionais para concluir a configuração do sistema.
Verifique as mensagens relatadas nos subtestes que falharam para obter mais informações.

## <a name="patching-hgs"></a>Aplicação de patch no HGS
É importante manter os nós do serviço guardião de host atualizados, instalando a atualização cumulativa mais recente quando ela sair. Se você estiver configurando um nó HGS totalmente novo, é altamente recomendável que você instale todas as atualizações disponíveis antes de instalar a função HGS ou configurá-la.
Isso garantirá que qualquer funcionalidade nova ou alterada entrará em vigor imediatamente.

Ao aplicar patch na malha protegida, é altamente recomendável que você atualize primeiro *todos os* hosts Hyper-V **antes de atualizar o HgS**.
Isso é para garantir que todas as alterações nas políticas de atestado no HGS sejam feitas *depois* que os hosts do Hyper-V tiverem sido atualizados para fornecer as informações necessárias para eles.
Se uma atualização for alterar o comportamento das políticas, elas não serão habilitadas automaticamente para evitar a interrupção da sua malha.
Essas atualizações exigem que você siga as diretrizes na seção a seguir para ativar as políticas de atestado novas ou alteradas.
Incentivamos você a ler as notas de versão do Windows Server e as atualizações cumulativas que instalar para verificar se as atualizações de política são necessárias.

### <a name="updates-requiring-policy-activation"></a>Atualizações que exigem a ativação da política
Se uma atualização para o HGS apresentar ou alterar significativamente o comportamento de uma política de atestado, será necessária uma etapa adicional para ativar a política alterada.
As alterações de política só são aplicadas após a exportação e a importação do estado do HGS.
Você só deve ativar as políticas novas ou alteradas depois de aplicar a atualização cumulativa a todos os hosts e a todos os nós HGS em seu ambiente.
Depois que cada computador tiver sido atualizado, execute os seguintes comandos em qualquer nó HGS para disparar o processo de atualização:

```powershell
$password = Read-Host -AsSecureString -Prompt "Enter a temporary password"
Export-HgsServerState -Path .\temporaryExport.xml -Password $password
Import-HgsServerState -Path .\temporaryExport.xml -Password $password
```

Se uma nova política foi introduzida, ela será desabilitada por padrão.
Para habilitar a nova política, primeiro localize-a na lista de políticas da Microsoft (prefixada com ' HGS_ ') e, em seguida, habilite-a usando os seguintes comandos:

```powershell
Get-HgsAttestationPolicy

Enable-HgsAttestationPolicy -Name <Hgs_NewPolicyName>
```

## <a name="managing-attestation-policies"></a>Gerenciando políticas de atestado
O HGS mantém várias políticas de atestado que definem o conjunto mínimo de requisitos que um host deve atender para ser considerado "íntegro" e ter permissão para executar VMs blindadas.
Algumas dessas políticas são definidas pela Microsoft, outras são adicionadas por você para definir as políticas de integridade de código permitidas, as linhas de base do TPM e os hosts em seu ambiente.
A manutenção regular dessas políticas é necessária para garantir que os hosts possam continuar atestando corretamente à medida que você os atualiza e substitui e para garantir que quaisquer configurações ou hosts não confiáveis sejam bloqueados de atestado com êxito.

Para atestado confiável de administrador, há apenas uma política que determina se um host está íntegro: Associação em um grupo de segurança conhecido e confiável.
O atestado de TPM é mais complicado e envolve várias políticas para medir o código e a configuração de um sistema antes de determinar se ele está íntegro.

Um único HGS pode ser configurado com as políticas Active Directory e TPM de uma só vez, mas o serviço verificará apenas as políticas para o modo atual para o qual ele está configurado quando um host tentar atestar.
Para verificar o modo do seu servidor HGS, execute `Get-HgsServer` .

### <a name="default-policies"></a>Políticas padrão
Para atestado confiável de TPM, há várias políticas internas configuradas no HGS.
Algumas dessas políticas são "bloqueadas", o que significa que elas não podem ser desabilitadas por motivos de segurança.
A tabela a seguir explica a finalidade de cada política padrão.

Nome da Política                    | Finalidade
-------------------------------|-----------------------------------------------------
Hgs_SecureBootEnabled          | Requer que os hosts tenham a inicialização segura habilitada. Isso é necessário para medir os binários de inicialização e outras configurações bloqueadas por UEFI.
Hgs_UefiDebugDisabled          | Garante que os hosts não tenham um depurador de kernel habilitado. Os depuradores de modo de usuário são bloqueados com políticas de integridade de código.
Hgs_SecureBootSettings         | Política negativa para garantir que os hosts correspondam a pelo menos uma (definida pelo administrador) linha de base TPM.
Hgs_CiPolicy                   | Política negativa para garantir que os hosts estejam usando uma das políticas de CI definidas pelo administrador.
Hgs_HypervisorEnforcedCiPolicy | Requer que a política de integridade de código seja imposta pelo hipervisor. Desabilitar esta política enfraquece as proteções contra ataques de política de integridade de código no modo kernel.
Hgs_FullBoot                   | Garante que o host não retomou a suspensão ou hibernação. Os hosts devem ser reiniciados corretamente ou desligados para passar essa política.
Hgs_VsmIdkPresent              | Requer que a segurança baseada em virtualização esteja em execução no host. O IDK representa a chave necessária para criptografar informações enviadas de volta para o espaço de memória segura do host.
Hgs_PageFileEncryptionEnabled  | Requer que o arquivo de paginações seja criptografado no host. Desabilitar essa política pode resultar em exposição de informações se um arquivo de paginação não criptografado for inspecionado quanto aos segredos do locatário.
Hgs_BitLockerEnabled           | Requer que o BitLocker esteja habilitado no host Hyper-V. Essa política é desabilitada por padrão por motivos de desempenho e não é recomendável para ser habilitada. Essa política não tem nenhuma influência sobre a criptografia das VMs blindadas em si.
Hgs_IommuEnabled               | Requer que o host tenha um dispositivo IOMMU em uso para evitar ataques de acesso direto à memória. Desabilitar essa política e usar hosts sem uma IOMMU habilitada pode expor segredos de VM de locatário para ataques de memória direta.
Hgs_NoHibernation              | Requer que a hibernação seja desabilitada no host Hyper-V. Desabilitar essa política pode permitir que os hosts salvem a memória da VM blindada em um arquivo de hibernação não criptografado.
Hgs_NoDumps                    | Requer que os despejos de memória sejam desabilitados no host Hyper-V. Se você desabilitar essa política, é recomendável configurar a criptografia de despejo para impedir que a memória da VM blindada seja salva em arquivos de despejo de falha não criptografados.
Hgs_DumpEncryption             | Requer despejos de memória, se habilitado no host Hyper-V, para serem criptografados com uma chave de criptografia confiável pelo HGS. Essa política não se aplicará se os despejos não estiverem habilitados no host. Se essa política e o *HgS \_ nodespejos* estiverem desabilitados, a memória da VM blindada poderá ser salva em um arquivo de despejo não criptografado.
Hgs_DumpEncryptionKey          | Política negativa para garantir que os hosts configurados para permitir despejos de memória estejam usando uma chave de criptografia de arquivo de despejo definida pelo administrador conhecida como HGS. Esta política não se aplica quando *o \_ DumpEncryption HgS* está desabilitado.

### <a name="authorizing-new-guarded-hosts"></a>Autorizando novos hosts protegidos
Para autorizar um novo host a se tornar um host protegido (por exemplo, atestado com êxito), o HGS deve confiar no host e (quando configurado para usar o atestado confiável do TPM) o software em execução.
As etapas para autorizar um novo host diferem com base no modo de atestado para o qual o HGS está configurado no momento.
Para verificar o modo de atestado para sua malha protegida, execute `Get-HgsServer` em qualquer nó HgS.

#### <a name="software-configuration"></a>Configuração de software
No novo host Hyper-V, verifique se o Windows Server 2016 Datacenter Edition está instalado.
O Windows Server 2016 Standard não pode executar VMs blindadas em uma malha protegida.
O host pode estar instalado na experiência desktop ou no Server Core.

No servidor com a experiência desktop e o Server Core, você precisa instalar as funções de servidor de suporte do Hyper-V e de guardião de host de Hyper-v:

```powershell
Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
```

#### <a name="admin-trusted-attestation"></a>Administrador-atestado confiável
Para registrar um novo host no HGS ao usar o atestado confiável de administrador, você deve primeiro adicionar o host a um grupo de segurança no domínio ao qual ele está associado.
Normalmente, cada domínio terá um grupo de segurança para hosts protegidos.
Se você já tiver registrado esse grupo com HGS, a única ação que precisa ser executada é reiniciar o host para atualizar sua associação de grupo.

Você pode verificar quais grupos de segurança são confiáveis pelo HGS executando o seguinte comando:

```powershell
Get-HgsAttestationHostGroup
```

Para registrar um novo grupo de segurança com o HGS, primeiro Capture o SID (identificador de segurança) do grupo no domínio do host e registre o SID com o HGS.

```powershell
Add-HgsAttestationHostGroup -Name "Contoso Guarded Hosts" -Identifier "S-1-5-21-3623811015-3361044348-30300820-1013"
```

As instruções sobre como configurar a confiança entre o domínio de host e o HGS estão disponíveis no guia de implantação.

#### <a name="tpm-trusted-attestation"></a>TPM-atestado confiável
Quando o HGS é configurado no modo TPM, os hosts devem passar por todas as políticas bloqueadas e políticas "habilitadas" prefixadas com "Hgs_", bem como pelo menos uma linha de base do TPM, um identificador TPM e uma política de integridade de código.
Cada vez que você adicionar um novo host, será necessário registrar o novo identificador de TPM com HGS.
Contanto que o host esteja executando o mesmo software (e tenha a mesma política de integridade de código aplicada) e a linha de base do TPM como outro host em seu ambiente, você não precisará adicionar novas políticas de CI ou linhas de base.

**Adicionando o identificador TPM a um novo host** No novo host, execute o comando a seguir para capturar o identificador do TPM.
Certifique-se de especificar um nome exclusivo para o host que o ajudará a procurar no HGS.
Você precisará dessas informações se encerrar o host ou quiser impedir que ele execute VMs blindadas no HGS.

```powershell
(Get-PlatformIdentifier -Name "Host01").InnerXml | Out-File C:\temp\host01.xml -Encoding UTF8
```

Copie esse arquivo para o servidor HGS e execute o comando a seguir para registrar o host com o HGS.

```powershell
Add-HgsAttestationTpmHost -Name 'Host01' -Path C:\temp\host01.xml
```

**Adicionando uma nova linha de base do TPM** Se o novo host estiver executando uma nova configuração de hardware ou firmware para seu ambiente, talvez seja necessário obter uma nova linha de base do TPM.
Para fazer isso, execute o seguinte comando no host.

```powershell
Get-HgsAttestationBaselinePolicy -Path 'C:\temp\hardwareConfig01.tcglog'
```

> [!NOTE]
> Se você receber um erro informando que o host falhou na validação e não será atestado com êxito, não se preocupe.
> Essa é uma verificação de pré-requisitos para garantir que o host possa executar VMs blindadas e, provavelmente, significa que você ainda não aplicou uma política de integridade de código ou outra configuração necessária.
> Leia a mensagem de erro, faça as alterações sugeridas por ela e tente novamente.
> Como alternativa, você pode ignorar a validação no momento adicionando o `-SkipValidation` sinalizador ao comando.

Copie a linha de base do TPM para o servidor HGS e registre-a com o comando a seguir.
Incentivamos você a usar uma Convenção de nomenclatura que ajuda a entender a configuração de hardware e firmware dessa classe de host Hyper-V.

```powershell
Add-HgsAttestationTpmPolicy -Name 'HardwareConfig01' -Path 'C:\temp\hardwareConfig01.tcglog'
```

**Adicionando uma nova política de integridade de código** Se você alterou a política de integridade de código em execução nos hosts do Hyper-V, será necessário registrar a nova política com HGS antes que esses hosts possam ser atestados com êxito.
Em um host de referência, que serve como uma imagem mestra para computadores Hyper-V confiáveis em seu ambiente, Capture uma nova política de CI usando o `New-CIPolicy` comando.
Incentivamos você a usar o nível de **filepublisher** e o fallback de **hash** para políticas de CI de host do Hyper-V.
Primeiro, você deve criar uma política de CI no modo de auditoria para garantir que tudo esteja funcionando conforme o esperado.
Depois de validar uma carga de trabalho de exemplo no sistema, você pode impor a política e copiar a versão imposta para HGS.
Para obter uma lista completa das opções de configuração de política de integridade de código, consulte a [documentação do Device Guard](https://technet.microsoft.com/itpro/windows/keep-secure/deploy-device-guard-deploy-code-integrity-policies).

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

Depois de criar a política, testada e imposta, copie o arquivo binário (. p7b) para o servidor HGS e registre a política.

```powershell
Add-HgsAttestationCiPolicy -Name 'WS2016-Hardware01' -Path 'C:\temp\ws2016-hardware01-ci.p7b'
```

**Adicionando uma chave de criptografia de despejo de memória**

Quando a política do *HgS \_ nodumps* está desabilitada e a política *HgS \_ DumpEncryption* está habilitada, os hosts protegidos têm permissão para que os despejos de memória (incluindo despejos de falhas) sejam habilitados, desde que esses despejos sejam criptografados. Os hosts protegidos passarão por atestado apenas se tiverem despejos de memória desabilitados ou estiverem criptografando-os com uma chave conhecida por HGS. Por padrão, nenhuma chave de criptografia de despejo é configurada no HGS.

Para adicionar uma chave de criptografia de despejo ao HGS, use o `Add-HgsAttestationDumpPolicy` cmdlet para fornecer o HgS com o hash de sua chave de criptografia de despejo.
Se você capturar uma linha de base do TPM em um host Hyper-V configurado com a criptografia de despejo, o hash será incluído no tcglog e poderá ser fornecido ao `Add-HgsAttestationDumpPolicy` cmdlet.

```powershell
Add-HgsAttestationDumpPolicy -Name 'DumpEncryptionKey01' -Path 'C:\temp\TpmBaselineWithDumpEncryptionKey.tcglog'
```

Como alternativa, você pode fornecer diretamente a representação de cadeia de caracteres do hash para o cmdlet.

```powershell
Add-HgsAttestationDumpPolicy -Name 'DumpEncryptionKey02' -PublicKeyHash '<paste your hash here>'
```

Certifique-se de adicionar cada chave de criptografia de despejo exclusivo ao HGS se você optar por usar chaves diferentes em sua malha protegida.
Os hosts que estão criptografando despejos de memória com uma chave que não seja conhecida como HGS não passarão por atestado.

Consulte a documentação do Hyper-V para obter mais informações sobre como [Configurar a criptografia de despejo em hosts](../../virtualization/hyper-v/manage/about-dump-encryption.md).

#### <a name="check-if-the-system-passed-attestation"></a>Verifique se o sistema passou por atestado
Depois de registrar as informações necessárias com o HGS, você deve verificar se o host passa por atestado.
No host do Hyper-V recém-adicionado, execute `Set-HgsClientConfiguration` e forneça as URLs corretas para o cluster HgS.
Essas URLs podem ser obtidas executando `Get-HgsServer` em qualquer nó HgS.

```powershell
Set-HgsClientConfiguration -KeyProtectionServerUrl 'http://hgs.bastion.local/KeyProtection' -AttestationServerUrl 'http://hgs.bastion.local/Attestation'
```

Se o status resultante não indicar "IsHostGuarded: true", você precisará solucionar o problema da configuração.
No host que falhou o atestado, execute o seguinte comando para obter um relatório detalhado sobre problemas que podem ajudá-lo a resolver o atestado com falha.

```powershell
Get-HgsTrace -RunDiagnostics -Detailed
```

> [!IMPORTANT]
> Se você estiver usando o Windows Server 2019 ou o Windows 10, a versão 1809 e estiver usando políticas de integridade de código, `Get-HgsTrace` o poderá retornar uma falha para o diagnóstico **ativo da política de integridade de código** .
> Você pode ignorar esse resultado com segurança quando ele for o único diagnóstico com falha.

### <a name="review-attestation-policies"></a>Examinar políticas de atestado
Para examinar o estado atual das políticas configuradas no HGS, execute os seguintes comandos em qualquer nó HGS:

```powershell
# List all trusted security groups for admin-trusted attestation
Get-HgsAttestationHostGroup

# List all policies configured for TPM-trusted attestation
Get-HgsAttestationPolicy
```

Se você encontrar uma política habilitada que não atende mais ao seu requisito de segurança (por exemplo, uma política de integridade de código antiga que agora é considerada insegura), você poderá desabilitá-la substituindo o nome da política no seguinte comando:

```powershell
Disable-HgsAttestationPolicy -Name 'PolicyName'
```

Da mesma forma, você pode usar `Enable-HgsAttestationPolicy` o para reabilitar uma política.

Se você não precisar mais de uma política e quiser removê-la de todos os nós HGS, execute `Remove-HgsAttestationPolicy -Name 'PolicyName'` para excluir permanentemente a política.

## <a name="changing-attestation-modes"></a>Alterando modos de atestado
Se você iniciou sua malha protegida usando o atestado de administrador confiável, provavelmente desejará atualizar para o modo de atestado do TPM muito mais forte assim que tiver hosts compatíveis com o TPM 2,0 suficiente em seu ambiente.
Quando estiver pronto para alternar, você poderá pré-carregar todos os artefatos de atestado (políticas de CI, linhas de base do TPM e identificadores de TPM) no HGS enquanto continua a executar o HGS com atestado confiável de administrador.
Para fazer isso, basta seguir a instrução na seção [autorizando um novo host protegido](#authorizing-new-guarded-hosts) .

Depois de adicionar todas as suas políticas ao HGS, a próxima etapa é executar uma tentativa de atestado sintética em seus hosts para ver se eles passarão por atestado no modo TPM.
Isso não afeta o estado operacional atual do HGS.
Os comandos a seguir devem ser executados em um computador que tenha acesso a todos os hosts no ambiente e a pelo menos um nó HGS.
Se o firewall ou outras políticas de segurança impedirem isso, você poderá ignorar esta etapa.
Quando possível, é recomendável executar o atestado sintético para fornecer uma boa indicação se a "inversão" para o modo do TPM causará tempo de inatividade para suas VMs.

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

Após a conclusão do diagnóstico, examine as informações de saída para determinar se os hosts teriam atestado com falha no modo TPM.
Execute novamente o diagnóstico até obter uma "passagem" de cada host e, em seguida, vá para alterar o HGS para o modo TPM.

A **alteração para o modo do TPM** leva apenas um segundo para ser concluída.
Execute o comando a seguir em qualquer nó HGS para atualizar o modo de atestado.

```powershell
Set-HgsServer -TrustTpm
```

Se você tiver problemas e precisar voltar para o modo de Active Directory, poderá fazer isso executando `Set-HgsServer -TrustActiveDirectory` .

Depois de confirmar que tudo está funcionando conforme o esperado, remova todos os grupos de hosts Active Directory confiáveis do HGS e remova a relação de confiança entre os domínios HGS e de malha.
Se você deixar a Active Directory confiança em vigor, você arriscará alguém reabilitando a relação de confiança e alternando o HGS para o modo de Active Directory, o que pode permitir que um código não confiável seja executado em seus hosts protegidos.

## <a name="key-management"></a>Gerenciamento de chaves
A solução de malha protegida usa vários pares de chaves públicas/privadas para validar a integridade de vários componentes na solução e criptografar os segredos do locatário.
O serviço guardião de host é configurado com pelo menos dois certificados (com chaves públicas e privadas), que são usados para assinar e criptografar as chaves usadas para iniciar as VMs blindadas.
Essas chaves devem ser gerenciadas com cuidado.
Se a chave privada for adquirida por um adversário, ela poderá desproteger as VMs em execução na sua malha ou configurar um cluster HGS impostor que usa políticas de atestado mais fracas para ignorar as proteções que você colocou no local.
Se você perder as chaves privadas durante um desastre e não encontrá-las em um backup, precisará configurar um novo par de chaves e ter cada VM reinserida para autorizar seus novos certificados.

Esta seção aborda tópicos gerais de gerenciamento de chaves para ajudá-lo a configurar suas chaves para que elas sejam funcionais e seguras.

### <a name="adding-new-keys"></a>Adicionando novas chaves
Embora o HGS precise ser inicializado com um conjunto de chaves, você pode adicionar mais de uma chave de criptografia e assinatura ao HGS.
Os dois motivos mais comuns para adicionar novas chaves ao HGS são:
1. Para dar suporte a cenários "Traga sua própria chave" onde os locatários copiam suas chaves privadas para o módulo de segurança de hardware e autorizam apenas suas chaves para iniciar suas VMs blindadas.
2. Para substituir as chaves existentes para HGS, primeiro adicione as novas chaves e mantendo os dois conjuntos de chaves até que cada configuração de VM seja atualizada para usar as novas chaves.

O processo para adicionar as novas chaves difere com base no tipo de certificado que você está usando.

**Opção 1: adicionar um certificado armazenado em um HSM**

Nossa abordagem recomendada para proteger chaves HGS é usar certificados criados em um módulo de segurança de hardware (HSM).
Os HSMs garantem que o uso de suas chaves esteja vinculado ao acesso físico a um dispositivo sensível à segurança em seu datacenter.
Cada HSM é diferente e tem um processo exclusivo para criar certificados e registrá-los com o HGS.
As etapas a seguir destinam-se a fornecer uma orientação aproximada para o uso de certificados com suporte do HSM.
Consulte a documentação do fornecedor do HSM para obter as etapas e os recursos exatos.

1. Instale o software HSM em cada nó HGS no cluster. Dependendo se você tiver uma rede ou um dispositivo HSM local, talvez seja necessário configurar o HSM para conceder acesso ao seu computador ao seu repositório de chaves.
2. Criar dois certificados no HSM com **chaves RSA de 2048 bits** para criptografia e assinatura
    1. Criar um certificado de criptografia com a propriedade de uso da chave de **codificação de dados** no HSM
    2. Criar um certificado de assinatura com a propriedade de uso de chave de **assinatura digital** em seu HSM
3. Instale os certificados no repositório de certificados local de cada nó do HGS de acordo com as diretrizes do seu fornecedor HSM.
4. Se o HSM usar permissões granulares para conceder permissão a aplicativos específicos ou usuários para usar a chave privada, você precisará conceder acesso à conta de serviço gerenciado de grupo HGS ao certificado. Você pode encontrar o nome da conta do gMSA HGS executando`(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName`
5. Adicione os certificados de assinatura e criptografia ao HGS substituindo as impressões digitais por aquelas dos seus certificados nos seguintes comandos:

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899"
    Add-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint "99887766554433221100FFEEDDCCBBAA"
    ```

**Opção 2: adicionando certificados de software não exportáveis**

Se você tiver um certificado com suporte de software emitido por uma autoridade de certificação pública ou de sua empresa que tenha uma chave privada não exportável, será necessário adicionar seu certificado ao HGS usando sua impressão digital.
1. Instale o certificado em seu computador de acordo com as instruções da autoridade de certificação.
2. Conceda à conta de serviço gerenciado do grupo HGS acesso de leitura à chave privada do certificado. Você pode encontrar o nome da conta do gMSA HGS executando`(Get-IISAppPool -Name KeyProtection).ProcessModel.UserName`
3. Registre o certificado com HGS usando o comando a seguir e substituindo na impressão digital do certificado (altere a *criptografia* para *assinar* certificados de assinatura):

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899"
    ```

> [!IMPORTANT]
> Será necessário instalar manualmente a chave privada e conceder acesso de leitura à conta gMSA em cada nó HGS.
> O HGS não pode replicar automaticamente chaves privadas para *qualquer* certificado registrado por sua impressão digital.

**Opção 3: adicionar certificados armazenados em arquivos PFX**

Se você tiver um certificado com suporte de software com uma chave privada exportável que possa ser armazenada no formato de arquivo PFX e protegido por uma senha, o HGS poderá gerenciar automaticamente seus certificados para você.
Os certificados adicionados com arquivos PFX são replicados automaticamente para cada nó do cluster HGS e o HGS protege o acesso às chaves privadas.
Para adicionar um novo certificado usando um arquivo PFX, execute os seguintes comandos em qualquer nó HGS (altere a *criptografia* para *assinar* certificados):

```powershell
$certPassword = Read-Host -AsSecureString -Prompt "Provide the PFX file password"
Add-HgsKeyProtectionCertificate -CertificateType Encryption -CertificatePath "C:\temp\encryptionCert.pfx" -CertificatePassword $certPassword
```

**Identificando e alterando os certificados primários** Embora o HGS possa dar suporte a vários certificados de assinatura e criptografia, ele usa um par como seus certificados "primários".
Esses são os certificados que serão usados se alguém baixar os metadados do guardião para esse cluster HGS.
Para verificar quais certificados estão marcados atualmente como seus certificados primários, execute o seguinte comando:

```powershell
Get-HgsKeyProtectionCertificate -IsPrimary $true
```

Para definir uma nova criptografia primária ou certificado de autenticação, localize a impressão digital do certificado desejado e marque-a como primária usando os seguintes comandos:

```powershell
Get-HgsKeyProtectionCertificate
Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint "AABBCCDDEEFF00112233445566778899" -IsPrimary
Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint "99887766554433221100FFEEDDCCBBAA" -IsPrimary
```

### <a name="renewing-or-replacing-keys"></a>Renovando ou substituindo chaves
Quando você criar os certificados usados pelo HGS, os certificados receberão uma data de expiração de acordo com a política da autoridade de certificação e suas informações de solicitação.
Normalmente, em cenários em que a validade do certificado é importante, como a proteção de comunicações HTTP, os certificados devem ser renovados antes de expirarem para evitar uma interrupção do serviço ou uma mensagem de erro preocupante.
O HGS não usa certificados nesse sentido.
O HGS está simplesmente usando certificados como uma maneira conveniente de criar e armazenar um par de chaves assimétricas.
Um certificado de criptografia ou autenticação expirado no HGS não indica um ponto fraco ou perda de proteção para VMs blindadas.
Além disso, as verificações de revogação de certificado não são executadas pelo HGS.
Se um certificado HGS ou o certificado de uma autoridade de emissão for revogado, ele não afetará o uso de HGS do certificado.

A única vez que você precisa se preocupar com um certificado HGS é se você tem motivo para acreditar que sua chave privada foi roubada.
Nesse caso, a integridade de suas VMs blindadas está em risco porque a posse da metade privada do par de chaves de criptografia e assinatura HGS é suficiente para remover as proteções de blindagem em uma VM ou para criar um servidor HGS falso que tenha políticas de atestado mais fracas.

Se você se deparar com essa situação ou for exigido pelos padrões de conformidade para atualizar as chaves de certificado regularmente, as etapas a seguir descrevem o processo para alterar as chaves em um servidor HGS.
Observe que as diretrizes a seguir representam uma tarefa significativa que resultará em uma interrupção do serviço para cada VM servida pelo cluster HGS.
O planejamento adequado para alterar chaves HGS é necessário para minimizar a interrupção do serviço e garantir a segurança das VMs de locatário.

Em um nó HGS, execute as seguintes etapas para registrar um novo par de certificados de criptografia e autenticação.
Consulte a seção sobre [adição de novas chaves](#adding-new-keys) para obter informações detalhadas de várias maneiras de adicionar novas chaves ao HgS.

1. Crie um novo par de certificados de criptografia e autenticação para o servidor HGS. Idealmente, eles serão criados em um módulo de segurança de hardware.

2. Registre a nova criptografia e certificados de autenticação com **Add-HgsKeyProtectionCertificate**

    ```powershell
    Add-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint>
    Add-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint>
    ```

3. Se você usou impressões digitais, precisará ir para cada nó no cluster para instalar a chave privada e conceder a gMSA do HGS acesso à chave.

4. Tornar os novos certificados os certificados padrão no HGS

    ```powershell
    Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint> -IsPrimary
    Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint> -IsPrimary
    ```

Neste ponto, os dados de blindagem criados com metadados obtidos do nó HGS usarão os novos certificados, mas as VMs existentes continuarão funcionando porque os certificados mais antigos ainda estão lá.

Para garantir que todas as VMs existentes funcionem com as novas chaves, será necessário atualizar o protetor de chave em cada VM.

Essa é uma ação que requer que o proprietário da VM (pessoa ou entidade em posse do guardião de "proprietário") esteja envolvido. Para cada VM blindada, execute as seguintes etapas:

1. Desligue a VM. A VM não pode ser reativada até que as etapas restantes sejam concluídas ou, caso contrário, será necessário iniciar o processo novamente.

2. Salve o protetor de chave atual em um arquivo:`Get-VMKeyProtector -VMName 'VM001' | Out-File '.\VM001.kp'`

3. Transferir o KP para o proprietário da VM

4. Fazer com que o proprietário Baixe as informações do guardião atualizadas do HGS e importe-as em seu sistema local

5. Leia o KP atual na memória, conceda o novo acesso do guardião ao KP e salve-o em um novo arquivo executando os seguintes comandos:

    ```powershell
    $kpraw = Get-Content -Path .\VM001.kp
    $kp = ConvertTo-HgsKeyProtector -Bytes $kpraw
    $newGuardian = Get-HgsGuardian -Name 'UpdatedHgsGuardian'
    $updatedKP = Grant-HgsKeyProtectorAccess -KeyProtector $kp -Guardian $newGuardian
    $updatedKP.RawData | Out-File .\updatedVM001.kp
    ```

6. Copie o KP atualizado de volta para a malha de hospedagem.

7. Aplique o KP à VM original:

   ```powershell
   $updatedKP = Get-Content -Path .\updatedVM001.kp
   Set-VMKeyProtector -VMName VM001 -KeyProtector $updatedKP
   ```

8. Por fim, inicie a VM e certifique-se de que ela seja executada com êxito.

    > [!NOTE]
    > Se o proprietário da VM definir um protetor de chave incorreto na VM e não autorizar sua malha para executar a VM, você não poderá iniciar a VM blindada.
    > Para retornar ao último protetor de chave válido, execute`Set-VMKeyProtector -RestoreLastKnownGoodKeyProtector`

    Depois que todas as VMs forem atualizadas para autorizar as novas chaves do guardião, você poderá desabilitar e remover as chaves antigas.

9.  Obter as impressões digitais dos certificados antigos de`Get-HgsKeyProtectionCertificate -IsPrimary $false`

10. Desabilite cada certificado executando os seguintes comandos:

    ```powershell
    Set-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint> -IsEnabled $false
    Set-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint> -IsEnabled $false
    ```

11. Depois de garantir que as VMs ainda possam começar com os certificados desabilitados, remova os certificados do HGS executando os seguintes comandos:

    ```powershell
    Remove-HgsKeyProtectionCertificate -CertificateType Signing -Thumbprint <Thumbprint>`
    Remove-HgsKeyProtectionCertificate -CertificateType Encryption -Thumbprint <Thumbprint>`
    ```

> [!IMPORTANT]
> Os backups de VM conterão informações antigas do protetor de chave que permitem que os certificados antigos sejam usados para iniciar a VM.
> Se você estiver ciente de que sua chave privada foi comprometida, você deve assumir que os backups de VM também podem ser comprometidos e tomar as devidas providências.
> A destruição da configuração da VM dos backups (. vmcx) removerá os protetores de chave, ao custo da necessidade de usar a senha de recuperação do BitLocker para inicializar a VM na próxima vez.

### <a name="key-replication-between-nodes"></a>Replicação de chave entre nós
Cada nó no cluster HGS deve ser configurado com os mesmos certificados SSL de criptografia, assinatura e (quando configurados).
Isso é necessário para garantir que os hosts do Hyper-V que chegam a qualquer nó no cluster possam ter suas solicitações atendidas com êxito.

**Se você tiver inicializado o servidor HgS com certificados baseados em pfx** , o HgS replicará automaticamente a chave pública e privada desses certificados em todos os nós no cluster.
Você só precisa adicionar as chaves em um nó.

**Se você tiver inicializado o servidor HgS com referências de certificado** ou impressões digitais, o HgS replicará apenas a chave *pública* no certificado para cada nó.
Além disso, o HGS não pode conceder a si próprio o acesso à chave privada em qualquer nó nesse cenário.
Portanto, é sua responsabilidade:
1. Instalar a chave privada em cada nó HGS
2. Conceder ao grupo HGS acesso de conta de serviço gerenciado (gMSA) à chave privada em cada nó essas tarefas adicionam carga operacional extra, no entanto, são necessárias para certificados e chaves com suporte do HSM com chaves privadas não exportáveis.

Os **certificados SSL** nunca são replicados em nenhum formato.
É sua responsabilidade inicializar cada servidor HGS com o mesmo certificado SSL e atualizar cada servidor sempre que você optar por renovar ou substituir o certificado SSL.
Ao substituir o certificado SSL, é recomendável que você faça isso usando o cmdlet [set-HgsServer](https://technet.microsoft.com/library/mt652180.aspx) .

## <a name="unconfiguring-hgs"></a>Desconfigurando HGS

Se você precisar encerrar ou reconfigurar significativamente um servidor HGS, poderá fazer isso usando os cmdlets [Clear-hgsserver](https://technet.microsoft.com/library/mt652176.aspx) ou [Uninstall-hgsserver](https://technet.microsoft.com/library/mt652182.aspx) .

### <a name="clearing-the-hgs-configuration"></a>Limpando a configuração do HGS

Para remover um nó do cluster HGS, use o cmdlet [Clear-HgsServer](https://technet.microsoft.com/library/mt652176.aspx) .
Esse cmdlet fará as seguintes alterações no servidor onde ele é executado:

- Cancela o registro do atestado e dos serviços de proteção de chave
- Remove o ponto de extremidade de gerenciamento de JEA "Microsoft. Windows. HgS"
- Remove o computador local do cluster de failover do HGS

Se o servidor for o último nó HGS no cluster, o cluster e seu recurso de nome de rede distribuída correspondente também serão destruídos.

```powershell
# Removes the local computer from the HGS cluster
Clear-HgsServer
```

Após a conclusão da operação de limpeza, o servidor HGS pode ser inicializado novamente com [Initialize-HgsServer](https://technet.microsoft.com/library/mt652185.aspx).
Se você usou [install-HgsServer](https://technet.microsoft.com/library/mt652169.aspx) para configurar um domínio de Active Directory Domain Services, esse domínio permanecerá configurado e operacional após a operação de limpeza.

### <a name="uninstalling-hgs"></a>Desinstalando o HGS

Se você quiser remover um nó do cluster HGS **e** rebaixar o controlador de domínio do Active Directory em execução nele, use o cmdlet [Uninstall-HgsServer](https://technet.microsoft.com/library/mt652182.aspx) .
Esse cmdlet fará as seguintes alterações no servidor onde ele é executado:

- Cancela o registro do atestado e dos serviços de proteção de chave
- Remove o ponto de extremidade de gerenciamento de JEA "Microsoft. Windows. HgS"
- Remove o computador local do cluster de failover do HGS
- Rebaixa o controlador de Domínio do Active Directory, se configurado

Se o servidor for o último nó HGS no cluster, o domínio, o cluster de failover e o recurso de nome de rede distribuída do cluster também serão destruídos.

```powershell
# Removes the local computer from the HGS cluster and demotes the ADDC (restart required)
$newLocalAdminPassword = Read-Host -AsSecureString -Prompt "Enter a new password for the local administrator account"
Uninstall-HgsServer -LocalAdministratorPassword $newLocalAdminPassword -Restart
```

Depois que a operação de desinstalação for concluída e o computador tiver sido reiniciado, você poderá reinstalar o ADDC e o HGS usando [install-hgsserver](https://technet.microsoft.com/library/mt652169.aspx) ou ingressar o computador em um domínio e inicializar o servidor HgS nesse domínio com [Initialize-hgsserver](https://technet.microsoft.com/library/mt652185.aspx).

Se você não pretende mais usar o computador como um nó HGS, você pode remover a função do Windows.

```powershell
Uninstall-WindowsFeature HostGuardianServiceRole
```