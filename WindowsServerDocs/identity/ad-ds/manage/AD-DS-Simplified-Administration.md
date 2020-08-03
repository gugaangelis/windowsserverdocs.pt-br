---
ms.assetid: f74eec9a-2485-4ee0-a0d8-cce01250a294
title: Administração simplificada do AD DS
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 4358f48b2373ee0c521c3970c4cb235a0d19dfca
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87519033"
---
# <a name="ad-ds-simplified-administration"></a>Administração simplificada do AD DS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico explica os recursos e os benefícios da implantação e da administração do controlador de domínio do Windows Server 2012 e as diferenças entre a implantação do DC do sistema operacional anterior e a nova implementação do Windows Server 2012.

O Windows Server 2012 introduziu a próxima geração de Active Directory Domain Services a administração simplificada e foi o reprovisionamento de domínio mais radical desde o Windows 2000 Server. A Administração Simplificada do AD DS traz lições aprendidas de 12 anos de Active Directory e torna a experiência administrativa mais flexível, mais intuitiva e com maior capacidade de suporte para arquitetos e administradores. Isso significa criar novas versões de tecnologias existentes, bem como estender as funcionalidades dos componentes liberados no Windows Server 2008 R2.

A Administração Simplificada do AD DS é uma reformulação da imagem de implantação do domínio.

