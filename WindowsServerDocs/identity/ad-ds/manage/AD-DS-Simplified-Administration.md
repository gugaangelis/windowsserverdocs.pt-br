---
ms.assetid: f74eec9a-2485-4ee0-a0d8-cce01250a294
title: "AD DS simplificado administração"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 6232e281c47f3b5b4627bc9d8ccf53269aafc390
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="ad-ds-simplified-administration"></a>AD DS simplificado administração

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico explica os novos recursos e benefícios de implantação de controlador de domínio do Windows Server 2012 e administração e as diferenças entre a implantação de sistema operacional DC anterior e a nova implementação do Windows Server 2012.  
  
Windows Server 2012 apresenta a próxima geração de administração simplificada domínio serviços do Active Directory e é o mais radical domínio novamente concepção desde o Windows 2000 Server. Administração simplificada do AD DS leva lições aprendidas com 12 anos do Active Directory e proporciona uma experiência de administrativa mais intuitiva mais com suporte, mais flexível, arquitetos e administradores. Isso significa criar novas versões do existente tecnologias, bem como estender as capacidades dos componentes lançados no Windows Server 2008 R2.  
  
Administração simplificada do AD DS é uma releitura da implantação de domínio.  
  
-   Implantação de função do AD DS agora faz parte da nova arquitetura Gerenciador do servidor e permite a instalação remota  
  
-   O mecanismo de implantação e configuração do AD DS agora é Windows PowerShell, mesmo quando usando o novo Assistente de configuração do AD DS  
  
-   Extensão de esquema, preparação de floresta e preparação de domínio são automaticamente parte da promoção de controlador de domínio e não requerem tarefas separadas em servidores especiais, como o mestre de esquema  
  
-   Promoção agora inclui a verificação de pré-requisitos que valida a preparação de floresta e domínio novo controlador de domínio, reduzindo a chance de promoções com falha  
  
-   Módulo do Active Directory para Windows PowerShell agora inclui cmdlets para gerenciamento de topologia de replicação, controle de acesso dinâmico e outras operações  
  
-   Precisa de floresta do Windows Server 2012 funcional nível não implementar novos recursos e o nível funcional do domínio é necessário somente para um subconjunto dos novos recursos de Kerberos, liberando os administradores das frequentes para um ambiente de controlador de domínio homogêneo  
  
-   Suporte integral adicionado virtualizado para controladores de domínio, para incluir a proteção de reversão e implantação automatizada  
  
