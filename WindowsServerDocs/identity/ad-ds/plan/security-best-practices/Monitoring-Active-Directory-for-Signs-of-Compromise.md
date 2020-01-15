---
ms.assetid: a7ef2fba-b05c-4be2-93b2-b9456244c3ad
title: Monitorar o Active Directory em busca de sinais de comprometimento
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: e51b7ea151db1ca5d53a8cacef3b042e345175de
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949630"
---
# <a name="monitoring-active-directory-for-signs-of-compromise"></a>Monitorar o Active Directory em busca de sinais de comprometimento

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Número da lei cinco: a vigilância eternas é o preço da segurança.* - [10 leis imutáveis da administração de segurança](https://technet.microsoft.com/library/cc722488.aspx)  
  
Um sistema de monitoramento de log de eventos sólido é uma parte crucial de qualquer design de Active Directory seguro. Muitos comprometimentos de segurança de computador poderiam ser descobertos no início do evento se as vítimas imprometessem o monitoramento e o alerta do log de eventos apropriado. Os relatórios independentes têm muito suporte para essa conclusão. Por exemplo, o [relatório de violação de dados 2009 Verizon](http://www.verizonbusiness.com/resources/security/reports/2009_databreach_rp.pdf) informa:  
  
"A ineficiência aparente do monitoramento de eventos e da análise de log continua sendo um pouco de uma enigma. A oportunidade de detecção está lá; os investigadores observaram que 66 por cento das vítimas tinham evidências suficientes disponíveis em seus logs para descobrir que a violação tinha sido mais empenhada em analisar esses recursos. "  
  
Essa falta de monitoramento de logs de eventos ativos continua sendo um ponto fraco consistente em muitos planos de defesa de segurança de empresas. O [relatório de violações de dados 2012 Verizon](http://www.verizonbusiness.com/resources/reports/rp_data-breach-investigations-report-2012_en_xg.pdf) descobriu que, embora 85% das violações levasse várias semanas para ser notado, 84% das vítimas tinha evidências da violação em seus logs de eventos.  
  
## <a name="windows-audit-policy"></a>Política de auditoria do Windows

Veja a seguir links para o blog de suporte do Microsoft Official Enterprise. O conteúdo desses Blogs fornece conselhos, orientações e recomendações sobre auditoria que ajudarão você a aprimorar a segurança de sua infraestrutura de Active Directory e é um recurso valioso ao criar uma diretiva de auditoria.  
  
* A [auditoria de acesso a objetos globais é mágica](https://blogs.technet.com/b/askds/archive/2011/03/10/global-object-access-auditing-is-magic.aspx) -descreve um mecanismo de controle chamado configuração de política de auditoria avançada que foi adicionada ao Windows 7 e ao windows Server 2008 R2 que permite definir quais tipos de dados você deseja auditar facilmente e não para manipular scripts e Auditpol. exe.  
* [Apresentando alterações de auditoria no windows 2008](https://blogs.technet.com/b/askds/archive/2007/10/19/introducing-auditing-changes-in-windows-2008.aspx) -apresenta as alterações de auditoria feitas no windows Server 2008.  
* [Truques de auditoria interessantes no Vista e 2008](https://blogs.technet.com/b/askds/archive/2007/11/16/cool-auditing-tricks-in-vista-and-2008.aspx) -explica os recursos de auditoria interessantes do Windows Vista e do windows Server 2008 que podem ser usados para solucionar problemas ou ver o que está acontecendo em seu ambiente.  
* A [loja única para auditoria no Windows server 2008 e no Windows Vista](https://blogs.technet.com/b/askds/archive/2008/03/27/one-stop-shop-for-auditing-in-windows-server-2008-and-windows-vista.aspx) -contém uma compilação de recursos de auditoria e informações contidas no windows Server 2008 e no Windows Vista.  
  
Os links a seguir fornecem informações sobre melhorias na auditoria do Windows no Windows 8 e no Windows Server 2012, além de informações sobre AD DS auditoria no Windows Server 2008.  
  
* [O que há de novo na auditoria de segurança](https://technet.microsoft.com/library/hh849638.aspx) – fornece uma visão geral dos novos recursos de auditoria de segurança no Windows 8 e no windows Server 2012.  
* [Guia passo a passo de AD DS auditoria](https://technet.microsoft.com/library/a9c25483-89e2-4202-881c-ea8e02b4b2a5.aspx) – descreve o novo recurso de auditoria de Active Directory Domain Services (AD DS) no Windows Server 2008. Ele também fornece procedimentos para implementar esse novo recurso.  
  
### <a name="windows-audit-categories"></a>Categorias de auditoria do Windows

Antes do Windows Vista e do Windows Server 2008, o Windows tinha apenas nove categorias de diretiva de auditoria de log de eventos:  
  
* Eventos de logon de conta  
* Gerenciamento de contas  
* Acesso ao serviço de diretório  
* Eventos de logon  
* Acesso a objeto  
* Alteração de Política  
* Uso de privilégios  
* Controle de processos  
* Eventos do sistema  
  
Essas nove categorias de auditoria tradicionais compõem uma diretiva de auditoria. Cada categoria de política de auditoria pode ser habilitada para eventos de êxito, falha ou êxito e falha. Suas descrições são incluídas na próxima seção.  
  
#### <a name="audit-policy-category-descriptions"></a>Descrições da categoria da política de auditoria  
As categorias de política de auditoria habilitam os seguintes tipos de mensagem de log de eventos.  
  
##### <a name="audit-account-logon-events"></a>Eventos de logon de conta de auditoria  
Relata cada instância de uma entidade de segurança (por exemplo, usuário, computador ou conta de serviço) que está fazendo logon ou logoff de um computador no qual outro computador é usado para validar a conta. Os eventos de logon de conta são gerados quando uma conta de entidade de segurança de domínio é autenticada em um controlador de domínio. A autenticação de um usuário local em um computador local gera um evento de logon que é registrado no log de segurança local. Nenhum evento de logoff de conta é registrado.  
  
Essa categoria gera muitos "ruídos" porque o Windows está constantemente tendo contas de logon e logoff dos computadores locais e remotos durante o curso de negócios normal. Ainda assim, qualquer plano de segurança deve incluir o êxito e a falha dessa categoria de auditoria.  
  
##### <a name="audit-account-management"></a>Gerenciamento de conta de auditoria  
Essa configuração de auditoria determina se o gerenciamento de usuários e grupos deve ser acompanhado. Por exemplo, os usuários e grupos devem ser controlados quando uma conta de usuário ou computador, um grupo de segurança ou um grupo de distribuição é criado, alterado ou excluído; Quando uma conta de usuário ou computador é renomeada, desabilitada ou habilitada; ou quando uma senha de usuário ou computador é alterada. Um evento pode ser gerado para usuários ou grupos que são adicionados ou removidos de outros grupos.  
  
##### <a name="audit-directory-service-access"></a>Auditoria de acesso do serviço de diretório  

Essa configuração de política determina se deve-se auditar o acesso de entidade de segurança a um objeto Active Directory que tenha sua própria SACL (lista de controle de acesso) do sistema especificada. Em geral, essa categoria só deve ser habilitada em controladores de domínio. Quando habilitada, essa configuração gera muitos "ruídos".  
  
##### <a name="audit-logon-events"></a>Eventos de logon de auditoria  
Os eventos de logon são gerados quando uma entidade de segurança local é autenticada em um computador local. Eventos de logon registra logons de domínio que ocorrem no computador local. Eventos de logoff da conta não são gerados. Quando habilitado, os eventos de logon geram muitos "ruídos", mas devem ser habilitados por padrão em qualquer plano de auditoria de segurança.  
  
##### <a name="audit-object-access"></a>Auditoria de acesso a objetos  
O acesso ao objeto pode gerar eventos quando os objetos definidos posteriormente com auditoria habilitada são acessados (por exemplo, aberto, lido, renomeado, excluído ou fechado). Depois que a categoria de auditoria principal estiver habilitada, o administrador deverá definir individualmente quais objetos terão a auditoria habilitada. Muitos objetos do sistema Windows vêm com a auditoria habilitada, portanto, habilitar essa categoria normalmente começará a gerar eventos antes que o administrador tenha definido qualquer um.  
  
Essa categoria é muito "ruidosa" e gerará cinco a dez eventos para cada acesso de objeto. Pode ser difícil para os administradores novos na auditoria de objetos obter informações úteis. Ele só deve ser habilitado quando necessário.  
  
##### <a name="auditing-policy-change"></a>Alteração de política de auditoria  
Essa configuração de política determina se deve auditar cada incidência de uma alteração nas políticas de atribuição de direitos do usuário, políticas de firewall do Windows, políticas de confiança ou alterações na política de auditoria. Essa categoria deve ser habilitada em todos os computadores. Ele gera muito pouco ruído.  
  
##### <a name="audit-privilege-use"></a>Uso de privilégios de auditoria  

Há dezenas de direitos de usuário e permissões no Windows (por exemplo, fazer logon como um trabalho em lotes e agir como parte do sistema operacional). Essa configuração de política determina se deve auditar cada instância de uma entidade de segurança, exercitando um direito de usuário ou privilégio. A habilitação dessa categoria resulta em muitos "ruído", mas pode ser útil para rastrear contas de entidade de segurança usando privilégios elevados.  
  
##### <a name="audit-process-tracking"></a>Controle de processo de auditoria  
Essa configuração de política determina se é para auditar informações detalhadas de controle de processos para eventos como ativação do programa, saída do processo, tratamento de duplicação e acesso indireto a objetos. Ele é útil para controlar usuários mal-intencionados e os programas que eles usam.  
  
Habilitar o controle de processo de auditoria gera um grande número de eventos, portanto, normalmente ele é definido como **sem auditoria**. No entanto, essa configuração pode fornecer um grande benefício durante uma resposta a incidentes do log detalhado dos processos iniciados e a hora em que foram iniciadas. Para controladores de domínio e outros servidores de infraestrutura de função única, essa categoria pode ser ativada com segurança sempre. Os servidores de função única não geram muito tráfego de controle de processos durante o curso normal de suas tarefas. Assim, eles podem ser habilitados para capturar eventos não autorizados, se eles ocorrerem.  
  
##### <a name="system-events-audit"></a>Auditoria de eventos do sistema  

Os eventos do sistema são quase uma categoria genérica catch-all, registrando vários eventos que afetam o computador, a segurança do sistema ou o log de segurança. Ele inclui eventos para desligamentos e reinicializações do computador, falhas de energia, alterações de horário do sistema, inicializações do pacote de autenticação, limpezas do log de auditoria, problemas de representação e um host de outros eventos gerais. Em geral, a habilitação dessa categoria de auditoria gera uma grande quantidade de "ruído", mas gera eventos muito úteis suficientes que é difícil recomendar não habilitá-la.  
  
#### <a name="advanced-audit-policies"></a>Políticas de auditoria avançadas

A partir do Windows Vista e do Windows Server 2008, a Microsoft melhorou a maneira como as seleções de categoria de log de eventos podem ser feitas pela criação de subcategorias em cada categoria de auditoria principal. As subcategorias permitem que a auditoria seja muito mais granular do que poderia, caso contrário, usando as categorias principais. Usando subcategorias, você pode habilitar apenas partes de uma categoria principal específica e ignorar a geração de eventos para os quais você não tem uso. Cada subcategoria de política de auditoria pode ser habilitada para eventos de Êxito, Falha ou Êxito e Falha.  
  
Para listar todas as subcategorias de auditoria disponíveis, examine o contêiner política de auditoria avançada em um objeto Política de Grupo ou digite o seguinte em um prompt de comando em qualquer computador que esteja executando o Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008, Windows 8, Windows 7 ou Windows Vista:  
  
`auditpol /list /subcategory:*`
  
Para obter uma lista de subcategorias de auditoria configuradas no momento em um computador que executa o Windows Server 2012, o Windows Server 2008 R2 ou o Windows 2008, digite o seguinte:  
  
`auditpol /get /category:*`
  
A captura de tela a seguir mostra um exemplo de Auditpol. exe que lista a política de auditoria atual.  
  
![monitorando o AD](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_5.gif)  
  
> [!NOTE]  
> O Política de Grupo nem sempre relata com precisão o status de todas as políticas de auditoria habilitadas, enquanto o Auditpol. exe faz isso. Consulte [obtendo a política de auditoria efetiva no Windows 7 e 2008 R2](https://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx) para obter mais detalhes.  
  
Cada categoria principal tem várias subcategorias. Abaixo está uma lista de categorias, suas subcategorias e uma descrição de suas funções.  
  
### <a name="auditing-subcategories-descriptions"></a>Descrições das subcategorias de auditoria  
As subcategorias de política de auditoria habilitam os seguintes tipos de mensagem de log de eventos:  
  
#### <a name="account-logon"></a>Logon de conta  
  
##### <a name="credential-validation"></a>Validação de credenciais  
Essa subcategoria relata os resultados de testes de validação em credenciais enviadas para uma solicitação de logon de conta de usuário. Esses eventos ocorrem no computador autoritativo para as credenciais. Para contas de domínio, o controlador de domínio é autoritativo, enquanto para contas locais, o computador local é autoritativo.  
  
Em ambientes de domínio, a maioria dos eventos de logon de conta é registrada no log de segurança dos controladores de domínio que são autoritativos para as contas de domínio. No entanto, esses eventos podem ocorrer em outros computadores da organização quando contas locais são usadas para fazer logon.  
  
##### <a name="kerberos-service-ticket-operations"></a>Auditoria de Operações do Tíquete de Serviço Kerberos  
Essa subcategoria relata eventos gerados por processos de solicitação de tíquete Kerberos no controlador de domínio que é autoritativo para a conta de domínio.  
  
##### <a name="kerberos-authentication-service"></a>Serviço de autenticação Kerberos  
Essa subcategoria relata eventos gerados pelo serviço de autenticação Kerberos. Esses eventos ocorrem no computador autoritativo para as credenciais.  
  
##### <a name="other-account-logon-events"></a>Outros eventos de Logon de Conta  
Essa subcategoria relata os eventos que ocorrem em resposta a credenciais enviadas para uma solicitação de logon de conta de usuário que não se relacionam a validação de credencial ou tíquetes Kerberos. Esses eventos ocorrem no computador autoritativo para as credenciais. Para contas de domínio, o controlador de domínio é autoritativo, enquanto para contas locais, o computador local é autoritativo.  
  
Em ambientes de domínio, a maioria dos eventos de logon de conta é registrada no log de segurança dos controladores de domínio que são autoritativos para as contas de domínio. No entanto, esses eventos podem ocorrer em outros computadores da organização quando contas locais são usadas para fazer logon. Entre os exemplos podem estar os seguintes:  
  
* Desconexões de Serviços de Área de Trabalho Remota sessão  
* Novas sessões de Serviços de Área de Trabalho Remota  
* Bloqueio e desbloqueio de uma estação de trabalho  
* Invocação de uma proteção de tela  
* Descarte de uma proteção de tela  
* Detecção de um ataque de reprodução de Kerberos, no qual uma solicitação de Kerberos com informações idênticas é recebida duas vezes  
* Acesso a uma rede sem fio concedido a uma conta de usuário ou computador  
* Acesso a uma rede com fio 802.1x concedido a uma conta de usuário ou computador  
  
#### <a name="account-management"></a>Gerenciamento de contas  
  
##### <a name="user-account-management"></a>Gerenciamento de contas de usuário  
Essa subcategoria relata cada evento de gerenciamento de conta de usuário, como quando uma conta de usuário é criada, alterada ou excluída; uma conta de usuário é renomeada, desabilitada ou habilitada; ou uma senha é definida ou alterada. Se essa configuração de diretiva de auditoria estiver habilitada, os administradores poderão rastrear eventos para detectar a criação de contas de usuário maliciosas, acidentais e mal-intencionadas.  
  
##### <a name="computer-account-management"></a>Gerenciamento de contas de computador  
Essa subcategoria relata cada evento de gerenciamento de conta de computador, como quando uma conta de computador é criada, alterada, excluída, renomeada, desabilitada ou habilitada.  
  
##### <a name="security-group-management"></a>Gerenciamento de grupo de segurança  
Essa subcategoria relata cada evento de gerenciamento de grupo de segurança, como quando um grupo de segurança é criado, alterado ou excluído ou quando um membro é adicionado ou removido de um grupo de segurança. Se essa configuração de diretiva de auditoria estiver habilitada, os administradores poderão rastrear eventos para detectar a criação mal-intencionada, acidental e autorizada de contas de grupo de segurança.  
  
##### <a name="distribution-group-management"></a>Gerenciamento do grupo de distribuição  
Essa subcategoria relata cada evento de gerenciamento de grupo de distribuição, como quando um grupo de distribuição é criado, alterado ou excluído ou quando um membro é adicionado ou removido de um grupo de distribuição. Se essa configuração de diretiva de auditoria estiver habilitada, os administradores poderão rastrear eventos para detectar a criação mal-intencionada, acidental e autorizada de contas de grupo.  
  
##### <a name="application-group-management"></a>Gerenciamento de grupo de aplicativos  
Essa subcategoria relata cada evento de gerenciamento de grupo de aplicativos em um computador, como quando um grupo de aplicativos é criado, alterado ou excluído ou quando um membro é adicionado ou removido de um grupo de aplicativos. Se essa configuração de diretiva de auditoria estiver habilitada, os administradores poderão rastrear eventos para detectar a criação mal-intencionada, acidental e autorizada de contas de grupo de aplicativos.  
  
##### <a name="other-account-management-events"></a>Outros eventos de gerenciamento de conta  
Essa subcategoria relata outros eventos de gerenciamento de conta.  
  
#### <a name="detailed-process-tracking"></a>Controle de processo detalhado  
  
##### <a name="process-creation"></a>Criação de processo  
Essa subcategoria relata a criação de um processo e o nome do usuário ou programa que o criou.  
  
##### <a name="process-termination"></a>Término do processo  
Essa subcategoria relata quando um processo é encerrado.  
  
##### <a name="dpapi-activity"></a>Atividade de DPAPI  
Essa subcategoria relata as chamadas criptografar ou descriptografar na DPAPI (interface de programação de aplicativo de proteção de dados). A DPAPI é usada para proteger informações secretas como senha armazenada e informações de chave.  
  
##### <a name="rpc-events"></a>Eventos RPC  
Esta subcategoria relata eventos de conexão de chamada de procedimento remoto (RPC).  
  
#### <a name="directory-service-access"></a>Acesso ao serviço de diretório  
  
##### <a name="directory-service-access"></a>Acesso ao serviço de diretório  
Essa subcategoria relata quando um objeto de AD DS é acessado. Somente objetos com SACLs configuradas causam a geração de eventos de auditoria e somente quando eles são acessados de uma maneira que corresponda às entradas da SACL. Esses eventos são semelhantes aos eventos de acesso do serviço de diretório em versões anteriores do Windows Server. Essa subcategoria se aplica somente aos controladores de domínio.  
  
##### <a name="directory-service-changes"></a>Alterações no serviço de diretório  
Essa subcategoria relata alterações em objetos no AD DS. Os tipos de alterações relatadas são as operações criar, modificar, mover e restaurar que são executadas em um objeto. Auditoria de alteração do serviço de diretório, quando apropriado, indica os valores novos e antigos das propriedades alteradas dos objetos que foram alterados. Somente objetos com SACLs causam a geração de eventos de auditoria e somente quando eles são acessados de uma maneira que corresponde às suas entradas de SACL. Alguns objetos e propriedades não fazem eventos de auditoria serem gerados por causa de configurações na classe de objeto no esquema. Essa subcategoria se aplica somente aos controladores de domínio.  
  
##### <a name="directory-service-replication"></a>Replicação do Serviço de Diretório  
Essa subcategoria relata quando a replicação entre dois controladores de domínio começa e termina.  
  
##### <a name="detailed-directory-service-replication"></a>Replicação detalhada do serviço de diretório  
Essa subcategoria relata informações detalhadas sobre as informações replicadas entre controladores de domínio. Esses eventos podem ser muito altos no volume.  
  
#### <a name="logonlogoff"></a>Logon/logoff  
  
##### <a name="logon"></a>Logon  
Essa subcategoria relata quando um usuário tenta fazer logon no sistema. Esses eventos ocorrem no computador acessado. Para logons interativos, a geração desses eventos ocorre no computador que está conectado ao. Se ocorrer um logon de rede para acessar um compartilhamento, esses eventos geram no computador que hospeda o recurso acessado. Se essa configuração estiver definida como **sem auditoria**, é difícil ou impossível determinar qual usuário acessou ou tentou acessar computadores da organização.  
  
##### <a name="network-policy-server"></a>Servidor de Políticas de Rede  
Essa subcategoria relata eventos gerados pelas solicitações de acesso do usuário RADIUS (IAS) e NAP (proteção de acesso à rede). Essas solicitações podem ser **conceder**, **negar**, **descartar**, **colocar em quarentena**, **Bloquear**e **desbloquear**. A auditoria dessa configuração resultará em um volume médio ou alto de registros em servidores NPS e IAS.  
  
##### <a name="ipsec-main-mode"></a>Modo principal do IPsec  
Essa subcategoria relata os resultados do protocolo protocolo IKE (IKE) e do protocolo Authenticated IP (AuthIP) durante as negociações do modo principal.  
  
##### <a name="ipsec-extended-mode"></a>Modo estendido do IPsec  
Essa subcategoria relata os resultados de AuthIP durante negociações de modo estendido.  
  
##### <a name="other-logonlogoff-events"></a>Outros eventos de logon/logoff  
Essa subcategoria relata outros eventos relacionados ao logon e ao logoff, como Serviços de Área de Trabalho Remota desconexões de sessão e reconexão, usando executar como para executar processos em uma conta diferente, bloqueando e desbloqueando uma estação de trabalho.  
  
##### <a name="logoff"></a>Logoff  
Essa subcategoria relata quando um usuário faz logoff do sistema. Esses eventos ocorrem no computador acessado. Para logons interativos, a geração desses eventos ocorre no computador que está conectado ao. Se ocorrer um logon de rede para acessar um compartilhamento, esses eventos geram no computador que hospeda o recurso acessado. Se essa configuração estiver definida como **sem auditoria**, é difícil ou impossível determinar qual usuário acessou ou tentou acessar computadores da organização.  
  
##### <a name="account-lockout"></a>Bloqueio de conta  
Essa subcategoria relata quando a conta de um usuário é bloqueada como resultado de muitas tentativas de logon com falha.  
  
##### <a name="ipsec-quick-mode"></a>Modo rápido do IPsec  
Essa subcategoria relata os resultados do protocolo IKE e do AuthIP durante negociações de modo rápido.  
  
##### <a name="special-logon"></a>Logon especial  
Essa subcategoria informa quando um logon especial é usado. Um logon especial é um logon que tem privilégios equivalentes de administrador e pode ser usado para elevar um processo a um nível mais alto.  
  
#### <a name="policy-change"></a>Alteração de Política  
  
##### <a name="audit-policy-change"></a>Alterar política de auditoria  
Essa subcategoria relata alterações na política de auditoria, incluindo alterações de SACL.  
  
##### <a name="authentication-policy-change"></a>Alteração da política de autenticação  
Essa subcategoria relata alterações na política de autenticação.  
  
##### <a name="authorization-policy-change"></a>Alteração da política de autorização  
Essa subcategoria relata alterações na política de autorização, incluindo alterações de DACL (permissões).  
  
##### <a name="mpssvc-rule-level-policy-change"></a>Alteração de política de nível de regra MPSSVC  
Essa subcategoria relata as alterações nas regras de política usadas pelo serviço de proteção da Microsoft (MPSSVC. exe). Esse serviço é usado pelo firewall do Windows.  
  
##### <a name="filtering-platform-policy-change"></a>Filtrando alteração de política da plataforma  
Essa subcategoria relata a adição e a remoção de objetos do WFP, incluindo filtros de inicialização. Esses eventos podem ser muito altos no volume.  
  
##### <a name="other-policy-change-events"></a>Outros eventos de alteração de política  
Essa subcategoria relata outros tipos de alterações de política de segurança, como a configuração do Trusted Platform Module (TPM) ou provedores criptográficos.  
  
#### <a name="privilege-use"></a>Uso de privilégios  
  
##### <a name="sensitive-privilege-use"></a>Uso de privilégios confidenciais  
Essa subcategoria relata quando uma conta de usuário ou serviço usa um privilégio confidencial. Um privilégio confidencial inclui os seguintes direitos de usuário: atuar como parte do sistema operacional, fazer backup de arquivos e diretórios, criar um objeto de token, depurar programas, habilitar contas de computador e de usuário para serem confiáveis para delegação, gerar auditorias de segurança, representar um cliente após a autenticação, carregar e descarregar drivers de dispositivo, gerenciar logs de auditoria e segurança, modificar os valores de ambiente de firmware, substituir um token de nível de processo, restaurar arquivos e diretórios e apropriar-se de arquivos ou outros objetos. A auditoria dessa subcategoria criará um alto volume de eventos.  
  
##### <a name="nonsensitive-privilege-use"></a>Uso de privilégios não confidenciais  
Essa subcategoria relata quando uma conta de usuário ou serviço usa um privilégio não confidencial. Um privilégio não confidencial inclui os seguintes direitos de usuário: acessar o Gerenciador de credenciais como um chamador confiável, acessar este computador da rede, adicionar estações de trabalho ao domínio, ajustar cotas de memória para um processo, permitir logon localmente, permitir logon remotamente Serviços de área de trabalho, ignorar a verificação completa, alterar a hora do sistema, criar um arquivo de paginação, criar objetos globais, criar objetos compartilhados permanentemente, criar links simbólicos, negar acesso a este computador da rede, negar logon como um trabalho em lotes, negar logon como um serviço, Negar logon localmente, negar logon por meio de Serviços de Área de Trabalho Remota, forçar o desligamento de um sistema remoto, aumentar um conjunto de trabalho de processo, aumentar a prioridade de agendamento, bloquear páginas na memória, fazer logon como um trabalho em lotes, fazer logon como um serviço, modificar um rótulo de objeto, executar volume tarefas de manutenção, processo único de perfil, desempenho do sistema de perfil, remover computador da estação de encaixe, desligar o sistema e sincronizar dados do serviço de diretório. A auditoria dessa subcategoria criará um grande volume de eventos.  
  
##### <a name="other-privilege-use-events"></a>Outros eventos de uso de privilégio  
Essa configuração de política de segurança não está sendo usada no momento.  
  
#### <a name="object-access"></a>Acesso a objeto  
  
##### <a name="file-system"></a>Sistema de arquivos  
Essa subcategoria relata quando os objetos do sistema de arquivos são acessados. Somente objetos do sistema de arquivos com SACLs causam a geração de eventos de auditoria e somente quando eles são acessados de maneira correspondente às entradas da SACL. Por si só, essa configuração de política não causará a auditoria de nenhum evento. Ele determina se é necessário auditar o evento de um usuário que acessa um objeto do sistema de arquivos que tem uma SACL (lista de controle de acesso) do sistema especificada, permitindo que a auditoria ocorra com eficiência.  
  
Se a configuração de acesso ao objeto de auditoria estiver configurada como **êxito**, uma entrada de auditoria será gerada cada vez que um usuário acessar com êxito um objeto com uma SACL especificada. Se essa configuração de política estiver configurada como **falha**, uma entrada de auditoria será gerada cada vez que um usuário falhar em uma tentativa de acessar um objeto com uma SACL especificada.  
  
##### <a name="registry"></a>Registro  
Essa subcategoria relata quando os objetos do registro são acessados. Somente objetos do registro com SACLs causam a geração de eventos de auditoria e somente quando eles são acessados de maneira correspondente às entradas da SACL. Por si só, essa configuração de política não causará a auditoria de nenhum evento.  
  
##### <a name="kernel-object"></a>Objeto kernel  
Essa subcategoria relata quando objetos kernel, como processos e mutexes, são acessados. Somente objetos kernel com SACLs causam a geração de eventos de auditoria e somente quando eles são acessados de maneira correspondente às entradas da SACL. Normalmente, os objetos kernel só recebem SACLs se as opções de auditoria AuditBaseObjects ou AuditBaseDirectories estiverem habilitadas.  
  
##### <a name="sam"></a>SAM  
Essa subcategoria relata quando os objetos de banco de dados de autenticação SAM (Gerenciador de contas de segurança) locais são acessados.  
  
##### <a name="certification-services"></a>Serviços de certificação  
Essa subcategoria relata quando as operações de serviços de certificação são executadas.  
  
##### <a name="application-generated"></a>Gerado pelo aplicativo  
Essa subcategoria relata quando os aplicativos tentam gerar eventos de auditoria usando as APIs (interfaces de programação de aplicativo) de auditoria do Windows.  
  
##### <a name="handle-manipulation"></a>Manipulação de identificador  
Essa subcategoria relata quando um identificador para um objeto é aberto ou fechado. Somente objetos com SACLs fazem com que esses eventos sejam gerados e somente se a operação de identificador tentada corresponder às entradas da SACL. Os eventos de manipulação de identificadores são gerados somente para tipos de objeto em que a subcategoria de acesso a objeto correspondente está habilitada (por exemplo, sistema de arquivos ou registro).  
  
##### <a name="file-share"></a>Comp. de Arquivos  
Essa subcategoria relata quando um compartilhamento de arquivos é acessado. Por si só, essa configuração de política não causará a auditoria de nenhum evento. Ele determina se deve-se auditar o evento de um usuário que acessa um objeto de compartilhamento de arquivo que tem uma SACL (lista de controle de acesso) do sistema especificada, permitindo que a auditoria ocorra com eficiência.  
  
##### <a name="filtering-platform-packet-drop"></a>Remoção de pacote de plataforma de filtragem  
Essa subcategoria relata quando os pacotes são descartados pela WFP (plataforma de filtragem do Windows). Esses eventos podem ser muito altos no volume.  
  
##### <a name="filtering-platform-connection"></a>Conexão da plataforma de filtragem  
Essa subcategoria relata quando as conexões são permitidas ou bloqueadas pela WFP. Esses eventos podem ser de alto volume.  
  
##### <a name="other-object-access-events"></a>Outros eventos de acesso de objeto  
Essa subcategoria relata outros eventos relacionados ao acesso a objetos, como Agendador de Tarefas trabalhos e objetos COM+.  
  
#### <a name="system"></a>Sistema  
  
##### <a name="security-state-change"></a>Alteração do estado de segurança  
Essa subcategoria relata as alterações no estado de segurança do sistema, como quando o subsistema de segurança é iniciado e interrompido.  
  
##### <a name="security-system-extension"></a>Extensão do sistema de segurança  
Essa subcategoria relata o carregamento do código de extensão, como pacotes de autenticação pelo subsistema de segurança.  
  
##### <a name="system-integrity"></a>Integridade do sistema  
Essa subcategoria relata violações de integridade do subsistema de segurança.  
  
Driver IPsec  
  
Essa subcategoria relata as atividades do driver de IPsec (Internet Protocol Security).  
  
##### <a name="other-system-events"></a>Outros eventos do sistema  
Essa subcategoria relata outros eventos do sistema.  
  
Para obter mais informações sobre as descrições de subcategorias, consulte a [ferramenta Microsoft Security Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx).  
  
Cada organização deve revisar as categorias e subcategorias cobertas anteriormente e habilitar as que melhor se ajustam ao seu ambiente. As alterações na política de auditoria devem ser sempre testadas antes da implantação em um ambiente de produção.  
  
## <a name="configuring-windows-audit-policy"></a>Configurando a política de auditoria do Windows

A política de auditoria do Windows pode ser definida usando políticas de grupo, Auditpol. exe, APIs ou edições do registro. Os métodos recomendados para configurar a diretiva de auditoria para a maioria das empresas são Política de Grupo ou Auditpol. exe. Definir a diretiva de auditoria de um sistema requer permissões de conta de nível de administrador ou permissões delegadas apropriadas.  
  
> [!NOTE]  
> O privilégio **gerenciar auditoria e log de segurança** deve ser fornecido a entidades de segurança (os administradores têm isso por padrão) para permitir a modificação de opções de auditoria de acesso a objetos de recursos individuais, como arquivos, Active Directory objetos e chaves do registro.  
  
### <a name="setting-windows-audit-policy-by-using-group-policy"></a>Definindo a política de auditoria do Windows usando Política de Grupo

Para definir a política de auditoria usando políticas de grupo, configure as categorias de auditoria apropriadas localizadas na **política do computador \** \ Configuração do Policies\Audit (consulte a captura de tela a seguir para obter um exemplo do editor de política de grupo local (gpedit. msc)). Cada categoria de política de auditoria pode ser habilitada para eventos de **êxito**, **falha**ou **êxito** e falha.  
  
![monitorando o AD](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_6.gif)  
  
A política de auditoria avançada pode ser definida usando Active Directory ou políticas de grupo local. Para definir a política de auditoria avançada, configure as subcategorias apropriadas localizadas em **computador \** \ Configurações de auditoria de Settings\Advanced (consulte a captura de tela a seguir para obter um exemplo do editor de política de grupo local (gpedit. msc)). Cada subcategoria de diretiva de auditoria pode ser habilitada para eventos de **êxito**, **falha**ou **êxito** e **falha** .  
  
![monitorando o AD](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_7.gif)  
  
### <a name="setting-windows-audit-policy-using-auditpolexe"></a>Definindo a política de auditoria do Windows usando Auditpol. exe

O Auditpol. exe (para configurar a política de auditoria do Windows) foi introduzido no Windows Server 2008 e no Windows Vista. Inicialmente, somente o Auditpol. exe poderia ser usado para definir a diretiva de auditoria avançada, mas Política de Grupo pode ser usado no Windows Server 2012, no Windows Server 2008 R2 ou no Windows Server 2008, Windows 8 e Windows 7.  
  
Auditpol. exe é um utilitário de linha de comando. A sintaxe é assim:  
  
`auditpol /set /<Category|Subcategory>:<audit category> /<success|failure:> /<enable|disable>`
  
Exemplos de sintaxe Auditpol. exe:  
  
`auditpol /set /subcategory:"user account management" /success:enable /failure:enable`
  
`auditpol /set /subcategory:"logon" /success:enable /failure:enable`
  
`auditpol /set /subcategory:"IPSEC Main Mode" /failure:enable`
  
> [!NOTE]  
> Auditpol. exe define a política de auditoria avançada localmente. Se a política local estiver em conflito com Active Directory ou Política de Grupo local, as configurações de Política de Grupo geralmente prevalecem sobre as configurações de Auditpol. exe. Quando houver conflitos de diretiva local ou de vários grupos, somente uma política prevalecerá (ou seja, substituirá). As políticas de auditoria não serão mescladas.  
  
#### <a name="scripting-auditpol"></a>Script de Auditpol

A Microsoft fornece um [script de exemplo](https://support.microsoft.com/kb/921469) para os administradores que desejam definir a diretiva de auditoria avançada usando um script em vez de digitar manualmente cada comando Auditpol. exe.  
  
**Observação** O Política de Grupo nem sempre relata com precisão o status de todas as políticas de auditoria habilitadas, enquanto o Auditpol. exe faz isso. Consulte [obtendo a política de auditoria efetiva no Windows 7 e no windows 2008 R2](https://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx) para obter mais detalhes.  
  
#### <a name="other-auditpol-commands"></a>Outros comandos Auditpol

Auditpol. exe pode ser usado para salvar e restaurar uma política de auditoria local e para exibir outros comandos relacionados à auditoria. Aqui estão os outros comandos **Auditpol** .  
  
`auditpol /clear`-usado para limpar e redefinir políticas de auditoria local  
  
`auditpol /backup /file:<filename>`-usado para fazer backup de uma política de auditoria local atual em um arquivo binário  
  
`auditpol /restore /file:<filename>`-usado para importar um arquivo de política de auditoria salvo anteriormente para uma política de auditoria local  
  
`auditpol /<get/set> /option:<CrashOnAuditFail> /<enable/disable>`-se essa configuração de diretiva de auditoria estiver habilitada, ela fará com que o sistema pare imediatamente (com a mensagem STOP: C0000244 {Audit Failed}) se uma auditoria de segurança não puder ser registrada por algum motivo. Normalmente, um evento não é registrado quando o log de auditoria de segurança está cheio e o método de retenção especificado para o log de segurança **não substitui eventos** nem **substitui eventos por dias**. Normalmente, ele só é habilitado por ambientes que precisam de maior garantia de que o log de segurança está registrando em log. Se habilitada, os administradores devem observar com atenção o tamanho do log de segurança e girar os logs conforme necessário. Ele também pode ser definido com Política de Grupo modificando a opção de segurança **auditoria: desligar o sistema imediatamente se não for possível registrar auditorias de segurança** (padrão = desabilitado).  
  
`auditpol /<get/set> /option:<AuditBaseObjects> /<enable/disable>` – essa configuração de política de auditoria determina se o acesso de objetos do sistema global deve ser auditado. Se essa política estiver habilitada, ela fará com que os objetos do sistema, como mutexes, eventos, semáforos e dispositivos DOS, sejam criados com uma SACL (lista de controle de acesso) do sistema padrão. A maioria dos administradores consideram a auditoria de objetos do sistema global como "ruidosa", e eles só serão habilitados se suspeitarem de hackers mal-intencionados. Somente objetos nomeados recebem uma SACL. Se a política de auditoria de acesso a objetos de auditoria (ou a subcategoria de auditoria de objeto de kernel) também estiver habilitada, o acesso a esses objetos do sistema será auditado. Ao definir essa configuração de segurança, as alterações não terão efeito até que você reinicie o Windows. Essa política também pode ser definida com Política de Grupo modificando a opção de segurança auditar o acesso de objetos do sistema global (padrão = desabilitado).  
  
`auditpol /<get/set> /option:<AuditBaseDirectories> /<enable/disable>`-essa configuração de diretiva de auditoria Especifica que os objetos de kernel nomeados (como mutexes e semáforos) devem receber SACLs quando forem criados. AuditBaseDirectories afeta objetos de contêiner enquanto AuditBaseObjects afeta objetos que não podem conter outros objetos.  
  
`auditpol /<get/set> /option:<FullPrivilegeAuditing> /<enable/disable>` – essa configuração de política de auditoria Especifica se o cliente gera um evento quando um ou mais desses privilégios são atribuídos a um token de segurança do usuário: AssignPrimaryTokenPrivilege, AuditPrivilege, BackupPrivilege, CreateTokenPrivilege, DebugPrivilege, EnableDelegationPrivilege, ImpersonatePrivilege, LoadDriverPrivilege, RestorePrivilege, SecurityPrivilege, SystemEnvironmentPrivilege, TakeOwnershipPrivilege e TcbPrivilege. Se essa opção não estiver habilitada (padrão = desabilitada), os privilégios de BackupPrivilege e RestorePrivilege não serão registrados. Habilitar essa opção pode tornar o log de segurança extremamente ruidosa (às vezes, centenas de eventos por segundo) durante uma operação de backup. Essa política também pode ser definida com Política de Grupo modificando a opção de segurança **auditoria: auditar o uso do privilégio de backup e restauração**.  
  
> [!NOTE]  
> Algumas informações fornecidas aqui foram tiradas do [tipo de opção de auditoria](https://msdn.microsoft.com/library/dd973862(prot.20).aspx) da Microsoft e da ferramenta Microsoft SCM.  
  
## <a name="enforcing-traditional-auditing-or-advanced-auditing"></a>Impondo auditoria tradicional ou auditoria avançada

No Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows 8, Windows 7 e Windows Vista, os administradores podem optar por habilitar as nove categorias tradicionais ou usar as subcategorias. É uma opção binária que deve ser feita em cada sistema Windows. As categorias principais podem ser habilitadas ou o subcategoriesit não pode ser ambos.  
  
Para impedir que a política de categoria tradicional herdada substitua as subcategorias de política de auditoria, você deve habilitar as configurações de política de **política de auditoria forçada (Windows Vista ou posterior) para substituir as configurações de categoria** de política de auditoria localizadas em **computador \** \ \ instalar opções.  
  
Recomendamos que as subcategorias sejam habilitadas e configuradas em vez das nove categorias principais. Isso requer que uma configuração de Política de Grupo seja habilitada (para permitir que as subcategorias substituam as categorias de auditoria) junto com a configuração de diferentes subcategorias que dão suporte a políticas de auditoria.  
  
As subcategorias de auditoria podem ser configuradas usando vários métodos, incluindo Política de Grupo e o programa de linha de comando, Auditpol. exe.  
  
## <a name="next-steps"></a>Próximas etapas
  
* [Auditoria de segurança avançada no Windows 7 e no Windows Server 2008 R2](https://social.technet.microsoft.com/wiki/contents/articles/advanced-security-auditing-in-windows-7-and-windows-server-2008-r2.aspx)  
  
* [Auditoria e conformidade no Windows Server 2008](https://technet.microsoft.com/magazine/2008.03.auditing.aspx)  
  
* [Como usar Política de Grupo para definir configurações de auditoria de segurança detalhadas para computadores baseados no Windows Vista e no Windows Server 2008 em um domínio do Windows Server 2008, em um domínio do Windows Server 2003 ou em um domínio do Windows 2000](https://support.microsoft.com/kb/921469)  
  
* [Guia passo a passo da política de auditoria de segurança avançada](https://technet.microsoft.com/library/dd408940(WS.10).aspx)  