- A implantação de função do AD DS agora é parte da nova arquitetura do Gerenciador do Servidor e permite instalação remota.
- O mecanismo de implantação e configuração do AD DS agora é o Windows PowerShell, mesmo ao usar o novo Assistente de configuração do AD DS
- A extensão do esquema, preparação da floresta e preparação de domínio são automaticamente parte da promoção do controlador de domínio e não requerem mais tarefas separadas em servidores especiais, como o mestre de esquema
- A promoção agora inclui a verificação de pré-requisitos que valida a floresta e a prontidão do domínio para o novo controlador de domínio, diminuindo as chances de falha nas promoções.
- O módulo do Active Directory para o Windows PowerShell agora inclui cmdlets para gerenciamento de topologia de replicação, Controle de acesso dinâmico e outras operações.
- O nível funcional da floresta do Windows Server 2012 não implementa novos recursos e o nível funcional de domínio é necessário somente para um subconjunto de novos recursos Kerberos, aliviando os administradores das necessidades frequentes por um ambiente de controlador de domínio mais homogêneo.
- Suporte integral adicionado aos controladores de domínio virtualizados, para incluir implantação automatizada e proteção de reversão
   - Para obter mais informações sobre controladores de domínio virtualizados, consulte [introdução ao Active Directory Domain Services &#40;AD DS&#41; virtualização &#40;nível 100&#41;](../../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md).

Além disso, há ainda muitas melhorias de manutenção e administrativas:

- O Centro Administrativo do Active Directory inclui uma lixeira gráfica do Active Directory, o gerenciamento de política de senha refinada e o visualizador de histórico do Windows PowerShell
- O novo Gerenciador de Servidor possui interfaces específicas do AD DS no monitoramento de desempenho, análises de melhores recomendadas, serviços críticos e os logs de evento
- As contas de serviço gerenciado de grupo ofereceem suporte a vários computadores usando as mesmas entidades de segurança
- Melhorias na emissão do identificador relativo (RID) e monitoramento para melhor capacidade de gerenciamento em domínios maduros do Active Directory

AD DS lucros de outros novos recursos incluídos no Windows Server 2012, como:

- Agrupamento NIC e Ponte de Datacenter
- Segurança DNS e disponibilidade de zona integrada AD mais rápida após a inicialização
- Melhorias de confiabilidade e escalabilidade do Hyper-V
- Desbloqueio de Rede do BitLocker
- Módulos de administração do componente Windows PowerShell adicionais

## <a name="adprep-integration"></a>Integração ADPREP

A extensão de esquema da floresta do Active Directory e a preparação de domínio agora integram o processo de configuração do controlador de domínio. Se você promover um novo controlador de domínio em uma floresta existente, o processo detecta o status de atualização e a extensão do esquema e também as fases de preparação do domínio ocorrem automaticamente. O usuário que instalar pela primeira vez o controlador de domínio do Windows Server 2012 ainda deve ser um Admin Corporativo e Admin. do Esquema ou deve fornecer credenciais alternativas válidas.

O Adprep.exe permanece no DVD para preparação de floresta e domínio separados. A versão da ferramenta incluída com o Windows Server 2012 é compatível com versões anteriores do Windows Server 2008 x64 e do Windows Server 2008 R2. O Adprep.exe também dá suporte a forestprep e domainprep remotos, assim como ferramentas de configuração do controlador de domínio baseadas em ADDSDeployment.

Para mais informações sobre Adprep e a preparação da floresta de sistema operacional anterior, consulte [Executando o Adprep (Windows Server 2008 R2)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd464018(v=ws.10)).

## <a name="server-manager-ad-ds-integration"></a>Integração de AD DS do Gerenciador do Servidor

![administração simplificada](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_Dashboard.png)

O Gerenciador do Servidor atua como um hub para tarefas de gerenciamento do servidor. Sua aparência em estilo de painel atualiza periodicamente as exibições de funções instaladas e grupos de servidores remotos. O Gerenciador do Servidor fornece gerenciamento centralizado de servidores locais e remotos, sem a necessidade de acesso de console.

Active Directory Domain Services é uma dessas funções de Hub; ao executar Gerenciador do Servidor em um controlador de domínio ou no Ferramentas de Administração de Servidor Remoto em um Windows 8, você verá problemas recentes importantes em controladores de domínio em sua floresta.

Essas exibições incluem:

- Disponibilidade do servidor
- Alertas do monitor de desempenho de alto uso da CPU e da memória
- O status dos serviços Windows específicos para AD DS
- Entradas de aviso e erro relacionadas aos Serviços de diretório recentes no log de eventos
- Análise da melhor prática de um controlador de domínio com relação a um conjunto de regras recomendadas pela Microsoft

## <a name="active-directory-administrative-center-recycle-bin"></a>Lixeira do Centro Administrativo do Active Directory

![administração simplificada](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_ADAC.png)

O Windows Server 2008 R2 introduziu a Lixeira do Active Directory, que recupera objetos excluídos do Active Directory sem restaurar do backup, reiniciando o serviço AD DS ou reinicializando os controladores de domínio.

O Windows Server 2012 aprimora as funcionalidades de restauração existentes baseadas em Windows PowerShell com uma nova interface gráfica no Centro Administrativo do Active Directory. Isso permite aos administradores habilitar a Lixeira e localizar ou restaurar objetos excluídos nos contextos de domínio da floresta, tudo sem executar diretamente os cmdlets do Windows PowerShell. O Centro Administrativo do Active Directory e a Lixeira do Active Directory ainda usam o Windows PowerShell nos bastidores, assim, os scripts anteriores e os procedimentos ainda são valiosos.

Para mais informações sobre a [Lixeira do Active Directory, consulte Guia passo a passo da lixeira do Active Directory (Windows Server 2008 R2)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd392261(v=ws.10)).

## <a name="active-directory-administrative-center-fine-grained-password-policy"></a>Política de senha refinada do Centro Administrativo do Active Directory

![administração simplificada](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_FGPP.png)

O Windows Server 2008 introduziu a política de senha refinada que permite aos administradores configurarem várias políticas de senha e bloqueio de conta por domínio. Isso permite aos domínios uma solução flexível para reforçar mais ou menos regras de senha restritiva, com base nos usuários e grupos. Ela não tinha interface de gerenciamento e era necessário que os administradores a configurassem usando Ldp.exe ou Adsiedit.msc. O Windows Server 2008 R2 introduziu o módulo do Active Directory ao Windows PowerShell, o que concedeu aos administradores uma interface de linha de comando para FGPP.

O Windows Server 2012 traz uma interface gráfica para Política de senha refinada. O Centro Administrativo do Active Directory é a página inicial desse novo diálogo, que traz gerenciamento FGPP simplificado a todos os administradores.

Para obter informações sobre Políticas de Senha Refinada, consulte o [Guia passo a passo da política de bloqueio de senhas e contas refinadas do AD DS (Windows Server 2008 R2)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770842(v=ws.10)).

## <a name="active-directory-administrative-center-windows-powershell-history-viewer"></a>O visualizador de histórico do Windows PowerShell no Centro Administrativo do Active Directory

![administração simplificada](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_HistoryViewer.png)

O Windows Server 2008 R2 introduziu o Centro Administrativo do Active Directory, que substitui o snap-in anterior de Usuários e computadores do Active Directory criado no Windows 2000. O Centro Administrativo do Active Directory cria uma interface administrativa gráfica para o então recente módulo do Active Directory para Windows PowerShell.

Enquanto o módulo do Active Directory contém mais de uma centena de cmdlets, a curva de aprendizagem de um administrador pode ser acentuada. Como o Windows PowerShell se integra consideravelmente à estratégia de administração do Windows, o Centro Administrativo do Active Directory agora inclui um visualizador que permite que você veja a execução do cmdlet na interface gráfica. Você pode pesquisar, copiar, limpar o histórico e adicionar notas com uma interface simples. O objetivo é que um administrador use a interface gráfica para criar e modificar objetos, depois reveja-os no visualizador de histórico para aprender mais sobre os scripts do Windows PowerShell e modificar os exemplos.

## <a name="ad-replication-windows-powershell"></a>Windows PowerShell de replicação AD

![administração simplificada](media/AD-DS-Simplified-Administration/ADDS_PSNewADReplSite.png)

O Windows Server 2012 adiciona cmdlets de replicação do Active Directory adicionais ao módulo do Active Directory do Windows PowerShell. Isso permite a configuração de sites, sub-redes, conexões, links de site e pontes novos e existentes. Também retornam metadados de replicação do Active Directory, status de replicação, enfileiramento e informações de vector de atualização de versão. A introdução dos cmdlets de replicação - combinada com a implantação e outros cmdlets existentes do AD DS - torna possível administrar uma floresta usando somente o Windows PowerShell. Isso cria novas oportunidades para administradores que desejam provisionar e gerenciar um Windows Server 2012 sem uma interface gráfica, o que reduz, então, a superfície de ataque do sistema operacional e os requisitos de serviço. Isso é especialmente importante ao implantar servidores em rede de alta segurança, como SIPR (Secret Internet Protocol Router) e DMZs corporativos.

Para obter mais informações sobre a topologia e replicação do site AD DS, consulte a [Referência técnica do Windows Server](/previous-versions/windows/it-pro/windows-server-2003/cc739127(v=ws.10)).

## <a name="rid-management-and-issuance-improvements"></a>Gerenciamento RID e melhorias de emissão

O Windows 2000 Active Directory introduziu o Mestre RID, que emite pools de identificadores relativos para os controladores de domínio, a fim de criar identificadores de segurança (SIDs) de entidades confiáveis, como usuários, grupos e computadores.  Por padrão, esse espaço RID global é limitado a um total de 2<sup>30</sup> (ou 1.073.741.823) SIDs criados em um domínio. Os SIDs não podem retornar para o pool ou serem reemitidos. Com o tempo, um domínio grande pode começar a ficar escasso nos RIDs, ou os acidentes podem levar ao esgotamento de RID desnecessário e a uma eventual exaustão.

O Windows Server 2012 atende a um grande número de problemas de emissão RID e gerenciamento não cobertos por clientes e pelo suporte ao cliente da Microsoft, na medida em que o AD DS amadurece, e isso desde a criação dos primeiros domínios do Active Directory, em 1999. Eles incluem:

- Avisos de consumo RID periódicos são gravados no log de eventos
- Os eventos são registrados quando um administrador invalida um pool RID
- Um teto máximo na política de RID, no tamanho de bloco RID, agora é reforçado
- Os tetos RID artificiais agora são reforçados e registrados quando o espaço RID global está baixo, permitindo que um administrador decida por uma ação antes que o espaço global seja exaurido
- O espaço RID global agora pode ser aumentado em um bit, dobrando o tamanho para 2<sup>31</sup> (2.147.483.648 SIDs)

Para obter mais informações sobre os RIDs e o Mestre RID, reveja [Como os identificadores de segurança funcionam](/previous-versions/windows/it-pro/windows-server-2003/cc778824(v=ws.10)).

## <a name="ad-ds-role-deployment-and-management-architecture"></a>Implantação de função AD DS e arquitetura de gerenciamento

O Gerenciador do Servidor e o ADDSDeployment do Windows PowerShell contam com os seguintes principais conjuntos para funcionalidade ao implantar ou gerenciar a função AD DS:

- Microsoft.ADroles.Aspects.dll
- Microsoft.ADroles.Instrumentation.dll
- Microsoft.ADRoles.ServerManager.Common.dll
- Microsoft.ADRoles.UI.Common.dll
- Microsoft.DirectoryServices.Deployment.Types.dll
- Microsoft.DirectoryServices.ServerManager.dll
- Addsdeployment.psm1
- Addsdeployment.psd1

Ambos contam com o Windows PowerShell e seu invoke-command remoto para instalação e configuração de função remotas.

![administração simplificada](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_DepDLLs.png)

O Windows Server 2012 também refatora algumas operações de promoção anteriores do LSASS.EXE, como parte do:

- Serviço do servidor de função DS (DsRoleSvc)
- DSRoleSvc.dll (carregado pelo serviço DsRoleSvc)

Esse serviço deve estar presente e em execução para promover, rebaixar ou clonar controladores de domínio virtuais. A instalação da função AD DS adiciona esse serviço e configura um tipo inicial de manual, por padrão. Não desabilite este serviço.

## <a name="adprep-and-prerequisite-checking-architecture"></a>ADPrep e arquitetura de verificação de pré-requisitos

O Adprep não requer mais a execução no mestre de esquema. Ele pode ser executado remotamente em um computador que executa o Windows Server 2008 x64 ou mais recente.

> [!NOTE]
> O Adprep usa LDAP para importar arquivos Schxx.ldf e não se reconecta automaticamente se a conexão ao mestre de esquema for perdida durante a importação. Como parte do processo de importação, o mestre de esquema é definido em um modo específico e a reconexão automática fica desabilitada, porque se o LDAP se reconectasse após a conexão ser perdida, a conexão restabelecida não seria no modo especifico. Nesse caso, o esquema não seria atualizado corretamente.

A verificação de pré-requisitos assegura que certas condições sejam verdadeiras. Essas condições são necessárias para uma instalação bem-sucedida do AD DS. Se algumas condições necessárias não forem verdadeiras, elas poderão ser resolvidas antes que a instalação continue. Ela também detecta se uma floresta ou um domínio ainda não está preparado, de forma que o código de implantação Adprep seja executado automaticamente.

### <a name="adprep-executables-dlls-ldfs-files"></a>Executáveis ADPrep, DLLs, LDFs, arquivos

- ADprep.dll
- Ldifde.dll
- Csvde.dll
- Sch14.ldf - Sch56.ldf
- Schupgrade.cat
- *dcpromo.csv

O código de preparação AD, antes situado no ADprep.exe, é refatorado no adprep.dll. Isso permite que ambos, o ADPrep.exe e o módulo ADDSDeployment do Windows PowerShell, usem a biblioteca para as mesmas tarefas e tenham as mesmas funcionalidades. O Adprep.exe é incluído com a mídia de instalação, mas os processos automatizados não o chamam diretamente - somente um Administrador o executa manualmente. Ele só pode ser executado no Windows Server 2008 x64 e sistemas operacionais mais recentes. Ldifde.exe e csvde.exe também possuem versões refatoradas como DLLs que são carregadas pelo processo de preparação. A expansão do esquema ainda usa os arquivos LDF verificados por assinatura, como nas versões de sistemas operacionais anteriores.

![administração simplificada](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_AdprepDLLs.png)

> [!IMPORTANT]
> Não há ferramenta Adprep32.exe de 32 bits para o Windows Server 2012. Você deve ter pelo menos um computador com Windows Server 2008 x64, Windows Server 2008 R2 ou Windows Server 2012 executando como controlador de domínio, servidor membro ou em um grupo de trabalho para preparar a floresta e o domínio. O Adprep.exe não executa em Windows Server 2003 x64.

## <a name="prerequisite-checking"></a><a name="BKMK_PrereuisiteChecking"></a>Verificação de pré-requisito

O sistema de verificação de pré-requisitos compilado no código gerenciado do ADDSDeployment do Windows PowerShell funciona em diferentes modos, de acordo com a operação. As tabelas abaixo descrevem cada teste, quando ele é usado e contêm uma explicação de como e o que ele valida. Essas tabelas podem ser úteis se houver problemas em que a validação falha e o erro não é suficiente para a solução de problemas.

Esses testes registram no log de evento operacional do **DirectoryServices-Deployment** o canal sob a Categoria da tarefa **Core**, sempre como o ID do evento **103**.

### <a name="prerequisite-windows-powershell"></a>Pré-requisito do Windows PowerShell

Há cmdlets do ADDSDeployment do Windows PowerShell para todos os cmdlets de implantação de controlador de domínio. Eles possuem aproximadamente os mesmos argumentos que seus cmdlets associados.

- Test-ADDSDomainControllerInstallation
- Test-ADDSDomainControllerUninstallation
- Test-ADDSDomainInstallation
- Test-ADDSForestInstallation
- Test-ADDSReadOnlyDomainControllerAccountCreation

Normalmente, não há necessidade de executar esses cmdlets; eles já são executados automaticamente com os cmdlets de implantação por padrão.

#### <a name="prerequisite-tests"></a><a name="BKMK_ADDSInstallPrerequisiteTests"></a>Testes de pré-requisitos

| Nome do teste | Protocolos<p>usados | Explicação e notas |
|--|--|--|
| VerifyAdminTrusted<p>ForDelegationProvider | LDAP | Valida se você tem o privilégio de "Habilitar contas de computador e usuário para serem confiáveis para delegação" (SeEnableDelegationPrivilege) no controlador de domínio parceiro existente. Isso requer acesso ao seu atributo tokenGroups construído.<p>Não usado quando em contato com controladores de domínio do Windows Server 2003. Você deve confirmar manualmente esse privilégio antes da promoção |
| VerifyADPrep<p>Pré-requisitos (floresta) | LDAP | Descobre e contata o Mestre de Esquema usando o atributo rootDSE namingContexts e o atributo fsmoRoleOwner de contexto de nomenclatura do Esquema. Determina quais operações preparatórias (forestprep, domainprep ou rodcprep) são necessárias para a instalação do AD DS. Verifica se objectVersion do esquema é esperado e se ele requer extensão adicional. |
| VerifyADPrep<p>Pré-requisitos (domínio e RODC) | LDAP | Descobre e contata o Mestre da Infraestrutura usando o atributo rootDSE namingContexts e o atributo fsmoRoleOwner do contêiner de Infraestrutura. No caso de uma instalação RODC, esse teste descobre o mestre de nomenclatura do domínio e assegura que ele esteja online. |
| CheckGroup<p>Associação | LDAP,<p>RPC via SMB (LSARPC) | Validar se o usuário é um membro do grupo Admins. do Domínio ou Admistradores de Empresa, dependendo da operação (DA para adicionar ou rebaixar um controlador de domínio, EA para adicionar ou remover um domínio) |
| CheckForestPrep<p>GroupMembership | LDAP,<p>RPC via SMB (LSARPC) | Validar se o usuário é um membro dos grupos Administradores de Esquema e Administradores de Empresa e se possui privilégio para Gerenciar auditoria e Logs de eventos de segurança (SesScurityPrivilege) nos controladores de domínio existentes |
| CheckDomainPrep<p>GroupMembership | LDAP,<p>RPC via SMB (LSARPC) | Validar se o usuário é um membro do grupo Admins. do Domínio e se possui privilégio para Gerenciar auditoria e Logs de eventos de segurança (SesScurityPrivilege) nos controladores de domínio existentes |
| CheckRODCPrep<p>GroupMembership | LDAP,<p>RPC via SMB (LSARPC) | Validar se o usuário é um membro do grupo Administradores de Empresa e se possui privilégio para Gerenciar auditoria e Logs de eventos de segurança (SesScurityPrivilege) nos controladores de domínio existentes |
| VerifyInitSync<p>AfterReboot | LDAP | Validar se o Mestre do esquema replicou pelo menos uma vez antes de reiniciar definindo um valor fictício no atributo becomeSchemaMaster do rootDSE |
| VerifySFUHotFix<p>Aplicado | LDAP | Validar se o esquema de floresta existente não contém problema conhecido com a extensão SFU2 para o atributo UID com OID 1.2.840.113556.1.4.7000.187.102<p>([https://support.microsoft.com/kb/821732](https://support.microsoft.com/kb/821732)) |
| VerifyExchange<p>SchemaFixed | LDAP, WMI, DCOM, RPC | Validar que o esquema de floresta existente ainda não contém as extensões do Exchange 2000 de problemas ms-Exch-Assistant-Name, ms-Exch-LabeledURI e ms-Exch-House-Identifier ( [https://support.microsoft.com/kb/314649](https://support.microsoft.com/kb/314649) ) |
| VerifyWin2KSchema<p>Consistência | LDAP | Validar se o esquema de floresta existente tem atributos núcleos e classes consistentes (não modificados incorretamente por terceiros). |
| DCPromo | DRSR via RPC,<p>LDAP,<p>DNS<p>RPC via SMB (SAMR) | Validar a sintaxe de linha de comando passada para o código de promoção e testar a promoção. Validar se a floresta ou o domínio já existe ou não para criar um novo. |
| VerifyOutbound<p>ReplicationEnabled | LDAP, DRSR via SMB, RPC via SMB (LSARPC) | Validar que o controlador de domínio existente especificado como o parceiro de replicação tem a replicação de saída habilitada, verificando o atributo das opções do objeto de Configurações do NTDS para NTDSDSA_OPT_DISABLE_OUTBOUND_REPL (0x00000004) |
| VerifyMachineAdmin<p>Senha | DRSR via RPC,<p>LDAP,<p>DNS<p>RPC via SMB (SAMR) | Validar se a senha de modo seguro definida para DSRM atende aos requisitos de complexidade do domínio. |
| VerifySafeModePassword | *N/A* | Validar se a senha do Administrador local definida atende aos requisitos de complexidade da política de segurança do computador. |
