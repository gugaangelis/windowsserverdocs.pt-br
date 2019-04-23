---
ms.assetid: a7ef2fba-b05c-4be2-93b2-b9456244c3ad
title: Monitorar o Active Directory em busca de sinais de comprometimento
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 40d0d06f8d6d25c2c1dbf4662d3296a996d22055
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882937"
---
# <a name="monitoring-active-directory-for-signs-of-compromise"></a>Monitorar o Active Directory em busca de sinais de comprometimento

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

*Lei número cinco: Vigilância eterna é o preço da segurança.* - [10 leis imutáveis da segurança de administração](https://technet.microsoft.com/library/cc722488.aspx)  
  
Um sólido sistema de monitoramento de log de eventos é uma parte essencial de qualquer design do Active Directory seguro. Muitos comprometimentos de segurança do computador foi descobertos no início de eventos se as vítimas imposta apropriado de log de eventos de monitoramento e alertas. Relatórios independentes há muito oferecem suporte a essa conclusão. Por exemplo, o [relatório de violação de dados de Verizon 2009](http://www.verizonbusiness.com/resources/security/reports/2009_databreach_rp.pdf) estados:  
  
"O ineffectiveness aparente de análise de log e monitoramento de eventos continua a ser um pouco de um enigma. É a oportunidade para detecção de lá; investigadores observado que 66% das vítimas tinha evidências suficientes disponíveis dentro de seus logs para descobrir a violação tinham que eles foram mais diligentes na análise de tais recursos."  
  
Essa falta de monitoramento de logs de eventos do Active Directory permanece uma fraqueza consistente nos planos de defesa de segurança de muitas empresas. O [relatório de violação de dados da Verizon 2012](http://www.verizonbusiness.com/resources/reports/rp_data-breach-investigations-report-2012_en_xg.pdf) encontrado que, mesmo que 85% das violações de realizar várias semanas para ser observado, 84 por cento de vítimas tinha evidência da violação em seus logs de eventos.  
  
## <a name="windows-audit-policy"></a>Política de auditoria do Windows

A seguir estão os links para o blog de suporte do Microsoft enterprise oficial. O conteúdo desses blogs fornece conselhos, diretrizes e recomendações sobre a auditoria que irá ajudá-lo a aprimorar a segurança de infra-estrutura do Active Directory e são um recurso valioso ao criar uma política de auditoria.  
  
* [Auditoria de acesso a objeto global é a mágica](http://blogs.technet.com/b/askds/archive/2011/03/10/global-object-access-auditing-is-magic.aspx) -descreve um mecanismo de controle chamado política de configuração de auditoria avançada que foi adicionado ao Windows 7 e Windows Server 2008 R2 que permite que você definir quais tipos de dados que você quisesse auditar facilmente e não malabarismos scripts e auditpol.exe.  
* [Introduzindo alterações de auditoria no Windows 2008](http://blogs.technet.com/b/askds/archive/2007/10/19/introducing-auditing-changes-in-windows-2008.aspx) -apresenta as alterações de auditoria feitas no Windows Server 2008.  
* [Fria truques de auditoria no Vista e no 2008](http://blogs.technet.com/b/askds/archive/2007/11/16/cool-auditing-tricks-in-vista-and-2008.aspx) -explica os interessantes recursos de auditoria do Windows Vista e Windows Server 2008 que podem ser usados para solução de problemas ou vendo o que está acontecendo em seu ambiente.  
* [Loja de conveniência para auditoria no Windows Server 2008 e Windows Vista](http://blogs.technet.com/b/askds/archive/2008/03/27/one-stop-shop-for-auditing-in-windows-server-2008-and-windows-vista.aspx) -contém uma compilação de auditoria de recursos e as informações contidas no Windows Server 2008 e Windows Vista.  
  
Os links a seguir fornecem informações sobre as melhorias de auditoria no Windows 8 e Windows Server 2012 do Windows e informações sobre o AD DS a auditoria no Windows Server 2008.  
  
* [O que há de novo na auditoria de segurança](https://technet.microsoft.com/library/hh849638.aspx) -fornece uma visão geral dos novos recursos no Windows 8 e Windows Server 2012 de auditoria de segurança.  
* [Guia passo a passo de auditoria do AD DS](https://technet.microsoft.com/library/a9c25483-89e2-4202-881c-ea8e02b4b2a5.aspx) -descreve o novo recurso de auditoria de serviços de domínio Active Directory (AD DS) no Windows Server 2008. Ele também fornece procedimentos para implementar esse novo recurso.  
  
### <a name="windows-audit-categories"></a>Categorias de auditoria do Windows

Antes do Windows Vista e Windows Server 2008, Windows tinham apenas nove categorias de política de auditoria de log de eventos:  
  
* Eventos de Logon de conta  
* Gerenciamento de contas  
* Acesso ao serviço de diretório  
* Eventos de logon  
* Acesso a objeto  
* Alteração de Política  
* Uso de privilégios  
* Acompanhamento de processo  
* Eventos do sistema  
  
Essas nove categorias de auditoria tradicional compõem uma política de auditoria. Cada categoria de política de auditoria pode ser habilitada para êxito, falha ou êxito e falha eventos. Suas descrições são incluídas na próxima seção.  
  
#### <a name="audit-policy-category-descriptions"></a>Descrições de categoria de política de auditoria  
As categorias de política de auditoria permitem os seguintes tipos de mensagem de log de eventos.  
  
##### <a name="audit-account-logon-events"></a>Eventos de Logon de conta de auditoria  
Relata cada instância de uma entidade de segurança (por exemplo, usuário, computador ou conta de serviço) que está fazendo logon ou logoff de um computador em que o outro computador é usado para validar a conta. Eventos de logon de conta são gerados quando uma conta de entidade de segurança de domínio é autenticada em um controlador de domínio. Autenticação de um usuário local em um computador local gera um evento de logon que é registrado no log de segurança local. Nenhum evento de logoff de conta é registrado.  
  
Esta categoria gera muita "ruído" porque o Windows está constantemente com contas de logon e logoff de computadores locais e remotos durante o curso normal de negócios. Ainda assim, qualquer plano de segurança deve incluir o sucesso e falha dessa categoria de auditoria.  
  
##### <a name="audit-account-management"></a>Gerenciamento de conta de auditoria  
Essa configuração de auditoria determina se deve controlar o gerenciamento de usuários e grupos. Por exemplo, usuários e grupos devem ser controlados quando um usuário ou conta de computador, um grupo de segurança ou um grupo de distribuição é criado, alterado ou excluído; Quando uma conta de usuário ou computador for renomeada, desabilitada ou habilitada; ou, quando uma senha de usuário ou computador é alterada. Um evento pode ser gerado para usuários ou grupos que são adicionados ou removidos de outros grupos.  
  
##### <a name="audit-directory-service-access"></a>Auditoria do acesso ao serviço de diretório  

Essa configuração de política determina se a auditoria de acesso à entidade de segurança a um objeto do Active Directory que tem sua própria lista de controle de acesso especificada do sistema (SACL). Em geral, esta categoria só deve ser habilitada nos controladores de domínio. Quando habilitada, essa configuração gera muita "ruído".  
  
##### <a name="audit-logon-events"></a>Eventos de Logon de auditoria  
Eventos de logon são gerados quando uma entidade de segurança local é autenticada em um computador local. Eventos de logon registra logons de domínio que ocorrem no computador local. Eventos de logoff da conta não são gerados. Quando habilitada, os eventos de Logon gerar muita "ruído", mas deve ser habilitados por padrão em qualquer plano de auditoria de segurança.  
  
##### <a name="audit-object-access"></a>Auditoria de acesso a objeto  
Acesso a objetos pode gerar eventos quando são acessados objetos de subsequentemente definidos com a auditoria ativada (por exemplo, Opened, leitura, renomeado, excluído ou fechado). Depois que a categoria de auditoria principal estiver habilitada, o administrador deve definir individualmente quais objetos serão auditoria habilitada. Muitos objetos de sistema do Windows vêm com a auditoria habilitada, portanto, habilitar essa categoria geralmente começará a gerar eventos antes que o administrador definiu qualquer.  
  
Esta categoria é muito "barulhento" e irá gerar eventos de cinco a dez para cada acesso ao objeto. Pode ser difícil para os administradores de novos na auditoria de objeto obter informações úteis. Ele deve ser habilitado apenas quando necessário.  
  
##### <a name="auditing-policy-change"></a>Alteração de política de auditoria  
Essa configuração de política determina se a auditoria de todas as instâncias de uma alteração em diretivas de atribuição de direitos de usuário, políticas de Firewall do Windows, diretivas de confiança ou as alterações na diretiva de auditoria. Esta categoria deve ser habilitada em todos os computadores. Ele gera ruído muito pouco.  
  
##### <a name="audit-privilege-use"></a>Uso de privilégios de auditoria  

Existem dezenas de direitos de usuário e permissões no Windows (por exemplo, faça Logon como um trabalho em lotes e atuar como parte do sistema operacional). Essa configuração de política determina se a auditoria de cada instância de uma entidade de segurança ao exercer um direito de usuário ou privilégio. Permitindo que os resultados em uma grande quantidade de "ruído", mas essa categoria pode ser úteis no acompanhamento de contas de entidade de segurança usando privilégios elevados.  
  
##### <a name="audit-process-tracking"></a>Acompanhamento de processos de auditoria  
Essa configuração de política determina se a auditoria de processo detalhado, informações de controle de eventos como ativação de programa, saída do processo, duplicação de identificador e acesso indireto a objetos. Ele é útil para acompanhar usuários mal-intencionados e os programas que usarem.  
  
Habilitar o rastreamento do processo de auditoria gera um grande número de eventos, isso normalmente é definido como **sem auditoria**. No entanto, essa configuração pode fornecer um grande benefício durante uma resposta a incidentes do log detalhado de processos iniciados e a hora em que eles foram iniciados. Controladores de domínio e outros servidores de infraestrutura de função simples, nessa categoria pode ser ativada com segurança por todo o tempo. Servidores de função única não geram muito tráfego de controle durante o curso normal de suas tarefas de processo. Como tal, pode ser habilitados para capturar eventos não autorizados se elas ocorrerem.  
  
##### <a name="system-events-audit"></a>Auditoria de eventos do sistema  

Eventos do sistema é quase uma genérica categoria catch-all, registrando diversos eventos que afetam o computador, a segurança do sistema ou o log de segurança. Ele inclui eventos para desligamentos de computador e reinicializações, falhas de energia, as alterações de horário do sistema, inicializações de pacote de autenticação, clearings de log de auditoria, questões e um host de outros eventos gerais. Em geral, habilitar essa categoria de auditoria gera muita "ruído", mas ela gera eventos suficientes muito útil que é difícil nunca recomendaria não habilitá-la.  
  
#### <a name="advanced-audit-policies"></a>Políticas de auditoria avançada

Começando com o Windows Vista e Windows Server 2008, a Microsoft melhorou a maneira como as seleções de categoria de log de eventos podem ser feitas criando subcategorias sob cada categoria de auditoria principal. As subcategorias permitem que seja muito mais granular do que era possível caso contrário, usando as principais categorias de auditoria. Usando subcategorias, você pode habilitar apenas partes de uma determinada categoria principal e ignorar a geração de eventos para os quais você não tem nenhum uso. Cada subcategoria de política de auditoria pode ser habilitada para eventos de Êxito, Falha ou Êxito e Falha.  
  
Para listar todas as subcategorias de auditoria disponíveis, examine o contêiner de política de auditoria avançada em um objeto de diretiva de grupo, ou digite o seguinte no prompt de comando em qualquer computador executando o Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008, Windows 8 Windows 7 ou Windows Vista:  
  
`auditpol /list /subcategory:\*`
  
Para obter uma lista de subcategorias de auditoria configuradas no momento em um computador executando o Windows Server 2012, Windows Server 2008 R2 ou Windows 2008, digite o seguinte:  
  
`auditpol /get /category:\*`
  
Captura de tela a seguir mostra um exemplo de auditpol.exe listando a atual política de auditoria.  
  
![monitoramento do AD](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_5.gif)  
  
> [!NOTE]  
> Diretiva de grupo não relata sempre com precisão o status de todas as políticas de auditoria habilitadas, enquanto o auditpol.exe faz. Ver [obter a política efetiva de auditoria no Windows 7 e no 2008 R2](http://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx) para obter mais detalhes.  
  
Cada categoria principal tem várias subcategorias. Abaixo está uma lista de categorias, suas subcategorias e uma descrição de suas funções.  
  
### <a name="auditing-subcategories-descriptions"></a>Descrições de subcategorias de auditoria  
Subcategorias de política de auditoria habilitar os seguintes tipos de mensagem de log de eventos:  
  
#### <a name="account-logon"></a>Logon de conta  
  
##### <a name="credential-validation"></a>Validação de credenciais  
Essa subcategoria relata os resultados de testes de validação nas credenciais enviadas para uma solicitação de logon de conta de usuário. Esses eventos ocorrem no computador autoritativo para as credenciais. Contas de domínio, o controlador de domínio é autoritativo, enquanto para contas locais, o computador local é autoritativo.  
  
Em ambientes de domínio, a maioria dos eventos de logon de conta é registradas no log de segurança dos controladores de domínio autorizados para as contas de domínio. No entanto, esses eventos podem ocorrer em outros computadores na organização quando contas locais são usadas para fazer logon.  
  
##### <a name="kerberos-service-ticket-operations"></a>Auditoria de Operações do Tíquete de Serviço Kerberos  
Essa subcategoria relata eventos gerados pelos processos de solicitação de tíquete do Kerberos no controlador de domínio que está autorizado para a conta de domínio.  
  
##### <a name="kerberos-authentication-service"></a>Serviço de autenticação Kerberos  
Essa subcategoria relata eventos gerados pelo serviço de autenticação Kerberos. Esses eventos ocorrem no computador autoritativo para as credenciais.  
  
##### <a name="other-account-logon-events"></a>Outros eventos de Logon de Conta  
Essa subcategoria relata os eventos que ocorrem em resposta às credenciais enviado para uma solicitação de logon de conta de usuário que não estão relacionados a validação de credenciais ou tíquetes Kerberos. Esses eventos ocorrem no computador autoritativo para as credenciais. Contas de domínio, o controlador de domínio é autoritativo, enquanto para contas locais, o computador local é autoritativo.  
  
Em ambientes de domínio, a maioria dos eventos de logon de conta são registrados no log de segurança dos controladores de domínio autorizados para as contas de domínio. No entanto, esses eventos podem ocorrer em outros computadores na organização quando contas locais são usadas para fazer logon. Entre os exemplos podem estar os seguintes:  
  
* Desconexões de sessão dos serviços de área de trabalho remotas  
* Novas sessões de serviços de área de trabalho remota  
* Bloqueio e desbloqueio de uma estação de trabalho  
* Invocação de uma proteção de tela  
* Descarte de uma proteção de tela  
* A detecção de uma Kerberos reproduzir ataque, em que uma solicitação de Kerberos com informações idênticas é recebida duas vezes  
* Acesso a uma rede sem fio concedido a uma conta de usuário ou computador  
* Acesso a uma rede com fio 802.1x concedido a uma conta de usuário ou computador  
  
#### <a name="account-management"></a>Gerenciamento de contas  
  
##### <a name="user-account-management"></a>Gerenciamento de contas de usuário  
Essa subcategoria relata cada evento de gerenciamento de conta de usuário, como quando uma conta de usuário é criada, alterada ou excluída; uma conta de usuário é renomeada, desabilitada ou habilitada; ou uma senha é definida ou alterada. Se essa configuração de política de auditoria estiver habilitada, os administradores podem acompanhar eventos para detectar a criação de autorizados mal-intencionadas e acidental de contas de usuário.  
  
##### <a name="computer-account-management"></a>Gerenciamento de conta de computador  
Essa subcategoria relata cada evento de gerenciamento de conta de computador, como quando uma conta de computador é criada, alterada, excluída, renomeada, desabilitada ou habilitada.  
  
##### <a name="security-group-management"></a>Gerenciamento de grupo de segurança  
Essa subcategoria relata cada evento de gerenciamento de grupo de segurança, como quando um grupo de segurança é criado, alterado ou excluído ou quando um membro é adicionado ou removido de um grupo de segurança. Se essa configuração de política de auditoria estiver habilitada, os administradores podem acompanhar eventos para detectar a criação de autorizados mal-intencionadas e acidental de contas de grupo de segurança.  
  
##### <a name="distribution-group-management"></a>Gerenciamento de grupo de distribuição  
Essa subcategoria relata cada evento de gerenciamento de grupo de distribuição, como quando um grupo de distribuição é criado, alterado ou excluído, ou quando um membro é adicionado ou removido de um grupo de distribuição. Se essa configuração de política de auditoria estiver habilitada, os administradores podem acompanhar eventos para detectar a criação de autorizados mal-intencionadas e acidental de contas de grupo.  
  
##### <a name="application-group-management"></a>Gerenciamento de grupo de aplicativos  
Essa subcategoria relata cada evento de gerenciamento de grupo de aplicativos em um computador, como quando um grupo de aplicativos é criado, alterado ou excluído, ou quando um membro é adicionado ou removido de um grupo de aplicativo. Se essa configuração de política de auditoria estiver habilitada, os administradores podem acompanhar eventos para detectar a criação de autorizados mal-intencionadas e acidental de contas de grupo do aplicativo.  
  
##### <a name="other-account-management-events"></a>Outros eventos de gerenciamento de conta  
Essa subcategoria relata outros eventos de gerenciamento de conta.  
  
#### <a name="detailed-process-tracking"></a>Acompanhamento de processo detalhado  
  
##### <a name="process-creation"></a>Criação de processo  
Essa subcategoria relata a criação de um processo e o nome do usuário ou do programa que o criou.  
  
##### <a name="process-termination"></a>Encerramento do processo  
Essa subcategoria informa quando um processo é encerrado.  
  
##### <a name="dpapi-activity"></a>Atividade DPAPI  
Essa subcategoria relata criptografar ou descriptografar chama o data protection API (DPAPI). A DPAPI é usada para proteger informações secretas, como a senha armazenada e informações de chave.  
  
##### <a name="rpc-events"></a>Eventos de RPC  
Esse procedimento remoto de relatórios de subcategoria chamar eventos de conexão (RPC).  
  
#### <a name="directory-service-access"></a>Acesso ao serviço de diretório  
  
##### <a name="directory-service-access"></a>Acesso ao serviço de diretório  
Essa subcategoria relata quando um objeto do AD DS é acessado. Somente os objetos com as SACLs configurados fazer com que os eventos de auditoria a ser gerado e somente quando eles são acessados de uma maneira que coincide com as entradas da SACL. Esses eventos são semelhantes aos eventos de acesso do serviço de diretório em versões anteriores do Windows Server. Essa subcategoria se aplica apenas aos controladores de domínio.  
  
##### <a name="directory-service-changes"></a>Mudanças do serviço de diretório  
Essa subcategoria informa as alterações nos objetos no AD DS. Os tipos de alterações que são relatados são criar, modificar, mover e restaurar operações que são executadas em um objeto. Directory service auditoria de alteração, quando apropriado, indica os valores novos e antigos das propriedades alteradas dos objetos que foram alteradas. Somente os objetos com as SACLs fazer com que os eventos de auditoria a ser gerado e somente quando eles são acessados de uma maneira que corresponde a suas entradas da SACL. Alguns objetos e propriedades não fazem eventos de auditoria serem gerados por causa de configurações na classe de objeto no esquema. Essa subcategoria se aplica apenas aos controladores de domínio.  
  
##### <a name="directory-service-replication"></a>Replicação do serviço de diretório  
Essa subcategoria informa quando a replicação entre dois controladores de domínio começa e termina.  
  
##### <a name="detailed-directory-service-replication"></a>Replicação do serviço de diretório detalhadas  
Essa subcategoria relata informações detalhadas sobre as informações replicadas entre controladores de domínio. Esses eventos podem ser muito altos volume.  
  
#### <a name="logonlogoff"></a>Logon/logoff  
  
##### <a name="logon"></a>Logon  
Essa subcategoria relata quando um usuário tenta fazer logon no sistema. Esses eventos ocorrem no computador acessado. Para logons interativos, a geração desses eventos ocorre no computador que está conectado. Se um logon de rede ocorre ao acessar um compartilhamento, esses eventos geram no computador que hospeda o recurso acessado. Se essa configuração é definida como **nenhuma auditoria**, é difícil ou impossível determinar que o usuário tenha acessado ou tentativa de acesso a computadores da organização.  
  
##### <a name="network-policy-server"></a>Servidor de Políticas de Rede  
Essa subcategoria relata eventos gerados por solicitações de acesso RADIUS (IAS) e proteção de acesso de rede (NAP) do usuário. Essas solicitações podem ser **Grant**, **negar**, **descartar**, **quarentena**, **bloqueio**e **Desbloquear**. Essa configuração de auditoria resultará em um volume médio ou alto de registros nos servidores NPS e IAS.  
  
##### <a name="ipsec-main-mode"></a>Do modo IPsec principal  
Essa subcategoria relata os resultados da chave Exchange protocolo IKE (Internet) e protocolo de Internet autenticado (AuthIP) durante as negociações de modo principal.  
  
##### <a name="ipsec-extended-mode"></a>Modo IPsec estendido  
Essa subcategoria relata os resultados de AuthIP durante negociações de modo estendido.  
  
##### <a name="other-logonlogoff-events"></a>Outros eventos de Logon/Logoff  
Essa subcategoria relata eventos relacionados ao fazer logoff, como o Remote Desktop Services sessão desconecta e reconecta, usando Executar como para executar processos em uma conta diferente e bloquear e desbloquear uma estação de trabalho e outro logon.  
  
##### <a name="logoff"></a>Logoff  
Essa subcategoria informa quando um usuário faz logoff do sistema. Esses eventos ocorrem no computador acessado. Para logons interativos, a geração desses eventos ocorre no computador que está conectado. Se um logon de rede ocorre ao acessar um compartilhamento, esses eventos geram no computador que hospeda o recurso acessado. Se essa configuração é definida como **nenhuma auditoria**, é difícil ou impossível determinar que o usuário tenha acessado ou tentativa de acesso a computadores da organização.  
  
##### <a name="account-lockout"></a>Bloqueio de conta  
Essa subcategoria informa quando uma conta de usuário está bloqueada devido a muitas tentativas de logon com falha.  
  
##### <a name="ipsec-quick-mode"></a>Modo rápido do IPsec  
Essa subcategoria relata os resultados do protocolo IKE e AuthIP durante negociações de modo rápido.  
  
##### <a name="special-logon"></a>Logon especial  
Essa subcategoria informa quando um logon especial é usado. Um logon especial é um logon que tenha privilégios de administrador equivalentes e pode ser usado para elevar um processo para um nível mais alto.  
  
#### <a name="policy-change"></a>Alteração de Política  
  
##### <a name="audit-policy-change"></a>Alteração de política de auditoria  
Essa subcategoria relata as alterações na diretiva de auditoria, incluindo alterações SACL.  
  
##### <a name="authentication-policy-change"></a>Alteração de política de autenticação  
Essa subcategoria relata alterações na política de autenticação.  
  
##### <a name="authorization-policy-change"></a>Alteração de política de autorização  
Essa subcategoria relata alterações na política de autorização, incluindo alterações de permissões (DACL).  
  
##### <a name="mpssvc-rule-level-policy-change"></a>Alteração da política de nível de regra MPSSVC  
Essa subcategoria relata alterações nas regras de política usadas pelo serviço de proteção da Microsoft (MPSSVC.exe). Esse serviço é usado pelo Firewall do Windows.  
  
##### <a name="filtering-platform-policy-change"></a>Alteração de políticas de plataforma de filtragem  
Essa subcategoria relata a adição e remoção de objetos da WFP, incluindo filtros de inicialização. Esses eventos podem ser muito altos volume.  
  
##### <a name="other-policy-change-events"></a>Outros eventos de alteração de política  
Essa subcategoria relatórios de outros tipos de alterações de política de segurança, como configuração do Trusted Platform Module (TPM) ou provedores de criptografia.  
  
#### <a name="privilege-use"></a>Uso de privilégios  
  
##### <a name="sensitive-privilege-use"></a>Uso de privilégios confidenciais  
Essa subcategoria relata quando uma conta de usuário ou o serviço usa um privilégio confidencial. Um privilégio confidencial inclui os seguintes direitos de usuário: atuar como parte do sistema operacional, fazer backup de arquivos e diretórios, criar um objeto token, os programas de depuração e habilitar contas de computador e usuário ser confiável para delegação, gerar auditorias de segurança representar um cliente após autenticação, carregar e descarregar drivers de dispositivo, gerenciar o log de auditoria e de segurança, modificar valores de ambiente de firmware, substituir um token no nível de processo, restaurar arquivos e diretórios e apropriar-se de arquivos ou outros objetos. Essa subcategoria de auditoria, você criará um alto volume de eventos.  
  
##### <a name="nonsensitive-privilege-use"></a>Uso de privilégios não confidenciais  
Essa subcategoria relata quando uma conta de usuário ou o serviço usa um privilégio não confidenciais. Um privilégio não confidenciais inclui os seguintes direitos de usuário: acessar o Gerenciador de credenciais como chamador confiável, acesso a este computador pela rede, Adicionar estações de trabalho ao domínio, Ajustar quotas de memória para um processo, permitir logon localmente, permitir logon pelos remoto Área de trabalho serviços, ignorar verificações completas, altere a hora do sistema, crie um arquivo de paginação, criar objetos globais, criar objetos compartilhados permanentemente, criar links simbólicos, negar acesso este computador pela rede, Negar logon como um trabalho em lotes, Negar logon como um serviço Negar logon localmente, Negar logon pelos serviços de área de trabalho remota, forçar o desligamento de um sistema remoto, aumentar um conjunto de trabalho do processo, aumentar a prioridade de agendamento, bloquear páginas na memória, faça logon como um trabalho em lotes, faça logon como um serviço, modificar rótulo de objeto, de volumes tarefas de manutenção, o único processo de perfil, criar o perfil de desempenho do sistema, remover o computador de estação de encaixe, desligue o sistema e sincronizar dados do serviço de diretório. Essa subcategoria de auditoria, você criará um volume muito alto de eventos.  
  
##### <a name="other-privilege-use-events"></a>Outros eventos de uso de privilégio  
Essa configuração de política de segurança não é usada no momento.  
  
#### <a name="object-access"></a>Acesso a objeto  
  
##### <a name="file-system"></a>Sistema de Arquivos  
Essa subcategoria informa quando são acessados objetos de sistema de arquivos. Somente objetos de sistema de arquivos com as SACLs fazer com que os eventos de auditoria a ser gerado e somente quando eles são acessados de forma que suas entradas da SACL correspondentes. Por si só, essa configuração de política não causará a auditoria de todos os eventos. Ele determina se a auditoria do evento de um usuário que acessa um objeto do sistema de arquivos que tem uma lista de controle de acesso especificada do sistema (SACL), com eficiência a habilitação da auditoria entrem em vigor.  
  
Se a configuração Auditoria de acesso a objeto é configurado para **sucesso**, uma entrada de auditoria é gerada toda vez que um usuário acessa com êxito um objeto com a SACL especificada. Se essa configuração de política é definida como **falha**, uma entrada de auditoria é gerada sempre que um usuário falhar em uma tentativa de acessar um objeto com a SACL especificada.  
  
##### <a name="registry"></a>Registro  
Essa subcategoria informa quando são acessados objetos de registro. Somente os objetos de registro com as SACLs fazer com que os eventos de auditoria a ser gerado e somente quando eles são acessados de forma que suas entradas da SACL correspondentes. Por si só, essa configuração de política não causará a auditoria de todos os eventos.  
  
##### <a name="kernel-object"></a>Objeto de kernel  
Essa subcategoria informa quando são acessados objetos de kernel, como mutexes e processos. Somente os objetos de kernel com as SACLs fazer com que os eventos de auditoria a ser gerado e somente quando eles são acessados de forma que suas entradas da SACL correspondentes. Normalmente objetos kernel só recebem as SACLs se as opções de auditoria AuditBaseObjects ou AuditBaseDirectories estão habilitadas.  
  
##### <a name="sam"></a>SAM  
Essa subcategoria informa quando são acessados objetos de banco de dados de autenticação de Gerenciador de contas de segurança (SAM) local.  
  
##### <a name="certification-services"></a>Serviços de certificação  
Essa subcategoria informa quando são executadas operações de serviços de certificação.  
  
##### <a name="application-generated"></a>Aplicativo gerado  
Essa subcategoria relata quando aplicativos de tentam de gerar eventos de auditoria usando o Windows a auditoria de interfaces de programação de aplicativo (APIs).  
  
##### <a name="handle-manipulation"></a>Manipulação de identificador  
Essa subcategoria informa quando um identificador para um objeto é aberto ou fechado. Somente os objetos com as SACLs fazer com que esses eventos a ser gerado e somente se a operação tentada identificador corresponder as entradas da SACL. Eventos de manipulação de identificador são gerados somente para tipos de objeto no qual a subcategoria de acesso do objeto correspondente está habilitada (por exemplo, sistema de arquivos ou registro).  
  
##### <a name="file-share"></a>Compartilhamento de arquivos  
Essa subcategoria informa quando um compartilhamento de arquivo é acessado. Por si só, essa configuração de política não causará a auditoria de todos os eventos. Ele determina se a auditoria do evento de um usuário que acessa um objeto de compartilhamento de arquivo que tem uma lista de controle de acesso especificada do sistema (SACL), com eficiência a habilitação da auditoria entrem em vigor.  
  
##### <a name="filtering-platform-packet-drop"></a>Remoção de pacote de plataforma de filtragem  
Essa subcategoria relata quando os pacotes são descartados pelo Windows WFP (plataforma). Esses eventos podem ser muito altos volume.  
  
##### <a name="filtering-platform-connection"></a>Filtragem de Conexão de plataforma  
Essa subcategoria relata quando as conexões são permitidas ou bloqueadas pelo WFP. Esses eventos podem ser altos volume.  
  
##### <a name="other-object-access-events"></a>Outros eventos de acesso a objeto  
Essa subcategoria relata outros eventos relacionados ao acesso do objeto, como trabalhos do Agendador de tarefas e objetos COM +.  
  
#### <a name="system"></a>Sistema  
  
##### <a name="security-state-change"></a>Alteração de estado de segurança  
Essa subcategoria relata alterações no estado de segurança do sistema, como quando o subsistema de segurança inicia e para.  
  
##### <a name="security-system-extension"></a>Extensão do sistema de segurança  
Essa subcategoria relata o carregamento de código de extensão, como pacotes de autenticação pelo subsistema de segurança.  
  
##### <a name="system-integrity"></a>Integridade do sistema  
Essa subcategoria relatórios sobre violações de integridade do subsistema de segurança.  
  
IPsec Driver  
  
Essa subcategoria relatórios sobre as atividades do driver Internet Protocol security (IPsec).  
  
##### <a name="other-system-events"></a>Outros eventos do sistema  
Essa subcategoria relatórios sobre outros eventos do sistema.  
  
Para obter mais informações sobre as descrições de subcategoria, consulte o [ferramenta Microsoft Security Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx).  
  
Cada organização deve examinar as categorias cobertas anteriores e subcategorias e habilite aquelas que melhor se ajustar a seu ambiente. Alterações de política de auditoria sempre devem ser testadas antes da implantação em um ambiente de produção.  
  
## <a name="configuring-windows-audit-policy"></a>Configurar política de auditoria do Windows

Política de auditoria do Windows pode ser definida usando as edições de políticas, auditpol.exe, APIs ou registro de grupo. Os métodos recomendados para configurar a política de auditoria para a maioria das empresas são auditpol.exe ou diretiva de grupo. Configuração de política de auditoria do sistema requer permissões de nível de administrador de conta ou as permissões delegadas apropriadas.  
  
> [!NOTE]  
> O **gerenciar o log de auditoria e segurança** privilégio deve ser fornecido a entidades de segurança (os administradores têm-os por padrão) para permitir a modificação de opções de recursos individuais, como arquivos, Active Directory de auditoria de acesso a objetos Objetos de diretório e chaves do registro.  
  
### <a name="setting-windows-audit-policy-by-using-group-policy"></a>Definindo diretiva de auditoria do Windows usando a diretiva de grupo

Para definir a política de auditoria usando políticas de grupo, configure as categorias de auditoria apropriadas localizadas sob **Configuration\Windows Settings\Security Settings\Local \ política de computador** (consulte a captura de tela a seguir para um exemplo do Editor de diretiva de Grupo Local (gpedit. msc)). Cada categoria de política de auditoria pode ser habilitada para **sucesso**, **falha**, ou **sucesso** e eventos de falha.  
  
![monitoramento do AD](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_6.gif)  
  
Política de auditoria avançada pode ser definida por meio do Active Directory ou diretivas de grupo local. Para definir a política de auditoria avançada, configurar as subcategorias apropriadas, localizadas sob **Configuration\Windows segurança computador \ Configurações de segurança \ política de auditoria** (consulte a captura de tela a seguir para obter um exemplo de Local Editor diretiva de grupo (gpedit. msc)). Cada subcategoria de política de auditoria pode ser habilitada para **sucesso**, **falha**, ou **sucesso** e **falha** eventos.  
  
![monitoramento do AD](media/Monitoring-Active-Directory-for-Signs-of-Compromise/SAD_7.gif)  
  
### <a name="setting-windows-audit-policy-using-auditpolexe"></a>Definindo diretiva de auditoria do Windows usando Auditpol.exe

Auditpol.exe (para a configuração de política de auditoria do Windows) foi introduzido no Windows Server 2008 e Windows Vista. Inicialmente, auditpol.exe só pode ser usado para definir a política de auditoria avançada, mas a diretiva de grupo pode ser usada no Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008, Windows 8 e Windows 7.  
  
Auditpol.exe é um utilitário de linha de comando. A sintaxe é da seguinte maneira:  
  
`auditpol /set /<Category|Subcategory>:<audit category> /<success|failure:> /<enable|disable>`
  
Exemplos de sintaxe Auditpol.exe:  
  
`auditpol /set /subcategory:"user account management" /success:enable /failure:enable`
  
`auditpol /set /subcategory:"logon" /success:enable /failure:enable`
  
`auditpol /set /subcategory:"IPSEC Main Mode" /failure:enable`
  
> [!NOTE]  
> Auditpol.exe define a política de auditoria avançada localmente. Se a política local em conflito com o Active Directory ou a diretiva de grupo local, configurações de diretiva de grupo geralmente prevalecem sobre as configurações de auditpol.exe. Se existirem vários grupo ou conflitos de política local, apenas uma política prevalecerá (isto é, substitua). Políticas de auditoria não serão mesclado.  
  
#### <a name="scripting-auditpol"></a>Script Auditpol

A Microsoft fornece um [exemplo de script](https://support.microsoft.com/kb/921469) para administradores que desejam configurar política de auditoria avançada usando um script em vez de digitar manualmente em cada comando auditpol.exe.  
  
**Observação** diretiva de grupo não sempre relatar com precisão o status de todas as políticas de auditoria habilitadas, enquanto o auditpol.exe faz. Ver [obter a política efetiva de auditoria no Windows 7 e Windows 2008 R2](http://blogs.technet.com/b/askds/archive/2011/03/11/getting-the-effective-audit-policy-in-windows-7-and-2008-r2.aspx) para obter mais detalhes.  
  
#### <a name="other-auditpol-commands"></a>Outros comandos Auditpol

Auditpol.exe pode ser usado para salvar e restaurar uma política de auditoria local e para exibir outros comandos relacionados à auditoria. Eis outro **auditpol** comandos.  
  
`auditpol /clear` – Usado para limpar e redefinir as políticas de auditoria local  
  
`auditpol /backup /file:<filename>` – Usado para fazer backup de uma política de auditoria local atual em um arquivo binário  
  
`auditpol /restore /file:<filename>` – Usado para importar um arquivo de política de auditoria salvas anteriormente para uma política de auditoria local  
  
`auditpol /<get/set> /option:<CrashOnAuditFail> /<enable/disable>` – Se essa configuração de política de auditoria é habilitada, ele faz com que o sistema parar imediatamente (com parada: Mensagem de {falha na auditoria} C0000244) se a auditoria de segurança não pode ser registrada por qualquer motivo. Normalmente, um evento deixa de ser registrado quando o log de auditoria de segurança está cheio e o método de retenção especificado para o log de segurança for **não substituir eventos** ou **substituir eventos periodicamente**. Normalmente ele é habilitado por ambientes que precisam de alto nível de garantia que o registro em log no log de segurança. Se habilitada, os administradores intimamente devem assistir o tamanho do log de segurança e girar os logs conforme necessário. Também podem ser definida com a política de grupo, modificando a opção de segurança **auditoria: Desligar o sistema imediatamente se não for possível registrar auditorias de segurança** (padrão = desabilitado).  
  
`auditpol /<get/set> /option:<AuditBaseObjects> /<enable/disable>` -Essa configuração de política de auditoria determina auditar o acesso a objetos do sistema global. Se essa política estiver habilitada, ela faz com que objetos do sistema, como mutexes, eventos, semáforos e DOS dispositivos para ser criado com uma lista de controle de acesso do sistema (SACL) padrão. A maioria dos administradores considere auditoria em objetos do sistema global para ser muito "barulhento", e eles serão habilitá-lo somente se houver suspeita de hackers mal-intencionados. Somente os objetos nomeados recebem um SACL. Se a auditoria objeto acesso auditoria política (ou subcategoria de auditoria de objeto de Kernel) também estiver habilitada, o acesso a esses objetos de sistema é auditado. Ao definir essa configuração de segurança, as alterações não entrarão em vigor até que você reinicie o Windows. Essa política também pode ser definida com a política de grupo, modificando a opção de segurança de auditoria de acesso de objetos do sistema (padrão = desabilitado).  
  
`auditpol /<get/set> /option:<AuditBaseDirectories> /<enable/disable>` -Essa configuração de política de auditoria especifica que objetos kernel nomeados (por exemplo, exclusões mútuas e semáforos) devem receber as SACLs quando eles são criados. AuditBaseDirectories afeta objetos de contêiner, enquanto AuditBaseObjects afeta objetos que não podem conter outros objetos.  
  
`auditpol /<get/set> /option:<FullPrivilegeAuditing> /<enable/disable>` -Essa configuração de política de auditoria especifica se o cliente gera um evento quando um ou mais desses privilégios são atribuídos a um token de segurança do usuário: AssignPrimaryTokenPrivilege, AuditPrivilege, BackupPrivilege, CreateTokenPrivilege, DebugPrivilege, EnableDelegationPrivilege, ImpersonatePrivilege, LoadDriverPrivilege, RestorePrivilege, SecurityPrivilege, SystemEnvironmentPrivilege, TakeOwnershipPrivilege e TcbPrivilege. Se essa opção não estiver habilitada (padrão = desabilitado), os privilégios BackupPrivilege e RestorePrivilege não estão registrados. A habilitação dessa opção pode fazer o log de segurança muito barulhento (às vezes, centenas de eventos de um segundo) durante uma operação de backup. Essa política também pode ser definida com a política de grupo, modificando a opção de segurança **auditoria: Auditar o uso de privilégios de Backup e restauração**.  
  
> [!NOTE]  
> Algumas informações fornecidas aqui foi tiradas da Microsoft [tipo de opção de auditoria](https://msdn.microsoft.com/library/dd973862(prot.20).aspx) e a ferramenta Microsoft SCM.  
  
## <a name="enforcing-traditional-auditing-or-advanced-auditing"></a>Imposição de auditoria tradicional ou auditoria avançada

No Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows 8, Windows 7 e Windows Vista, os administradores podem optar para habilitar as nove categorias tradicionais ou use as subcategorias. Ele é uma opção de binária que deve ser feita em cada sistema do Windows. As principais categorias podem ser habilitadas ou o subcategoriesit não pode ser ambos.  
  
Para impedir que a política de categoria tradicional herdado substituindo subcategorias de política de auditoria, você deve habilitar o **forçar configurações de subcategorias de política de auditoria (Windows Vista ou posterior) para substituir configurações de categoria de política de auditoria** configuração de política localizado sob **computador Configuration\Windows Settings\Security Settings\Local Policies\Security Options**.  
  
É recomendável que as subcategorias ser habilitadas e configuradas em vez das nove categorias principais. Isso exige que uma configuração de diretiva de grupo habilitada (para permitir as subcategorias substituir as categorias de auditoria), além da configuração as subcategorias diferentes que dão suporte a políticas de auditoria.  
  
Subcategorias de auditoria pode ser configurado usando vários métodos, incluindo a política de grupo e o programa de linha de comando, auditpol.exe.  
  
## <a name="next-steps"></a>Próximas etapas
  
* [Segurança avançada, auditoria no Windows 7 e Windows Server 2008 R2](https://social.technet.microsoft.com/wiki/contents/articles/advanced-security-auditing-in-windows-7-and-windows-server-2008-r2.aspx)  
  
* [Auditoria e conformidade no Windows Server 2008](https://technet.microsoft.com/magazine/2008.03.auditing.aspx)  
  
* [Como usar a diretiva de grupo para configurar a segurança detalhada, configurações de auditoria para computadores baseados em Windows Server 2008 e Windows Vista em um domínio do Windows Server 2008, em um domínio do Windows Server 2003 ou em um domínio Windows 2000](https://support.microsoft.com/kb/921469)  
  
* [Guia passo a passo de política de auditoria de segurança avançada](https://technet.microsoft.com/library/dd408940(WS.10).aspx)  