Para obter mais informações sobre controladores de domínio virtualizado, consulte [Introdução aos serviços de domínio do Active Directory & #40; AD DS e 41; Virtualização & #40; Nível de 100 & #41;](../../ad-ds/Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md).  
  
Além disso, há muitos administrativos e melhorias de manutenção:  
  
-   O Centro Administrativo do Active Directory inclui um gráfico Active Directory Lixeira, gerenciamento de política de senha refinadas e Visualizador de histórico do Windows PowerShell  
  
-   O novo Gerenciador do servidor tem interfaces específicas do AD DS para monitoramento de desempenho, melhor análise de prática, serviços críticos e os logs de eventos  
  
-   Contas de serviço gerenciado do grupo dão suporte a vários computadores usando as mesma entidades de segurança  
  
-   Melhorias na emissão RID (identificador relativo) e o monitoramento de melhor capacidade de gerenciamento em domínios do Active Directory maduros  
  
Por fim, o AD DS os lucros de outros novos recursos incluídos no Windows Server 2012, como:  
  
-   Agrupamento de NIC e Datacenter pontes  
  
-   Segurança de DNS e a disponibilidade de zona integrada ao AD mais rápida após a inicialização  
  
-   Aperfeiçoamentos de confiabilidade e escalabilidade do Hyper-V  
  
-   Desbloqueio pela rede do BitLocker  
  
-   Módulos adicionais de administração de componente do Windows PowerShell  
  
## <a name="technical-overview"></a>Visão geral técnica  
  
### <a name="adprep-integration"></a>Integração de ADPREP  
Floresta esquema extensão e domínio preparação do Active Directory agora integrar o processo de configuração do controlador de domínio. Se você promover um novo controlador de domínio em uma floresta existente, o processo detecta o status da atualização e as fases de preparação de domínio e de extensão de esquema ocorrem automaticamente. O usuário instalando o primeiro controlador de domínio do Windows Server 2012 ainda deve ser um administrador da empresa e o administrador de esquema ou fornecer credenciais alternativas válidas.  
  
Adprep.exe permanece no DVD de floresta separada e preparação do domínio. A versão da ferramenta incluída com o Windows Server 2012 é compatível com versões anteriores ao Windows Server 2008 x64 e Windows Server 2008 R2. Adprep.exe também dá suporte remoto forestprep e domainprep, assim como as ferramentas de configuração do controlador de domínio baseados em ADDSDeployment.  
  
Para obter informações sobre Adprep e preparação de floresta de sistema operacional anterior, consulte [Adprep em execução (Windows Server 2008 R2)](https://technet.microsoft.com/library/dd464018(WS.10).aspx).  
  
### <a name="server-manager-ad-ds-integration"></a>Integração com o Gerenciador do servidor AD DS  
![Administração simplificada](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_Dashboard.png)  
  
Gerenciador do servidor age como um hub para tarefas de gerenciamento de servidor. Sua aparência do painel Estilos periodicamente atualiza os modos de exibição de funções instaladas e grupos de servidores remotos. Gerenciador do servidor fornece gerenciamento centralizado de servidores locais e remotos, sem a necessidade de acesso ao console.  
  
Serviços de domínio do Active Directory é uma dessas funções de hub; executando o Gerenciador do servidor em um controlador de domínio ou as ferramentas de administração de servidor remoto em um Windows 8, consulte problemas recentes importantes em controladores de domínio na floresta.  
  
Esses modos de exibição incluem:  
  
-   Disponibilidade do servidor  
  
-   Alertas do monitor de desempenho para alto uso de CPU e memória  
  
-   O status dos serviços do Windows específicos no AD DS  
  
-   Recentes relacionado a serviços de diretório do erro e aviso de entradas no log de eventos  
  
-   Análise de prática recomendada de um controlador de domínio contra um conjunto de regras a Microsoft recomenda  
  
### <a name="active-directory-administrative-center-recycle-bin"></a>Centro Administrativo do Active Directory a Lixeira  
![Administração simplificada](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_ADAC.png)  
  
Windows Server 2008 R2 introduziu o Active Directory Lixeira, que recupera objetos do Active Directory excluídos sem restaurar a partir do backup, reiniciar o serviço do AD DS ou reiniciar controladores de domínio.  
  
Windows Server 2012 aprimora os recursos de restauração do Windows PowerShell existente com uma nova interface gráfica no Centro Administrativo do Active Directory. Isso permite que os administradores habilitem a Lixeira e localize ou restaurar objetos excluídos os contextos de domínio da floresta, tudo sem diretamente executar cmdlets do Windows PowerShell. O Centro Administrativo do Active Directory e a Lixeira do Active Directory ainda usam o Windows PowerShell nos bastidores, portanto, procedimentos e scripts anteriores são ainda valiosos.  
  
Para obter informações sobre o Active Directory [a Lixeira, consulte a Lixeira do Active Directory Step-by-Step guia (Windows Server 2008 R2)](https://technet.microsoft.com/library/dd392261(WS.10).aspx).  
  
### <a name="active-directory-administrative-center-fine-grained-password-policy"></a>Política de senha refinada Centro Administrativo do Active Directory  
![Administração simplificada](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_FGPP.png)  
  
Windows Server 2008 introduziu a política de senha refinadas, que permite que os administradores definam várias políticas de bloqueio de conta e senha por domínio. Isso permite que domínios uma solução flexível para impor regras de senha mais ou menos restritivas, com base em usuários e grupos. Ele tenha nenhuma interface administrativa e os administradores de TI necessários configurá-lo usando Ldp.exe ou Adsiedit.msc. Windows Server 2008 R2 introduziu o módulo do Active Directory para Windows PowerShell, que é concedido a administradores de uma interface de linha de comando para FGPP.  
  
Windows Server 2012 traz uma interface gráfica para política de senha refinadas. O Centro Administrativo do Active Directory é a página inicial dessa nova caixa de diálogo, que traz o gerenciamento simplificado de FGPP para todos os administradores.  
  
Para obter informações sobre a política de senha refinadas, consulte [AD DS refinadas senha e política de bloqueio de conta Step-by-Step guia (Windows Server 2008 R2)](https://technet.microsoft.com/library/cc770842(WS.10).aspx).  
  
### <a name="active-directory-administrative-center-windows-powershell-history-viewer"></a>Visualizador de histórico do PowerShell do Windows de centro administrativo do Active Directory  
![Administração simplificada](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_HistoryViewer.png)  
  
Windows Server 2008 R2 introduziu o Centro Administrativo do Active Directory, que substituídos o Active Directory Users mais antigos computadores snap-in e criado no Windows 2000. O Centro Administrativo do Active Directory cria uma interface gráfica administrativa para o módulo do Active Directory, em seguida, novo para o Windows PowerShell.  
  
Embora o módulo do Active Directory contém sobre cmdlets de cem, a curva de aprendizado para um administrador pode ser íngremes. Desde que o Windows PowerShell intensamente integra a estratégia de administração do Windows, o Centro Administrativo do Active Directory agora inclui um visualizador que permite que você veja a execução do cmdlet na interface gráfica. Você pode pesquisar, copiar, limpar o histórico e adicionar anotações com uma interface simples. O objetivo é para um administrador usar a interface gráfica para criar e modificar objetos e, em seguida, revisá-los no Visualizador de histórico para saber mais sobre o Windows PowerShell script e modificar os exemplos.  
  
### <a name="ad-replication-windows-powershell"></a>Replicação do AD Windows PowerShell  
![Administração simplificada](media/AD-DS-Simplified-Administration/ADDS_PSNewADReplSite.png)  
  
Windows Server 2012 adiciona cmdlets de replicação do Active Directory adicionais para o módulo do Active Directory do Windows PowerShell. Eles permitem uma configuração de sites de novos ou existentes, sub-redes, conexões, links de site e pontes. Eles também retornam do Active Directory metadados de replicação, status de replicação, enfileiramento de mensagens e informações de vetor UTDV versão. A introdução dos cmdlets de replicação - combinadas com a implantação e outros cmdlets do AD DS existente - torna possível administrar uma floresta usando o Windows PowerShell sozinho. Isso cria novas oportunidades para os administradores que desejam provisionar e gerenciar o Windows Server 2012 sem uma interface gráfica, o que reduz a superfície de ataque do sistema operacional e requisitos de manutenção. Isso é especialmente importante quando implantar servidores em redes de alta segurança como segredo Internet protocolo roteador (SIPR) e DMZs corporativas.  
  
Para obter mais informações sobre a topologia de site do AD DS e replicação, consulte o [referência técnica do Windows Server](https://technet.microsoft.com/library/cc739127(WS.10).aspx).  
  
### <a name="rid-management-and-issuance-improvements"></a>Melhorias de emissão e gerenciamento RID  
Active Directory do Windows 2000 introduziu o mestre RID, quais pools de problemas de identificadores relativos para controladores de domínio, para criar identificadores de segurança (SIDs) de objetos de confiança da segurança, como os usuários, grupos e computadores.  Por padrão, esse espaço RID global é limitado a 2<sup>30</sup> (ou 1.073.741.823) total SIDs criados em um domínio. SIDs não pode retornar ao pool de ou envie. Ao longo do tempo, um grande domínio pode começar a esgotamento da RIDs ou acidentes podem levar a esgotamento de RID desnecessário e esgotamento eventual.  
  
Windows Server 2012 endereços de uma série de problemas de emissão e gerenciamento de RID revelados por clientes e atendimento ao cliente Microsoft como o AD DS se converteu desde a criação de domínios do Active Directory primeiro em 1999. Esses cenários incluem:  
  
-   Avisos de consumo RID periódicos são gravados no log de eventos  
  
-   Eventos de logon quando um administrador invalida um pool RID  
  
-   Um limite máximo na política RID que LIVRAR tamanho do bloco é imposto agora  
  
-   Artificiais tetos RID agora são impostos e registrados quando o espaço RID global é baixo, permitindo que um administrador executar uma ação antes do espaço global é esgotado  
  
-   O espaço RID global agora pode ser aumentado em um bit, dobrando o tamanho de 2<sup>31</sup> (2.147.483.648 SIDs)  
  
Para obter mais informações sobre RIDs e o mestre RID, examine [como funcionam os identificadores de segurança](https://technet.microsoft.com/library/cc778824(WS.10).aspx).  
  
## <a name="new-ad-ds-deployment-architecture"></a>Arquitetura de implantação do novo AD DS  
  
### <a name="ad-ds-role-deployment-and-management-architecture"></a>Arquitetura de gerenciamento e implantação do AD DS função  
Gerenciador do servidor e ADDSDeployment Windows PowerShell contam com os seguintes assemblies de core para a funcionalidade ao implantar ou gerenciar a função AD DS:  
  
-   Microsoft.ADroles.Aspects.dll  
  
-   Microsoft.ADroles.Instrumentation.dll  
  
-   Microsoft.ADRoles.ServerManager.Common.dll  
  
-   Microsoft.ADRoles.UI. Common.dll  
  
-   Microsoft.DirectoryServices.Deployment.Types.dll  
  
-   Microsoft.DirectoryServices.ServerManager.dll  
  
-   Addsdeployment.psm1  
  
-   Addsdeployment.psd1  
  
Ambos dependem do Windows PowerShell e seus remoto invocar comandos para a função remota instalação e configuração.  
  
![Administração simplificada](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_DepDLLs.png)  
  
Windows Server 2012 também refatorar um número de operações de promoção anteriores fora LSASS.EXE, como parte do:  
  
-   Serviço de servidor de função DS (DsRoleSvc)  
  
-   DSRoleSvc.dll (carregados pelo serviço de DsRoleSvc)  
  
Esse serviço deve estar presente e em execução para promover, rebaixar ou clone controladores de domínio virtual. Instalação de função do AD DS adiciona esse serviço e define um tipo de tela inicial do Manual, por padrão. Não desative esse serviço.  
  
### <a name="adprep-and-prerequisite-checking-architecture"></a>ADPrep e arquitetura de verificação de pré-requisito  
Adprep não requer mais em execução no mestre de esquema. Ele pode ser executado remotamente a partir de um computador que executa o Windows Server 2008 x64 ou posterior.  
  
> [!NOTE]  
> Adprep usa LDAP para importar arquivos de Schxx.ldf e não reconecte automaticamente se a conexão ao mestre de esquema é perdida durante a importação. Como parte do processo de importação, o mestre de esquema é definido em um modo específico e reconexão automática está desabilitada porque se LDAP se reconecta depois que a conexão é perdida, a conexão restabelecida não seria no modo específico. Nesse caso, o esquema não será atualizado corretamente.  
  
Verificação de pré-requisito garante que determinadas condições forem verdadeiras. Estas condições são necessárias para a instalação bem-sucedida do AD DS. Se algumas condições necessárias não forem verdadeiras, eles podem ser resolvidos antes de continuar a instalação. Ele também detecta se um domínio ou floresta não são ainda preparado, para que o código de implantação Adprep é executado automaticamente.  
  
#### <a name="adprep-executables-dlls-ldfs-files"></a>ADPrep Executables, DLLs, LDFs, arquivos  
  
-   ADprep.dll  
  
-   Ldifde.dll  
  
-   Csvde.dll  
  
-   Sch14.ldf - Sch56.ldf  
  
-   Schupgrade.cat  
  
-   *Dcpromo.csv  
  
O código de preparação do AD antigo alojado em ADprep.exe é refatorado em adprep.dll. Isso permite ADPrep.exe e o módulo ADDSDeployment Windows PowerShell usar a biblioteca para as mesmas tarefas e ter os mesmos recursos. Adprep.exe está incluído com a mídia de instalação, mas processos automatizados não chamar diretamente - apenas um administrador executa-lo manualmente. Ele só pode ser executado em Windows Server 2008 x64 e sistemas operacionais posteriores. Ldifde.exe e csvde.exe também Reformulamos versões como DLLs que são carregados pelo processo de preparação. Extensão de esquema ainda usa os arquivos de assinatura verificada LDF, como nas versões anteriores do sistema operacional.  
  
![Administração simplificada](media/AD-DS-Simplified-Administration/ADDS_SMI_TR_AdprepDLLs.png)  
  
> [!IMPORTANT]  
> Existe uma ferramenta de Adprep32.exe de 32 bits do Windows Server 2012. Você deve ter pelo menos um computador Windows Server 2012, em execução como um controlador de domínio, servidor membro, ou em um grupo de trabalho, para preparar a floresta e domínio, Windows Server 2008 R2 ou Windows Server 2008 x64. Adprep.exe não é executado no Windows Server 2003 x64.  
  
#### <a name="BKMK_PrereuisiteChecking"></a>Verificação de pré-requisitos  
O sistema integrado no código do Windows PowerShell ADDSDeployment gerenciada de verificação de pré-requisito funciona em diferentes modos, com base na operação. As tabelas a seguir descrevem cada teste, quando ele é usado e uma explicação sobre como e o que ele valida. Estas tabelas podem ser útil quando há problemas em que a validação falhar e o erro não for suficiente para solucionar o problema.  
  
Esses testes login o **DirectoryServices-Deployment** canal de log de eventos operacional sob a categoria de tarefa **Core**, sempre como ID de evento **103**.  
  
##### <a name="prerequisite-windows-powershell"></a>Pré-requisito Windows PowerShell  
Há ADDSDeployment Windows PowerShell cmdlets para todos os cmdlets de implantação do controlador de domínio. Eles têm aproximadamente os mesmos argumentos seus cmdlets associados.  
  
-   Test-ADDSDomainControllerInstallation  
  
-   Test-ADDSDomainControllerUninstallation  
  
-   Test-ADDSDomainInstallation  
  
-   Test-ADDSForestInstallation  
  
-   Test-ADDSReadOnlyDomainControllerAccountCreation  
  
Não é necessário para executar esses cmdlets, normalmente; eles já automaticamente executam com os cmdlets de implantação por padrão.  
  
##### <a name="BKMK_ADDSInstallPrerequisiteTests"></a>Testes de pré-requisito  
  
||||  
|-|-|-|  
|Nome do teste|Protocolos<br /><br />usado|Explicação e anotações|  
|VerifyAdminTrusted<br /><br />ForDelegationProvider|LDAP|Valida que você tenha "Habilitar computador e contas de usuário para serem confiáveis para delegação" privilégio (SeEnableDelegationPrivilege) no controlador de domínio existentes do parceiro. Isso exige acesso ao seu atributo tokenGroups construído.<br /><br />Não é usado quando entrar em contato com controladores de domínio do Windows Server 2003. Você deve confirmar manualmente esse privilégio antes da promoção|  
|VerifyADPrep<br /><br />Pré-requisitos (floresta)|LDAP|Detecta e contata o mestre de esquema usando o atributo de namingContexts rootDSE e o atributo de fsmoRoleOwner do contexto de nomenclatura de esquema. Determina quais operações preparatórias (forestprep, domainprep ou rodcprep) são necessárias para a instalação do AD DS. Valida o esquema objectVersion é esperado e, se ele ainda mais preciso extensão.|  
|VerifyADPrep<br /><br />Pré-requisitos (domínio e RODC)|LDAP|Detecta e contata o mestre de infraestrutura usando o atributo de namingContexts rootDSE e o atributo de fsmoRoleOwner do contêiner de infraestrutura. No caso de acontecer, esse teste detecta o mestre de nomeação de domínio e verifique se ele estiver online.|  
|CheckGroup<br /><br />Associação ao grupo|LDAP,<br /><br />RPC por SMB (LSARPC)|Validar o usuário é um membro do grupo Admins. do domínio ou administradores corporativos, dependendo da operação (para adicionar ou rebaixar um controlador de domínio, EA para adição ou remoção de um domínio)|  
|CheckForestPrep<br /><br />GroupMembership|LDAP,<br /><br />RPC por SMB (LSARPC)|Validar o usuário é um membro do grupo Administradores de esquema e grupos de administradores corporativos e tem a gerenciar auditoria e Logs de eventos de segurança (SesScurityPrivilege) privilégio nos controladores de domínio existentes|  
|CheckDomainPrep<br /><br />GroupMembership|LDAP,<br /><br />RPC por SMB (LSARPC)|Validar o usuário é um membro do grupo Admins. do domínio e tem a gerenciar auditoria e Logs de eventos de segurança (SesScurityPrivilege) privilégio nos controladores de domínio existentes|  
|CheckRODCPrep<br /><br />GroupMembership|LDAP,<br /><br />RPC por SMB (LSARPC)|Validar o usuário é um membro do grupo Administradores corporativos e tem a gerenciar auditoria e Logs de eventos de segurança (SesScurityPrivilege) privilégio nos controladores de domínio existentes|  
|VerifyInitSync<br /><br />AfterReboot|LDAP|Validar que o mestre de esquema duplicou pelo menos uma vez desde que reiniciá-lo definindo um valor fictício em rootDSE atributo becomeSchemaMaster|  
|VerifySFUHotFix<br /><br />Aplicada|LDAP|Validar floresta existente esquema não contém problema conhecido SFU2 extensão para o atributo X:UID com OID 1.2.840.113556.1.4.7000.187.102<br /><br />([https://support.microsoft.com/kb/821732](https://support.microsoft.com/kb/821732))|  
|VerifyExchange<br /><br />SchemaFixed|LDAP, WMI, DCOM, RPC|Validar floresta existente esquema não contêm ainda problema Exchange 2000 extensões ms-Exchange-assistente-nome, ms-Exchange-LabeledURI e ms-Exchange-House-Identifier ([https://support.microsoft.com/kb/314649](https://support.microsoft.com/kb/314649))|  
|VerifyWin2KSchema<br /><br />Consistência|LDAP|Validar floresta existente esquema tem consistente (não incorretamente modificado por um terceiro) core atributos e classes.|  
|DCPromo|DRSR via RPC,<br /><br />LDAP,<br /><br />DNS<br /><br />RPC por SMB (SAMR)|Valide a sintaxe de linha de comando passada para a promoção de código e teste de promoção. Validar a floresta ou domínio ainda não existir se a criação de novos.|  
|VerifyOutbound<br /><br />ReplicationEnabled|LDAP, DRSR em SMB, RPC sobre SMB (LSARPC)|Validar o controlador de domínio existente especificado como o parceiro de replicação tenha habilitada, verificando o atributo de opções do objeto de configurações NTDS para NTDSDSA_OPT_DISABLE_OUTBOUND_REPL (0x00000004) a duplicação de saída|  
|VerifyMachineAdmin<br /><br />Senha|DRSR via RPC,<br /><br />LDAP,<br /><br />DNS<br /><br />RPC por SMB (SAMR)|Valide a senha de modo de segurança definida para DSRM atende aos requisitos de complexidade de domínio.|  
|VerifySafeModePassword|*N/D*|Valide os local administrador senha conjunto atende aos computador segurança política requisitos de complexidade.|  
  


