---
ms.assetid: 0abe0976-4b49-45d6-a7b3-81d28bdb8210
title: "Recomendações de política de auditoria"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: d03d38834f89f8cda80b7af147e2bd3e31f4f990
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="audit-policy-recommendations"></a>Recomendações de política de auditoria

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Esta seção aborda as configurações de política de auditoria do Windows padrão, linha de base recomendada configurações de política de auditoria e as recomendações mais agressivas da Microsoft, para produtos de estação de trabalho e servidor.  

As recomendações de linha de base SCM mostradas aqui, juntamente com as configurações, que é recomendável para ajudar a detectar comprometimento, destinam-se apenas a ser um guia de linha de base inicial para administradores. Cada organização deve fazer suas própria decisões sobre as ameaças que enfrentam, seu tolerância aceitável risco e quais categorias de política de auditoria ou subcategorias devem permitir. Para obter mais informações sobre ameaças, consulte o [guia de ameaças e contramedidas](https://technet.microsoft.com/library/hh125921(v=ws.10).aspx). Os administradores sem uma política de auditoria cuidadoso in-loco são incentivados a começar com as configurações recomendadas aqui e depois modificar e testar, antes da implementação em seu ambiente de produção.  

As recomendações são para computadores de classe empresarial, que a Microsoft define como computadores que têm requisitos de segurança média e exigem um alto nível de funcionalidade operacional. Entidades que precisam de maior segurança requisitos devem considerar mais agressivos políticas de auditoria.  

> [!NOTE]  
> Padrões do Microsoft Windows e recomendações de linha de base foram extraídas do [ferramenta do Microsoft Security Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx).  

As seguintes configurações de política de auditoria de linha de base são recomendadas para computadores de segurança normal que não são conhecidos por serem sob ataque bem-sucedido, active por determinados adversários ou malware.  

## <a name="recommended-audit-policies-by-operating-system"></a>Recomendado políticas de auditoria pelo sistema operacional  
Esta seção contém tabelas que listam as recomendações de configuração de auditoria que se aplicam aos seguintes sistemas operacionais:  

-   Windows Server 2012  

-   Windows Server 2012 R2  

-   Windows Server 2008  

-   Windows 8  

-   Windows 7  

Estas tabelas contêm a configuração padrão do Windows, as recomendações de linha de base e as recomendações mais fortes para estes sistemas operacionais.  

**Legenda de tabelas de política de auditoria**  

|||  
|-|-|  
|**Notação**|**Recomendação**|  
|SIM|Em geral habilitar cenários|  
|Não|Fazer **não** em geral habilitar cenários|  
|SE|Habilitar se necessário para um cenário específico, ou se uma função ou recurso para o qual a auditoria é desejada é instalado no computador|  
|DC|Habilitar em controladores de domínio|  
|[Em branco]|Nenhuma recomendação|  

**Windows 8 e recomendações de configurações de auditoria 7 do Windows**  

**Política de auditoria**  

|Categoria de política de auditoria ou subcategoria|Padrão do Windows<br /><br />Falha de sucesso|Recomendação de linha de base<br /><br />Falha de sucesso|Recomendação mais forte<br /><br />Falha de sucesso|  
|----------------------------------------|------------------------------------------|--------------------------------------------------|--------------------------------------------------|  
|**Logon de conta**||||  
|Validação de credenciais de auditoria|Não há nenhuma|Sim não|Sim Sim|  
|Auditoria de serviço de autenticação Kerberos|||Sim Sim|  
|Auditoria de operações do tíquete de serviço Kerberos|||Sim Sim|  
|Auditoria de outros eventos de Logon de conta|||Sim Sim|  
|**Gerenciamento de contas**||||  
|Gerenciamento de grupo de aplicativos de auditoria||||  
|Auditoria do gerenciamento de conta de computador||Sim não|Sim Sim|  
|Gerenciamento de grupo de distribuição de auditoria||||  
|Auditoria de outros eventos de gerenciamento de conta||Sim não|Sim Sim|  
|Gerenciamento de grupo de segurança de auditoria||Sim não|Sim Sim|  
|Auditoria do gerenciamento de conta de usuário|Sim não|Sim não|Sim Sim|  
|**Acompanhamento detalhado**||||  
|Auditoria das atividades DPAPI|||Sim Sim|  
|Auditoria de criação de processo||Sim não|Sim Sim|  
|Auditoria de terminação de processo||||  
|Auditoria dos eventos RPC||||  
|**Acesso DS**||||  
|Auditoria da replicação do serviço de diretório detalhadas||||  
|Auditoria de acesso ao serviço de diretório||||  
|Auditoria de alterações de serviço de diretório||||  
|Auditoria da replicação do serviço de diretório||||  
|**Logon e Logoff**||||  
|Bloqueio de conta de auditoria|Sim não||Sim não|  
|Auditar declarações do usuário/dispositivo||||  
|Modo estendido do IPsec de auditoria||||  
|Auditoria do modo principal IPsec|||SE IF|  
|Auditoria do modo rápido IPsec||||  
|Auditoria do Logoff|Sim não|Sim não|Sim não|  
|Logon de auditoria|Sim não|Sim não|Sim Sim|  
|Servidor de política de rede de auditoria|Sim Sim|||  
|Auditoria de outros eventos de Logon/Logoff||||  
|Auditoria do Logon especial|Sim não|Sim não|Sim Sim|  
|**Acesso a objetos**||||  
|Auditoria do aplicativo gerado||||  
|Auditoria de serviços de certificação||||  
|Auditoria de compartilhamento de arquivos detalhada||||  
|Compartilhamento de arquivos de auditoria||||  
|Auditoria de sistema de arquivos||||  
|Auditoria de Conexão de plataforma de filtragem||||  
|Descarte de pacote de plataforma de filtragem de auditoria||||  
|Manipulação de identificador de auditoria||||  
|Auditoria de objeto Kernel||||  
|Auditoria de outros eventos de acesso do objeto||||  
|Auditoria de registro||||  
|Auditoria de armazenamento removível||||  
|Auditoria SAM||||  
|Auditoria do preparo da política de acesso Central||||  
|**Alteração da política**||||  
|Auditoria de alteração de política de auditoria|Sim não|Sim Sim|Sim Sim|  
|Auditoria da alteração da política de autenticação|Sim não|Sim não|Sim Sim|  
|Auditoria de alteração de políticas de autorização||||  
|Auditoria de alteração de política de plataforma de filtragem||||  
|Auditoria da alteração da política de nível de regra MPSSVC|||Sim  |  
|Auditoria de outros eventos de alteração de política||||  
|**Uso de privilégio**||||  
|Auditoria de uso de privilégio não importante||||  
|Auditoria de outros eventos de uso de privilégio||||  
|Auditoria de uso de privilégio importante||||  
|**Sistema**||||  
|Auditoria do Driver IPsec||Sim Sim|Sim Sim|  
|Auditoria de outros eventos do sistema|Sim Sim|||  
|Alteração de estado de segurança de auditoria|Sim não|Sim Sim|Sim Sim|  
|Auditoria de extensão do sistema de segurança||Sim Sim|Sim Sim|  
|Auditoria de integridade do sistema|Sim Sim|Sim Sim|Sim Sim|  
|**Acesso a objetos globais de auditoria**||||  
|Auditoria do Driver IPsec||||  
|Auditoria de outros eventos do sistema||||  
|Alteração de estado de segurança de auditoria||||  
|Auditoria de extensão do sistema de segurança||||  
|Auditoria de integridade do sistema||||  

**Recomendações de configurações de auditoria do Windows Server 2008, Windows Server 2008 R2 e Windows Server 2012**  

|Categoria de política de auditoria ou subcategoria|Padrão do Windows<br /><br />Falha de sucesso|Recomendação de linha de base<br /><br />Falha de sucesso|Recomendação mais forte<br /><br />Falha de sucesso|  
|----------------------------------------|------------------------------------------|--------------------------------------------------|--------------------------------------------------|  
|**Logon de conta**||||  
|Validação de credenciais de auditoria|Não há nenhuma|Sim Sim|Sim Sim|  
|Auditoria de serviço de autenticação Kerberos|||Sim Sim|  
|Auditoria de operações do tíquete de serviço Kerberos|||Sim Sim|  
|Auditoria de outros eventos de Logon de conta|||Sim Sim|  
|**Gerenciamento de contas**||||  
|Gerenciamento de grupo de aplicativos de auditoria||||  
|Auditoria do gerenciamento de conta de computador||Sim DC|Sim Sim|  
|Gerenciamento de grupo de distribuição de auditoria||||  
|Auditoria de outros eventos de gerenciamento de conta||Sim Sim|Sim Sim|  
|Gerenciamento de grupo de segurança de auditoria||Sim Sim|Sim Sim|  
|Auditoria do gerenciamento de conta de usuário|Sim não|Sim Sim|Sim Sim|  
|**Acompanhamento detalhado**||||  
|Auditoria das atividades DPAPI|||Sim Sim|  
|Auditoria de criação de processo||Sim não|Sim Sim|  
|Auditoria de terminação de processo||||  
|Auditoria dos eventos RPC||||  
|**Acesso DS**||||  
|Auditoria da replicação do serviço de diretório detalhadas||||  
|Auditoria de acesso ao serviço de diretório||DC DC|DC DC|  
|Auditoria de alterações de serviço de diretório||DC DC|DC DC|  
|Auditoria da replicação do serviço de diretório||||  
|**Logon e Logoff**||||  
|Bloqueio de conta de auditoria|Sim não||Sim não|  
|Auditar declarações do usuário/dispositivo||||  
|Modo estendido do IPsec de auditoria||||  
|Auditoria do modo principal IPsec|||SE IF|  
|Auditoria do modo rápido IPsec||||  
|Auditoria do Logoff|Sim não|Sim não|Sim não|  
|Logon de auditoria|Sim não|Sim Sim|Sim Sim|  
|Servidor de política de rede de auditoria|Sim Sim|||  
|Auditoria de outros eventos de Logon/Logoff|||Sim Sim|  
|Auditoria do Logon especial|Sim não|Sim não|Sim Sim|  
|**Acesso a objetos**||||  
|Auditoria do aplicativo gerado||||  
|Auditoria de serviços de certificação||||  
|Auditoria de compartilhamento de arquivos detalhada||||  
|Compartilhamento de arquivos de auditoria||||  
|Auditoria de sistema de arquivos||||  
|Auditoria de Conexão de plataforma de filtragem||||  
|Descarte de pacote de plataforma de filtragem de auditoria||||  
|Manipulação de identificador de auditoria||||  
|Auditoria de objeto Kernel||||  
|Auditoria de outros eventos de acesso do objeto||||  
|Auditoria de registro||||  
|Auditoria de armazenamento removível||||  
|Auditoria SAM||||  
|Auditoria do preparo da política de acesso Central||||  
|**Alteração da política**||||  
|Auditoria de alteração de política de auditoria|Sim não|Sim Sim|Sim Sim|  
|Auditoria da alteração da política de autenticação|Sim não|Sim não|Sim Sim|  
|Auditoria de alteração de políticas de autorização||||  
|Auditoria de alteração de política de plataforma de filtragem||||  
|Auditoria da alteração da política de nível de regra MPSSVC|||Sim  |  
|Auditoria de outros eventos de alteração de política||||  
|**Uso de privilégio**||||  
|Auditoria de uso de privilégio não importante||||  
|Auditoria de outros eventos de uso de privilégio||||  
|Auditoria de uso de privilégio importante||||  
|**Sistema**||||  
|Auditoria do Driver IPsec||Sim Sim|Sim Sim|  
|Auditoria de outros eventos do sistema|Sim Sim|||  
|Alteração de estado de segurança de auditoria|Sim não|Sim Sim|Sim Sim|  
|Auditoria de extensão do sistema de segurança||Sim Sim|Sim Sim|  
|Auditoria de integridade do sistema|Sim Sim|Sim Sim|Sim Sim|  
|**Acesso a objetos globais de auditoria**||||  
|Auditoria do Driver IPsec||||  
|Auditoria de outros eventos do sistema||||  
|Alteração de estado de segurança de auditoria||||  
|Auditoria de extensão do sistema de segurança||||  
|Auditoria de integridade do sistema||||  

## <a name="set-audit-policy-on-workstations-and-servers"></a>Definir a política de auditoria em servidores e estações de trabalho  
Todos os planos de gerenciamento de log de eventos devem monitorar estações de trabalho e servidores. Um erro comum é apenas monitorar servidores ou controladores de domínio. Como mal-intencionado hackers frequentemente inicialmente ocorre em estações de trabalho, não monitorando estações de trabalho está ignorando a melhor e mais antiga fonte de informações.  

Os administradores devem cuidadosamente revisar e teste qualquer política de auditoria antes da implementação em seu ambiente de produção.  

## <a name="events-to-monitor"></a>Eventos a serem monitorados  
Uma ID de evento perfeito para gerar um alerta de segurança deve conter os seguintes atributos:  

-   Alta probabilidade dessa ocorrência indica atividades não autorizadas  

-   Número baixo de falsos positivos  

-   Ocorrência deve resultar em uma resposta de investigação/perícia  

Dois tipos de eventos devem ser monitorados e alertados:  

1.  Esses eventos em que até mesmo uma única ocorrência indica atividades não autorizadas  

2.  Um acúmulo de eventos acima de uma linha de base esperada e aceito  

Um exemplo do primeiro evento é:  

Se a administradores de domínio (DAs) são proibidos de fazer logon nos computadores que não são controladores de domínio, uma única ocorrência de um membro DA fazer logon em uma estação de trabalho do usuário final deve gerar um alerta e ser investigada. Esse tipo de alerta é fácil gerar usando o evento de auditoria do Logon especial 4964 (grupos especiais atribuídos a um novo logon). Outros exemplos de alertas de instância única incluem:  

-   Se o servidor A nunca deve se conectar ao servidor B, alerta quando eles se conectarem entre si.  

-   Alerte se uma conta de usuário final normal inesperadamente é adicionada a um grupo de segurança confidenciais.  

-   Se os funcionários no local de fábrica A funcionam nunca à noite, alerta quando um usuário faz logon à meia-noite.  

-   Alerte se um serviço não autorizadas estiver instalado em um controlador de domínio.  

-   Investigue se um usuário final normal tenta fazer logon diretamente um SQL Server para os quais não tenham nenhum motivo claro para fazê-lo.  

-   Se você não têm membros em seu grupo DA e alguém adiciona propriamente ditos lá, verificá-la imediatamente.  

Um exemplo do segundo evento é:  

Um número de logons anormal pode indicar uma ataque de adivinhação de senha. Para uma empresa fornecer um alerta para um número excepcionalmente alto de logons, ele devem primeiro entender os níveis normais de logons com falha em seus ambientes antes de um evento de segurança mal-intencionados.  

Para obter uma lista abrangente de eventos que você deve incluir ao monitorar sinais de comprometimento, consulte [apêndice l: eventos a serem monitorados](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md).  

## <a name="active-directory-objects-and-attributes-to-monitor"></a>Objetos do Active Directory e atributos para Monitor  
A seguir está as contas, grupos e atributos que você deve monitorar para ajudar a detectar tentativas de comprometer sua instalação de serviços de domínio do Active Directory.  

-   Sistemas de desativação ou remoção de software antivírus e antimalware (automaticamente o reinício proteção quando ele estiver desabilitado manualmente)  

-   Contas de administrador para alterações não autorizadas  

-   Atividades que são executadas usando contas privilegiadas (automaticamente remover conta quando atividades suspeitas são concluídas ou alocadas tempo expirou)  

-   Privilégios e contas de VIP no AD DS. Monitorar as alterações, especialmente as alterações nos atributos na guia conta (por exemplo, cn, nome, sAMAccountName, userPrincipalName ou userAccountControl). Além de monitoramento as contas, restringir quem pode modificar as contas para como pequeno um conjunto de usuários administrativos quanto possível.  

Consulte [apêndice l: eventos a serem monitorados](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md) para obter uma lista de eventos recomendados para o monitor, suas classificações de importância e um resumo da mensagem do evento.  

-   Servidores grupo pela classificação das cargas de trabalho, que permite identificar rapidamente os servidores que devem ser monitorados melhor e mais severamente configurado  

-   Muda para as propriedades e a associação dos seguintes grupos do AD DS: administradores de empresa (EA), os administradores de domínio (DA), os administradores (BA) e administradores de esquema (SA)  

-   Contas desabilitadas privilegiadas (como contas de administrador internas no Active Directory e em sistemas de membro) para habilitar as contas  

-   Contas de gerenciamento para todas as gravações fazer logon na conta  

-   Assistente de configuração de segurança interna para configurar o serviço, registro, auditoria e configurações de firewall para reduzir a superfície de ataque do servidor. Use este assistente se você implementar atalhos servidores como parte da estratégia de host administrativos.  

## <a name="additional-information-for-monitoring-active-directory-domain-services"></a>Informações adicionais para monitorar os serviços de domínio do Active Directory  
Examine os links a seguir para obter informações adicionais sobre como monitorar o AD DS:  
  
-   [Auditoria de acesso a objeto global é mágica](http://blogs.technet.com/b/askds/archive/2011/03/10/global-object-access-auditing-is-magic.aspx) -fornece informações sobre como configurar e usar configuração avançada de auditoria política que foi adicionado ao Windows 7 e Windows Server 2008 R2.  

-   [Apresentando a auditoria de alterações no Windows 2008](http://blogs.technet.com/b/askds/archive/2007/10/19/introducing-auditing-changes-in-windows-2008.aspx) -apresenta a auditoria alterações feitas no Windows 2008.  

-   [Legais truques de auditoria no Vista e 2008](http://blogs.technet.com/b/askds/archive/2007/11/16/cool-auditing-tricks-in-vista-and-2008.aspx) -explica os novos recursos interessantes de auditoria no Windows Vista e Windows Server 2008, que pode ser usado para solução de problemas ou ver o que está acontecendo em seu ambiente.  

-   [Loja centralizada para a auditoria no Windows Server 2008 e Windows Vista](http://blogs.technet.com/b/askds/archive/2008/03/27/one-stop-shop-for-auditing-in-windows-server-2008-and-windows-vista.aspx) -contém uma compilação de auditoria de recursos e informações contidas no Windows Server 2008 e Windows Vista.  

-   [Guia passo a passo de auditoria do AD DS](https://technet.microsoft.com/library/a9c25483-89e2-4202-881c-ea8e02b4b2a5.aspx) -descreve o novo recurso de auditoria de serviços de domínio do Active Directory (AD DS) no Windows Server 2008. Ele também fornece os procedimentos para implementar esse novo recurso.  

## <a name="general-list-of-security-event-id-recommendation-criticalities"></a>Lista geral de Criticalities de recomendação de ID de evento de segurança  
Todas as recomendações de ID de evento são acompanhadas por um nível de importância classificação da seguinte maneira:  

**Alto:** IDs de evento com uma classificação de alto nível de importância deve sempre imediatamente serão alertados e investigadas.  

**Médio:** uma ID de evento com uma classificação de importância médio pode indicar atividade maliciosa, mas eles devem ser acompanhados por alguns outra anormalidade (por exemplo, um número incomum que ocorrem em um determinado período de tempo, ocorrências inesperadas ou ocorrências em um computador que normalmente não seria esperado para registrar o evento.). Um evento de importância médio pode também r ser coletada como uma métrica e comparado ao longo do tempo.  

**Baixo:** e ID de evento com um eventos de pouca importância não devem conseguir atenção ou causar alertas, a menos que correlacionados com eventos de nível de importância médio ou alto.  

Essas recomendações devem fornecer um guia de linha de base para o administrador. Todas as recomendações devem ser revisadas completamente antes da implementação em um ambiente de produção.  

Consulte [apêndice l: eventos a serem monitorados](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md) para obter uma lista dos eventos recomendados para o monitor, suas classificações de importância e um resumo da mensagem do evento.  
