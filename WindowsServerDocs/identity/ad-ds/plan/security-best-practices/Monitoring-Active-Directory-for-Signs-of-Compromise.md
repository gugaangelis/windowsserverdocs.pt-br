---
ms.assetid: a7ef2fba-b05c-4be2-93b2-b9456244c3ad
title: O monitoramento do Active Directory de sinais de comprometimento
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 096f2fa58b9aae53a06bf26c2107eb4cee3aa5d9
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="monitoring-active-directory-for-signs-of-compromise"></a>O monitoramento do Active Directory de sinais de comprometimento

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Lei número cinco: vigilância Eternal é o preço de segurança.* - [10 leis imutáveis de administração de segurança](https://technet.microsoft.com/library/cc722488.aspx)  
  
Um log de eventos sólido monitoramento do sistema é uma parte essencial de qualquer design seguro do Active Directory. Muitos comprometimentos de segurança do computador podem ser descobertos no início do evento se as vítimas aprovados apropriado log de eventos de monitoramento e alertas. Relatórios independentes longa oferecem suporte a essa conclusão. Por exemplo, o [relatório de violação de dados 2009 Verizon](http://www.verizonbusiness.com/resources/security/reports/2009_databreach_rp.pdf) estados:  
  
"O ineffectiveness aparente da análise de monitoramento e o log de eventos de continua a ser um pouco de um enigma. A oportunidade para detecção existe; investigadores indicados 66 por cento de vítimas tinha evidências suficientes disponíveis em seus logs para descobrir a violação tinham que eles foram mais eficiente na analisar esses recursos."  
  
Essa falta de monitoramento de logs de eventos active permanece pontos fracos consistente em planos de defesa de segurança de muitas empresas. O [relatório de violação de dados Verizon 2012](http://www.verizonbusiness.com/resources/reports/rp_data-breach-investigations-report-2012_en_xg.pdf) encontrado que, embora 85% das violações levou várias semanas para ser observado, 84% dos vítimas tinha evidências da violação em seus logs de eventos.  
  
## <a name="windows-audit-policy"></a>Política de auditoria do Windows  
A seguir é links para o blog de suporte do Microsoft enterprise oficial. O conteúdo desses blogs fornece consultoria, diretrizes e recomendações sobre como auditar que ajudarão você aprimora a segurança de sua infraestrutura do Active Directory e são um recurso valioso durante a criação de uma política de auditoria.  
  
-   [Auditoria de acesso a objeto global é mágica](http://blogs.technet.com/b/askds/archive/2011/03/10/global-object-access-auditing-is-magic.aspx) -descreve um mecanismo de controle chamado avançada configuração política de auditoria que foi adicionado ao Windows 7 e Windows Server 2008 R2 que permite que você defina quais tipos de dados que você quisesse de auditoria facilmente e manipular não scripts e auditpol.exe.  
  
-   [Apresentando a auditoria de alterações no Windows 2008](http://blogs.technet.com/b/askds/archive/2007/10/19/introducing-auditing-changes-in-windows-2008.aspx) -apresenta a auditoria alterações feitas no Windows Server 2008.  
  
-   [Legais truques de auditoria no Vista e 2008](http://blogs.technet.com/b/askds/archive/2007/11/16/cool-auditing-tricks-in-vista-and-2008.aspx) -explica os recursos de auditoria interessantes do Windows Vista e Windows Server 2008, que pode ser usado para solução de problemas ou ver o que está acontecendo em seu ambiente.  
  
-   [Loja centralizada para a auditoria no Windows Server 2008 e Windows Vista](http://blogs.technet.com/b/askds/archive/2008/03/27/one-stop-shop-for-auditing-in-windows-server-2008-and-windows-vista.aspx) -contém uma compilação de auditoria de recursos e informações contidas no Windows Server 2008 e Windows Vista.  
  
Os links a seguir fornecem informações sobre os aprimoramentos no Windows auditoria no Windows 8 e Windows Server 2012 e informações sobre o AD DS auditoria no Windows Server 2008.  
  
-   [O que há de novo em auditoria de segurança](https://technet.microsoft.com/library/hh849638.aspx) -fornece uma visão geral dos novos recursos no Windows 8 e Windows Server 2012 de auditoria de segurança.  
  
-   [Auditoria do AD DS Step-by-Step guia](https://technet.microsoft.com/library/a9c25483-89e2-4202-881c-ea8e02b4b2a5.aspx) -descreve o novo recurso de auditoria de serviços de domínio do Active Directory (AD DS) no Windows Server 2008. Ele também fornece os procedimentos para implementar esse novo recurso.  
  
### <a name="windows-audit-categories"></a>Categorias de auditoria do Windows  
Antes do Windows Vista e Windows Server 2008, Windows tinha apenas nove categorias de política de auditoria de log de eventos:  
  
-   Eventos de Logon de conta  
  
-   Gerenciamento de contas  
  
-   Acesso ao serviço de diretório  
  
-   Eventos de logon  
  
-   Acesso a objetos  
  
-   Alteração da política  
  
-   Uso de privilégio  
  
-   Rastreamento de processo  
  
-   Eventos do sistema  
  
Esses nove categorias de auditoria tradicional compõem uma política de auditoria. Cada categoria de política de auditoria pode ser habilitada para êxito, falha ou êxito e falha eventos. Suas descrições estão incluídas na próxima seção.  
  
#### <a name="audit-policy-category-descriptions"></a>Descrições de categoria de política de auditoria  
As categorias de política de auditoria permitem os seguintes tipos de mensagem de log de eventos.  
  
##### <a name="audit-account-logon-events"></a>Auditoria de eventos de Logon de conta  
Relata cada instância de uma entidade de segurança (por exemplo, usuário, computador ou conta de serviço) que está fazendo logon ou logoff de um computador no qual a outro computador é usado para validar a conta. Eventos de logon de conta são gerados quando uma conta de entidade de segurança do domínio é autenticada em um controlador de domínio. Autenticação de um usuário local em um computador local gera um evento de logon que é registrado no log de segurança local. Nenhum evento de logoff da conta é registrado.  
  
Essa categoria gera muita "ruído" porque o Windows está constantemente tendo contas fazendo logon e logoff os computadores locais e remotos durante o curso normal de negócios. Ainda assim, qualquer plano de segurança deve incluir o êxito e falha dessa categoria de auditoria.  
  
##### <a name="audit-account-management"></a>Auditoria de gerenciamento de conta  
Esta configuração de auditoria determina a acompanhar o gerenciamento de usuários e grupos. Por exemplo, os usuários e grupos devem ser acompanhados quando um usuário ou conta de computador, um grupo de segurança ou um grupo de distribuição é criado, alterado ou excluído; Quando uma conta de usuário ou computador é renomeada, desabilitada ou habilitada; ou, quando um usuário ou computador for alterada. Um evento pode ser gerado para usuários ou grupos que são adicionados ou removidos de outros grupos.  
  
##### <a name="audit-directory-service-access"></a>Auditoria de acesso ao serviço de diretório  

Esta configuração de política determina se a auditoria de acesso de entidade de segurança para um objeto do Active Directory que tem sua própria lista de controle de acesso especificado do sistema (SACL). Em geral, essa categoria deve ser habilitada apenas em controladores de domínio. Quando habilitada, essa configuração gera muita "ruído".  
  
##### <a name="audit-logon-events"></a>Eventos de Logon de auditoria  
Eventos de logon são gerados quando uma entidade de segurança local é autenticada em um computador local. Eventos de logon registra logons de domínio que ocorrem no computador local. Eventos de logoff da conta não são gerados. Quando habilitada, os eventos de Logon gera muita "ruído", mas deve ser habilitados por padrão em qualquer plano de auditoria de segurança.  
  
##### <a name="audit-object-access"></a>Auditoria de acesso a objeto  
Acesso a objetos pode gerar eventos quando são acessados objetos subsequentemente definidos com a auditoria ativada (por exemplo, aberto, leitura, renomeados, excluídos ou fechado). Depois que a categoria de auditoria principal é habilitada, o administrador deve definir individualmente quais objetos será auditoria habilitado. Muitos objetos de sistema do Windows acompanham auditoria habilitado, portanto, habilitar essa categoria de geralmente começará a gerar eventos antes que o administrador definiu qualquer.  
  
Essa categoria é muito "exagerada" e irá gerar eventos de 5 a 10 para cada acesso a objetos. Pode ser difícil para os administradores do novos objeto de auditoria obter informações úteis. Ele deve ser habilitado apenas quando necessário.  
  
##### <a name="auditing-policy-change"></a>Alteração de políticas de auditoria  
Esta configuração de política determina se será auditada cada instância de uma alteração de políticas de atribuição de direitos de usuário, políticas de Firewall do Windows, políticas de confiança ou alterações para a política de auditoria. Essa categoria deve ser habilitada em todos os computadores. Ele gera muito pouco ruído.  
  
##### <a name="audit-privilege-use"></a>Auditoria de uso de privilégio  

Existem dezenas de direitos de usuário e permissões na Windows (por exemplo, faça Logon como um trabalho em lotes e atuar como parte do sistema operacional). Esta configuração de política determina se será auditada cada instância de uma entidade de segurança no exercício de um direito de usuário ou o privilégio. Habilitar resultados essa categoria em muitas "ruído", mas ela pode ser útil no rastreamento de contas de entidade de segurança usando privilégios elevados.  
  
##### <a name="audit-process-tracking"></a>Acompanhamento de processos de auditoria  
Esta configuração de política determina a auditoria processo detalhado acompanhando as informações para eventos como ativação de programa, saída do processo, duplicação de identificador e acesso indireto a objetos. É útil para rastrear usuários mal-intencionados e os programas que eles usam.  
  
Habilitar a auditoria Process Tracking gera um grande número de eventos, geralmente é definido como **sem auditoria**. No entanto, essa configuração pode fornecer uma ótima vantagem durante uma resposta a incidentes do log detalhado dos processos iniciados e o momento em que foram iniciados. Para controladores de domínio e outros servidores de infraestrutura de função única, nessa categoria pode ser ativada com segurança em todo o tempo. Servidores de função única não gera muito tráfego de rastreamento durante o curso normal de suas obrigações do processo. Dessa forma, pode ser habilitados para capturar eventos não autorizados se elas ocorrem.  
  
##### <a name="system-events-audit"></a>Auditoria de eventos do sistema  

Eventos do sistema é quase é uma categoria termo genérica, registrar vários eventos que afetam o computador, a segurança do sistema ou o log de segurança. Ele inclui eventos para desligamentos de computador e reinicializações, falhas de energia, alterações de tempo do sistema, inicializações de pacote de autenticação, clearings de log de auditoria, problemas de representação e uma série de outros eventos gerais. Em geral, habilitar essa categoria de auditoria gera muita "ruído", mas ele gera eventos suficientes muito útil que é difícil nunca recomendar não ativá-lo.  
  
#### <a name="advanced-audit-policies"></a>Políticas de auditoria avançada  
A partir do Windows Vista e Windows Server 2008, a Microsoft melhorou a maneira como as seleções de categoria do log de eventos podem ser feitas através da criação de subcategorias em cada categoria de auditoria principal. Permitir que subcategorias de auditoria ser muito mais granulares que era possível caso contrário, usando as principais categorias. Usando subcategorias, você pode habilitar somente as partes de uma determinada categoria principal e ignorar a geração de eventos para os quais você tem que não faz uso. Cada subcategoria de política de auditoria pode ser habilitada para êxito, falha ou êxito e falha eventos.  
  
Para listar todas as subcategorias de auditoria disponíveis, leia o contêiner de política de auditoria avançada em um objeto de política de grupo ou digite o seguinte no prompt de comando em qualquer computador que executa o Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008, Windows 8, Windows 7 ou Windows Vista:  
  
**auditpol//list /subcategory: \ ***  
  
Para obter uma lista de subcategorias de auditoria configuradas no momento em um computador executando o Windows Server 2012, Windows Server 2008 R2 ou Windows 2008, digite o seguinte:  
  
**auditpol//get /category: \ ***  
  
Captura de tela a seguir mostra um exemplo de auditpol.exe listagem a política de auditoria atual.  
  
![Anúncio de monitoramento](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_5.gif)  
  
> [!NOTE]  
> Política de grupo não relatar o status de todas as políticas de auditoria habilitados, sempre com precisão enquanto faz auditpol.exe. Consulte [Obtendo a política de auditoria efetiva no Windows 7 e 2008 R2](http://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx) para obter mais detalhes.  
  
Cada categoria principal tem diversas subcategorias. A seguir está uma lista de categorias, suas subcategorias e uma descrição de suas funções.  
  
### <a name="auditing-subcategories-descriptions"></a>Descrições de subcategorias de auditoria  
Subcategorias de política de auditoria permitem os seguintes tipos de mensagem de log de eventos:  
  
#### <a name="account-logon"></a>Logon de conta  
  
##### <a name="credential-validation"></a>Validação de credenciais  
Essa subcategoria relata os resultados dos testes de validação em credenciais enviadas para uma solicitação de logon de conta de usuário. Esses eventos ocorrem no computador autoritativo para as credenciais. Para contas de domínio, o controlador de domínio é autoritativo, enquanto para contas locais, o computador local é autoritativo.  
  
Em ambientes de domínio, a maioria dos eventos de logon de conta é registrada no log de segurança dos controladores de domínio autoritativos para as contas de domínio. No entanto, esses eventos podem ocorrer em outros computadores na organização quando contas locais são usadas para fazer logon.  
  
##### <a name="kerberos-service-ticket-operations"></a>Operações do tíquete de serviço Kerberos  
Essa subcategoria relata eventos gerados por processos de solicitação de tíquete Kerberos no controlador de domínio é autoritativo para a conta de domínio.  
  
##### <a name="kerberos-authentication-service"></a>Serviço de autenticação Kerberos  
Essa subcategoria relata eventos gerados pelo serviço de autenticação Kerberos. Esses eventos ocorrem no computador autoritativo para as credenciais.  
  
##### <a name="other-account-logon-events"></a>Outros eventos de Logon de conta  
Essa subcategoria relata os eventos que ocorrem em resposta às credenciais enviadas para uma solicitação de logon de conta de usuário que não se relacionam a validação de credenciais ou tíquetes Kerberos. Esses eventos ocorrem no computador autoritativo para as credenciais. Para contas de domínio, o controlador de domínio é autoritativo, enquanto para contas locais, o computador local é autoritativo.  
  
Em ambientes de domínio, a maioria dos eventos de logon de conta são registrados no log de segurança dos controladores de domínio autoritativos para as contas de domínio. No entanto, esses eventos podem ocorrer em outros computadores na organização quando contas locais são usadas para fazer logon. Exemplos podem incluir o seguinte:  
  
-   Desconexões da sessão de serviços de área de trabalho remotas  
  
-   Novas sessões de serviços de área de trabalho remota  
  
-   Bloqueio e desbloqueio de uma estação de trabalho  
  
-   Invocando um protetor de tela  
  
-   Descartar uma proteção de tela  
  
-   Detecção de uma Kerberos repetir ataque, em que uma solicitação Kerberos com informações idênticas é recebida duas vezes  
  
-   Acesso a uma rede sem fio concedido a uma conta de usuário ou computador  
  
-   Acesso a um com fio 802.1 x concedido a uma conta de usuário ou computador de rede  
  
#### <a name="account-management"></a>Gerenciamento de contas  
  
##### <a name="user-account-management"></a>Gerenciamento de contas de usuário  
Essa subcategoria relata cada evento de gerenciamento de conta de usuário, como quando uma conta de usuário é criada, alterada ou excluída; uma conta de usuário é renomeada, desabilitada ou habilitada; ou uma senha é definida ou alterada. Se essa configuração de política de auditoria é habilitada, os administradores podem rastrear eventos para detectar mal-intencionados, acidental e autorizada criação de contas de usuário.  
  
##### <a name="computer-account-management"></a>Gerenciamento de contas de computador  
Essa subcategoria relata cada evento de gerenciamento de conta de computador, como quando uma conta de computador é criada, alterada, excluída, renomeada, desabilitada ou habilitada.  
  
##### <a name="security-group-management"></a>Gerenciamento de grupo de segurança  
Essa subcategoria relata cada evento de gerenciamento de grupo de segurança, como quando um grupo de segurança é criado, alterado ou excluído ou quando um membro é adicionado ou removido de um grupo de segurança. Se essa configuração de política de auditoria é habilitada, os administradores podem rastrear eventos para detectar mal-intencionados, acidental e autorizada criação de contas de grupo de segurança.  
  
##### <a name="distribution-group-management"></a>Gerenciamento de grupo de distribuição  
Essa subcategoria relata cada evento de gerenciamento de grupo de distribuição, como quando um grupo de distribuição é criado, alterado ou excluído ou quando um membro é adicionado ou removido de um grupo de distribuição. Se essa configuração de política de auditoria é habilitada, os administradores podem rastrear eventos para detectar mal-intencionados, acidental e autorizada criação de contas de grupo.  
  
##### <a name="application-group-management"></a>Gerenciamento de grupo de aplicativos  
Essa subcategoria relata cada evento de gerenciamento de grupo de aplicativos em um computador, como quando um grupo de aplicativos é criado, alterado ou excluído ou quando um membro é adicionado ou removido de um grupo de aplicativos. Se essa configuração de política de auditoria é habilitada, os administradores podem rastrear eventos para detectar mal-intencionados, acidental e autorizada criação de contas de grupo do aplicativo.  
  
##### <a name="other-account-management-events"></a>Outros eventos de gerenciamento de conta  
Essa subcategoria relata outros eventos de gerenciamento de conta.  
  
#### <a name="detailed-process-tracking"></a>Rastreamento de processo detalhado  
  
##### <a name="process-creation"></a>Criação de processo  
Essa subcategoria relata a criação de um processo e o nome do usuário ou o programa que a criou.  
  
##### <a name="process-termination"></a>Término do processo  
Essa subcategoria relata quando um processo é encerrado.  
  
##### <a name="dpapi-activity"></a>Atividades DPAPI  
Essa subcategoria relata criptografar ou descriptografar chama a dados proteção API (DPAPI). DPAPI é usada para proteger informações secretas, como a senha armazenada e informações importantes.  
  
##### <a name="rpc-events"></a>Eventos RPC  
Esse procedimento remoto de relatórios de subcategoria chamar eventos de conexão (RPC).  
  
#### <a name="directory-service-access"></a>Acesso ao serviço de diretório  
  
##### <a name="directory-service-access"></a>Acesso ao serviço de diretório  
Essa subcategoria relata quando um objeto do AD DS é acessado. Somente objetos com SACLs configuradas causam eventos de auditoria ser gerado e somente quando são acessados de maneira correspondente as entradas SACL. Esses eventos são semelhantes aos eventos de acesso do serviço de diretório em versões anteriores do Windows Server. Essa subcategoria se aplica somente a controladores de domínio.  
  
##### <a name="directory-service-changes"></a>Alterações de serviço de diretório  
Essa subcategoria informa as alterações nos objetos no AD DS. Os tipos de alterações relatadas são criar, modificar, mover e cancelamento da exclusão de operações que são executadas em um objeto. Diretório serviço auditoria de alteração, quando apropriado, indica os valores novos e antigos das propriedades alteradas dos objetos que foram alteradas. Somente objetos com SACLs causam eventos de auditoria ser gerado e somente quando são acessados de maneira correspondente suas entradas SACL. Alguns objetos e propriedades não fazem eventos de auditoria serem gerados por causa de configurações na classe de objeto no esquema. Essa subcategoria se aplica somente a controladores de domínio.  
  
##### <a name="directory-service-replication"></a>Replicação do serviço de diretório  
Essa subcategoria relata quando a replicação entre dois controladores de domínio começa e termina.  
  
##### <a name="detailed-directory-service-replication"></a>Replicação do serviço de diretório detalhadas  
Essa subcategoria relata informações detalhadas sobre as informações replicadas entre controladores de domínio. Esses eventos podem ser muito altos em volume.  
  
#### <a name="logonlogoff"></a>Logon/Logoff  
  
##### <a name="logon"></a>Logon  
Essa subcategoria relata quando um usuário tenta fazer logon sistema. Esses eventos ocorrem no computador acessado. Para logons interativos, a geração desses eventos ocorre no computador que está conectado. Se um logon de rede ocorre para acessar um compartilhamento, esses eventos geram no computador que hospeda o recurso acessado. Se essa configuração é definida como **sem auditoria**, é difícil ou impossível para determinar qual usuário tem acessado ou tentou acessar computadores da organização.  
  
##### <a name="network-policy-server"></a>Servidor de política de rede  
Essa subcategoria relata eventos gerados pelo RADIUS (IAS) e proteção de acesso à rede (NAP) solicitações de acesso de usuário. Essas solicitações podem ser **concessão**, **negar**, **descartar**, **quarentena**, **bloqueio**, e **desbloqueio**. Esta configuração de auditoria resultará em um volume médio ou alto de registros em servidores NPS e IAS.  
  
##### <a name="ipsec-main-mode"></a>Modo principal IPsec  
Essa subcategoria relata os resultados do protocolo de Internet Key Exchange (IKE) e do protocolo de Internet autenticados Authenticated durante as negociações de modo principal.  
  
##### <a name="ipsec-extended-mode"></a>Modo estendido IPsec  
Essa subcategoria relata os resultados de AuthIP durante as negociações de modo estendido.  
  
##### <a name="other-logonlogoff-events"></a>Outros eventos de Logon/Logoff  
Essa subcategoria relata outro logon e eventos de logoff relacionadas, como área de trabalho serviços remota se reconectar e se desconectar de sessão usando RunAs para executar processos em uma conta diferente e bloquear e desbloquear uma estação de trabalho.  
  
##### <a name="logoff"></a>Logoff  
Essa subcategoria relata quando um usuário faz logoff do sistema. Esses eventos ocorrem no computador acessado. Para logons interativos, a geração desses eventos ocorre no computador que está conectado. Se um logon de rede ocorre para acessar um compartilhamento, esses eventos geram no computador que hospeda o recurso acessado. Se essa configuração é definida como **sem auditoria**, é difícil ou impossível para determinar qual usuário tem acessado ou tentou acessar computadores da organização.  
  
##### <a name="account-lockout"></a>Bloqueio de conta  
Essa subcategoria relata quando uma conta de usuário é bloqueada como resultado de muitas tentativas de logon com falha.  
  
##### <a name="ipsec-quick-mode"></a>Modo rápido IPsec  
Essa subcategoria relata os resultados do protocolo IKE e AuthIP durante as negociações de modo rápido.  
  
##### <a name="special-logon"></a>Logon especial  
Essa subcategoria relata quando um logon especial é usado. Um logon especial é um logon que tem privilégios equivalentes de administrador e pode ser usado para elevar um processo para um nível superior.  
  
#### <a name="policy-change"></a>Alteração da política  
  
##### <a name="audit-policy-change"></a>Alteração de política de auditoria  
Essa subcategoria informa as alterações feitas na política de auditoria, incluindo alterações SACL.  
  
##### <a name="authentication-policy-change"></a>Alteração de política de autenticação  
Essa subcategoria informa as alterações feitas na política de autenticação.  
  
##### <a name="authorization-policy-change"></a>Alteração de políticas de autorização  
Essa subcategoria informa as alterações feitas na política de autorização, incluindo alterações de permissões (DACL).  
  
##### <a name="mpssvc-rule-level-policy-change"></a>Alteração na política de nível de regra MPSSVC  
Essa subcategoria relata as alterações nas regras de política usadas pelo (MPSSVC.exe) serviço de proteção Microsoft. Esse serviço é usado pelo Firewall do Windows.  
  
##### <a name="filtering-platform-policy-change"></a>Alteração de política de plataforma de filtragem  
Essa subcategoria relata a adição e remoção dos objetos de WFP, incluindo os filtros de inicialização. Esses eventos podem ser muito altos em volume.  
  
##### <a name="other-policy-change-events"></a>Outros eventos de alteração de política  
Essa subcategoria relata outros tipos de alterações de política de segurança, como configuração do Trusted Platform Module (TPM) ou provedores de criptografia.  
  
#### <a name="privilege-use"></a>Uso de privilégio  
  
##### <a name="sensitive-privilege-use"></a>Uso de privilégio importante  
Essa subcategoria relata quando uma conta de usuário ou serviço usa um privilégio importante. Um privilégio confidencial inclui os seguintes direitos de usuário: atuar como parte do sistema operacional, fazer backup de arquivos e diretórios, crie um objeto token, depurar programas, habilitar computador e contas de usuário para serem confiáveis para delegação, gerar auditorias de segurança, representar um cliente após autenticação, carregar e descarregar drivers de dispositivo, Gerenciar auditoria e log de segurança, modificar valores de ambiente de firmware, substituir um token no nível de processo, restaurar arquivos e diretórios e apropriar-se de arquivos ou outros objetos. Essa subcategoria de auditoria, você criará um grande volume de eventos.  
  
##### <a name="nonsensitive-privilege-use"></a>Uso de privilégios não confidenciais  
Essa subcategoria relata quando uma conta de usuário ou serviço usa um privilégio não confidenciais. Um privilégio não confidenciais inclui os seguintes direitos de usuário: acessar o Gerenciador de credenciais como chamador confiável, acessar este computador pela rede, Adicionar estações de trabalho ao domínio, ajustar cotas de memória para um processo, permitir logon localmente, permitir logon pelos serviços de área de trabalho remota, ignorar a verificação completa, alterar a hora do sistema, crie um arquivo de paginação, criar objetos globais, criar objetos compartilhados permanentemente, criar links simbólicos, negar acesso este computador pela rede, Negar logon como um trabalho em lotes, Negar logon como um serviço, Negar logon localmente, Negar logon pelos serviços de área de trabalho remota, forçar o desligamento a partir de um sistema remoto, aumentar a um conjunto de trabalho do processo, aumentar a prioridade de agendamento, bloquear páginas na memória, faça logon como um trabalho em lotes, faça logon como um serviço, modificar rótulo de objeto, realizar tarefas de manutenção de volume, o perfil de um único processo, o desempenho do sistema de perfil, remover computador da Base de encaixe, desligar o sistema e sincronizar dados do serviço de diretório. Essa subcategoria de auditoria, você criará um volume muito grande de eventos.  
  
##### <a name="other-privilege-use-events"></a>Outros eventos de uso de privilégio  
Esta configuração de política de segurança não é usada no momento.  
  
#### <a name="object-access"></a>Acesso a objetos  
  
##### <a name="file-system"></a>Sistema de arquivos  
Essa subcategoria relata quando objetos do sistema de arquivos são acessados. Somente objetos de sistema de arquivos com SACLs causam eventos de auditoria ser gerado e somente quando são acessados de maneira que suas entradas SACL correspondentes. Por si só, essa configuração de política não causará de todos os eventos de auditoria. Determina a auditoria do evento de um usuário que acessa um objeto do sistema de arquivo que tem uma lista de controle de acesso especificado do sistema (SACL), efetivamente habilitar a auditoria para que ela ocorra.  
  
Se a configuração Auditoria de acesso a objetos estiver configurado para **sucesso**, uma entrada de auditoria é gerada sempre que um usuário acessa com êxito um objeto com uma SACL especificada. Se essa configuração de política é definida como **falha**, uma entrada de auditoria é gerada sempre que um usuário falha na tentativa de acessar um objeto com uma SACL especificada.  
  
##### <a name="registry"></a>Registro  
Essa subcategoria relata quando são acessados objetos do registro. Somente os objetos do registro com SACLs causam eventos de auditoria ser gerado e somente quando são acessados de maneira que suas entradas SACL correspondentes. Por si só, essa configuração de política não causará de todos os eventos de auditoria.  
  
##### <a name="kernel-object"></a>Objeto de kernel  
Essa subcategoria relata quando são acessados objetos de kernel, como exclusões mútuas e de processos. Somente os objetos de kernel com SACLs causam eventos de auditoria ser gerado e somente quando são acessados de maneira que suas entradas SACL correspondentes. Normalmente objetos de kernel só recebem SACLs caso as opções de auditoria AuditBaseObjects ou AuditBaseDirectories estão habilitadas.  
  
##### <a name="sam"></a>SAM  
Essa subcategoria relata quando são acessados objetos de banco de dados de autenticação de Gerenciador de contas de segurança (SAM) local.  
  
##### <a name="certification-services"></a>Serviços de certificação  
Essa subcategoria relata quando os serviços de certificação operações serão executados.  
  
##### <a name="application-generated"></a>Aplicativo gerado  
Essa subcategoria relata quando aplicativos tentam geram eventos de auditoria usando o interfaces de programação de aplicativo (APIs) de auditoria do Windows.  
  
##### <a name="handle-manipulation"></a>Manipulação de identificador  
Essa subcategoria relata quando um identificador de um objeto é aberto ou fechado. Somente objetos com SACLs causam esses eventos ser gerado e somente se a operação de identificador tentada corresponder as entradas SACL. Eventos de manipulação de identificador são gerados apenas para tipos de objeto em que a subcategoria de acesso do objeto correspondente está habilitada (por exemplo, o sistema de arquivos ou do registro).  
  
##### <a name="file-share"></a>Compartilhamento de arquivos  
Essa subcategoria relata quando um compartilhamento de arquivos é acessado. Por si só, essa configuração de política não causará de todos os eventos de auditoria. Determina a auditoria do evento de um usuário que acessa um objeto de compartilhamento de arquivo que tem uma lista de controle de acesso especificado do sistema (SACL), efetivamente habilitar a auditoria para que ela ocorra.  
  
##### <a name="filtering-platform-packet-drop"></a>Descarte de pacote de plataforma de filtragem  
Essa subcategoria relata quando pacotes são removidos pela plataforma de filtragem Windows (WFP). Esses eventos podem ser muito altos em volume.  
  
##### <a name="filtering-platform-connection"></a>Conexão de plataforma de filtragem  
Essa subcategoria relata quando conexões são permitidas ou bloqueadas pela WFP. Esses eventos podem ser altos em volume.  
  
##### <a name="other-object-access-events"></a>Outros eventos de acesso a objeto  
Essa subcategoria relata outros eventos relacionados ao acesso a objetos, como trabalhos do Agendador de tarefas e objetos COM+.  
  
#### <a name="system"></a>Sistema  
  
##### <a name="security-state-change"></a>Alteração de estado de segurança  
Essa subcategoria relata as alterações no estado de segurança do sistema, como quando o subsistema de segurança inicia e para.  
  
##### <a name="security-system-extension"></a>Extensão do sistema de segurança  
Essa subcategoria relata o carregamento do código de extensão, como pacotes de autenticação, o subsistema de segurança.  
  
##### <a name="system-integrity"></a>Integridade do sistema  
Essa subcategoria relatórios sobre violações de integridade do subsistema de segurança.  
  
Driver IPsec  
  
Essa subcategoria relatórios sobre as atividades do driver de segurança (IPsec) do protocolo de Internet.  
  
##### <a name="other-system-events"></a>Outros eventos do sistema  
Essa subcategoria relatórios sobre outros eventos do sistema.  
  
Para saber mais sobre as descrições de subcategoria, consulte o [ferramenta do Microsoft Security Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx).  
  
Cada organização deve revisar as categorias cobertas anteriores e subcategorias e habilite aqueles que mais adequadas ao seu ambiente. Alterações na política de auditoria sempre devem ser testadas antes da implantação em um ambiente de produção.  
  
### <a name="configuring-windows-audit-policy"></a>Configurar política de auditoria do Windows  
Política de auditoria do Windows pode ser definida usando as edições do registro, auditpol.exe, APIs ou políticas de grupo. Os métodos recomendados para a configuração de política de auditoria para a maioria das empresas são a política de grupo ou auditpol.exe. Configuração de política de auditoria do sistema requer permissões da conta de nível de administrador ou as permissões delegadas apropriadas.  
  
> [!NOTE]  
> O **Gerenciar auditoria e log de segurança** privilégio deve ser concedido a entidades de segurança (administradores têm-os por padrão) para permitir que a modificação de opções de recursos individuais, como arquivos, objetos do Active Directory e chaves do registro de auditoria de acesso a objetos.  
  
#### <a name="setting-windows-audit-policy-by-using-group-policy"></a>Configuração política de auditoria do Windows usando a política de grupo  
Para definir a política de auditoria usando políticas de grupo, configure as categorias de auditoria apropriada localizadas sob **locais \ política de computador Configuration\Windows Settings\Security** (veja a captura de tela a seguir para obter um exemplo de (gpedit.msc) o Editor de política de Grupo Local). Cada categoria de política de auditoria pode ser ativada para **sucesso**, **falha**, ou **sucesso** e eventos de falha.  
  
![Anúncio de monitoramento](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_6.gif)  
  
Política de auditoria avançada pode ser definida por meio do Active Directory ou políticas de grupo local. Para definir a política de auditoria avançada, configure as subcategorias apropriadas localizadas sob **do computador \ Windows \ Configuração avançada auditoria política** (veja a captura de tela a seguir para obter um exemplo de (gpedit.msc) o Editor de política de Grupo Local). Cada subcategoria de política de auditoria pode ser ativada para **sucesso**, **falha**, ou **sucesso** e **falha** eventos.  
  
![Anúncio de monitoramento](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_7.gif)  
  
#### <a name="setting-windows-audit-policy-using-auditpolexe"></a>Configuração política de auditoria do Windows usando Auditpol.exe  
Auditpol.exe (para configurar a política de auditoria do Windows) foi introduzida no Windows Server 2008 e Windows Vista. Inicialmente, apenas auditpol.exe poderiam ser usados para definir a política de auditoria avançada, mas a política de grupo pode ser usada no Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008, Windows 8 e Windows 7.  
  
Auditpol.exe é um utilitário de linha de comando. A sintaxe é a seguinte:  
  
**auditpol//set / < categoria | Subcategoria >:<audit category> / < sucesso | falha: > / < habilitar | desabilitar >**  
  
Exemplos de sintaxe Auditpol.exe:  
  
**auditpol//set /subcategory: "gerenciamento de contas de usuário" /success: habilitar /failure: habilitar**  
  
**auditpol//set /subcategory: "logon" /success: habilitar /failure: habilitar**  
  
**auditpol//set /subcategory: "Modo IPSEC principal" /failure: habilitar**  
  
> [!NOTE]  
> Auditpol.exe define a política de auditoria avançada localmente. Se a política local em conflito com o Active Directory ou política de grupo local, as configurações de política de grupo geralmente prevalecem sobre configurações de auditpol.exe. Quando há vários conflitos de política local ou grupo, apenas uma política prevalecerão (isto é, substituir). Não mesclará as políticas de auditoria.  
  
#### <a name="scripting-auditpol"></a>Scripts Auditpol  
A Microsoft fornece um [script de exemplo](https://support.microsoft.com/kb/921469) para administradores que desejam definir a política de auditoria avançada usando um script em vez de digitar manualmente em cada comando auditpol.exe.  
  
**Observação** política de grupo não sempre com precisão relatar o status de todas as políticas de auditoria habilitados, enquanto faz auditpol.exe. Consulte [Obtendo a política de auditoria efetiva no Windows 7 e Windows 2008 R2](http://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx) para obter mais detalhes.  
  
#### <a name="other-auditpol-commands"></a>Outros comandos Auditpol  
Auditpol.exe pode ser usado para salvar e restaurar uma política de auditoria local e para exibir outros comandos relacionados a auditoria. Aqui estão os outros **auditpol** comandos.  
  
**auditpol/desmarque** - utilizado para limpar e redefinir as políticas de auditoria local  
  
**auditpol /backup /file:<filename>** - usada para fazer backup de uma política de auditoria local atual em um arquivo binário  
  
**auditpol /restore /file:<filename>** - utilizado para importar um arquivo de política de auditoria salvo anteriormente uma política de auditoria local  
  
**auditpol / < get/conjunto > /option:<CrashOnAuditFail> / < habilitar/desabilitar >** -se essa configuração de política de auditoria é habilitada, ele faz com que o sistema parar imediatamente (com parada: C0000244 {Falha na auditoria} mensagem) se uma auditoria de segurança não puder ser registrada por qualquer motivo. Normalmente, um evento deixa de ser registrado quando o log de auditoria de segurança está cheio e o método de retenção especificado para o log de segurança é **não substituir eventos** ou **substituir eventos periodicamente**. Normalmente ele só é habilitado por ambientes que precisam de uma maior garantia de registro em log no log de segurança. Se habilitada, os administradores devem estreitamente assistir o tamanho do log de segurança e gire logs conforme necessário. Também podem ser definida com a política de grupo, modificando a opção de segurança **auditoria: desligar o sistema imediatamente se não for possível o log de auditorias de segurança** (padrão = disabled).  
  
**Auditpol / < get/conjunto > /option:<AuditBaseObjects> / < habilitar/desabilitar >** -esta configuração de política de auditoria o determina a auditoria do acesso a objetos do sistema global. Se essa política estiver habilitada, ele faz com que os objetos de sistema, como exclusões mútuas, eventos, semáforos e dispositivos DOS serão criados com uma lista de controle de acesso do sistema (SACL) padrão. A maioria dos administradores considere auditoria objetos globais do sistema para ser muito "exagerado", e eles serão habilitá-lo somente se você suspeitar de hackers mal-intencionados. Somente objetos nomeados recebem uma SACL. Se a auditoria objeto acesso auditoria política (ou subcategoria de auditoria de objeto Kernel) também estiver habilitada, o acesso a esses objetos do sistema será auditado. Ao definir essa configuração de segurança, as alterações não terão efeito até você reiniciar o Windows. Essa política também pode ser definida com a política de grupo, modificando a opção de segurança auditoria do acesso a objetos do sistema global (padrão = disabled).  
  
**auditpol / < get/conjunto > /option:<AuditBaseDirectories> / < habilitar/desabilitar >** -esta configuração de política de auditoria especifica que os objetos de kernel nomeado (por exemplo, mutexes e semáforos) devem ter as SACLs quando eles são criados. AuditBaseDirectories afeta objetos de contêiner enquanto AuditBaseObjects afeta objetos que não podem conter outros objetos.  
  
**Auditpol / < get/conjunto > /option:<FullPrivilegeAuditing> / < habilitar/desabilitar >** -  
  
Esta configuração de política de auditoria especifica se o cliente gera um evento quando um ou mais desses privilégios são atribuídas a um token de segurança do usuário: AssignPrimaryTokenPrivilege, AuditPrivilege, BackupPrivilege, CreateTokenPrivilege, DebugPrivilege, EnableDelegationPrivilege, ImpersonatePrivilege, LoadDriverPrivilege, RestorePrivilege, SecurityPrivilege, SystemEnvironmentPrivilege, TakeOwnershipPrivilege e TcbPrivilege. Se essa opção não estiver habilitada (padrão = Disabled), os privilégios BackupPrivilege e RestorePrivilege não são registrados. Habilitar essa opção pode fazer o log de segurança extremamente perturbador (às vezes centenas de eventos por segundo) durante uma operação de backup. Essa política também pode ser definida com a política de grupo, modificando a opção de segurança **auditoria: fazer auditoria do uso dos privilégios Backup e restauração**.  
  
> [!NOTE]  
> Algumas informações fornecidas aqui foi extraídas do Microsoft [tipo de opção de auditoria](https://msdn.microsoft.com/library/dd973862(prot.20).aspx) e a ferramenta Microsoft SCM.  
  
### <a name="enforcing-traditional-auditing-or-advanced-auditing"></a>Aplicação de auditoria avançada ou auditoria tradicional  
No Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows 8, Windows 7 e Windows Vista, os administradores podem optar para habilitar as nove categorias tradicionais ou usar as subcategorias. É uma opção binária que deve ser feita em cada sistema Windows. As principais categorias podem ser habilitadas tanto o subcategoriesit não pode ser ambos.  
  
Para impedir que a política de categoria tradicional herdados substituam as subcategorias de política de auditoria, você deve habilitar o **forçar configurações de subcategorias de política de auditoria (Windows Vista ou posterior) para substituir configurações de categorias de política de auditoria** configuração de política localizado em **Computer Configuration\Windows Settings\Security Settings\Local Policies\Security Options**.  
  
Recomendamos que as subcategorias de ser habilitadas e configuradas em vez das principais nove categorias. Isso requer que uma configuração de política de grupo ser habilitado (para permitir subcategorias substituir as categorias de auditoria) juntamente com Configurando as subcategorias diferentes que dão suporte a políticas de auditoria.  
  
Subcategorias de auditoria podem ser configurada usando vários métodos, incluindo a política de grupo e o programa de linha de comando auditpol.exe.  
  
Para saber mais sobre auditoria do Windows, consulte os artigos a seguir:  
  
-   [Segurança avançada, auditoria no Windows 7 e Windows Server 2008 R2](https://social.technet.microsoft.com/wiki/contents/articles/advanced-security-auditing-in-windows-7-and-windows-server-2008-r2.aspx)  
  
-   [Auditoria e conformidade no Windows Server 2008](https://technet.microsoft.com/magazine/2008.03.auditing.aspx)  
  
-   [Como usar a política de grupo para definir configurações para computadores baseados em Windows Server 2008 e Windows Vista em um domínio do Windows Server 2008, em um domínio do Windows Server 2003 ou em um domínio do Windows 2000 de auditoria de segurança detalhada](https://support.microsoft.com/kb/921469)  
  
-   [Avançada de política de auditoria de segurança Step-by-Step guia](https://technet.microsoft.com/library/dd408940(WS.10).aspx)  
  


