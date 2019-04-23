---
ms.assetid: 0abe0976-4b49-45d6-a7b3-81d28bdb8210
title: Recomendações de política de auditoria
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 343a9a7aedf22e9c021249f00fb628f871a2ce1f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835747"
---
# <a name="audit-policy-recommendations"></a>Recomendações de política de auditoria

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10, Windows 8.1, Windows 7

Esta seção aborda as configurações de política de auditoria do Windows padrão, linha de base recomendada configurações de política de auditoria e as recomendações mais agressivas da Microsoft, para produtos da estação de trabalho e de servidor.  

As recomendações de linha de base SCM mostradas aqui, juntamente com as configurações, que é recomendável para ajudar a detectar o comprometimento, destinam-se somente a ser um guia de linha de base inicial para os administradores. Cada organização deve tomar suas próprias decisões sobre as ameaças que enfrentam, seus tolerâncias de risco aceitável e quais categorias de política de auditoria ou subcategorias deve habilitar. Para obter mais informações sobre ameaças, consulte o [guia de ameaças e contramedidas](https://technet.microsoft.com/library/hh125921(v=ws.10).aspx). Os administradores sem uma política de auditoria cuidadosa são encorajados a começar com as configurações recomendadas aqui e, em seguida, modificar e testar, antes de implementar em seu ambiente de produção.  

As recomendações são para computadores de nível corporativo, que a Microsoft define como os computadores que têm requisitos de segurança médio e exigem um alto nível de funcionalidade operacional. Políticas de auditoria de entidades que precisam de maior segurança requisitos devem considerar mais agressivos.  

> [!NOTE]  
> Microsoft Windows assume como padrão e as recomendações de linha de base foram tiradas de [ferramenta Microsoft Security Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx).  

As seguintes configurações de política de auditoria de linha de base são recomendadas para computadores de segurança normal que não são conhecidos por estar sendo atacado do Active Directory, com êxito por determinados adversários ou malware.  

## <a name="recommended-audit-policies-by-operating-system"></a>Políticas de auditoria pelo sistema operacional recomendadas  
Esta seção contém as tabelas que listam as recomendações de configuração de auditoria que se aplicam aos seguintes sistemas operacionais:  

-   Windows Server 2016 

-   Windows Server 2012  

-   Windows Server 2012 R2  

-   Windows Server 2008  

-   Windows 10

-   Windows 8.1  

-   Windows 7  

Essas tabelas contêm a configuração do Windows padrão, as recomendações de linha de base e as recomendações mais fortes para esses sistemas operacionais.  

**Legenda de tabelas de política de auditoria**  

|||  
|-|-|  
|**Notação**|**Recomendação**|  
|SIM|Em geral, habilitar cenários|  
|NÃO|Fazer **não** em geral, habilitar cenários|  
|IF|Habilitar se necessária para um cenário específico, ou se uma função ou recurso para o qual a auditoria é desejada é instalado no computador|  
|DC|Habilitar em controladores de domínio|  
|[Blank]|Nenhuma recomendação|  

**Recomendações de configurações de auditoria 7 do Windows, Windows 8 e Windows 10**  

**A política de auditoria**  

|Categoria de política de auditoria ou subcategoria|Padrão do Windows<br /><br />Falha de sucesso|Recomendação de linha de base<br /><br />Falha de sucesso|Recomendação mais forte<br /><br />Falha de sucesso|  
|----------------------------------------|------------------------------------------|--------------------------------------------------|--------------------------------------------------|  
|**Logon de conta**||||  
|Auditoria da validação de credenciais|Não há nenhum|Sim não|Sim Sim|  
|Auditoria do serviço de autenticação Kerberos|||Sim Sim|  
|Auditoria das operações do tíquete de serviço Kerberos|||Sim Sim|  
|Auditoria de outros eventos de logon de conta|||Sim Sim|  
|**Gerenciamento de conta**||||  
|Auditoria do gerenciamento de grupo de aplicativos||||  
|Auditoria de Gerenciamento de Conta de Computador||Sim não|Sim Sim|  
|Auditoria de Gerenciamento de Grupo de Distribuição||||  
|Auditoria de Outros Eventos de Gerenciamento de Contas||Sim não|Sim Sim|  
|Auditoria de Gerenciamento de Grupo de Segurança||Sim não|Sim Sim|  
|Auditoria de Gerenciamento de Conta de Usuário|Sim não|Sim não|Sim Sim|  
|**Acompanhamento detalhado**||||  
|Auditoria de Atividade DPAPI|||Sim Sim|  
|Auditoria de Criação de Processo||Sim não|Sim Sim|  
|Auditoria de Terminação de Processo||||  
|Auditoria de Eventos de RPC||||  
|**Acesso DS**||||  
|Auditoria de Replicação Detalhada do Serviço de Diretório||||  
|Auditoria do acesso ao serviço de diretório||||  
|Auditoria de Alterações no Serviço de Diretório||||  
|Auditoria de Replicação do Serviço de Diretório||||  
|**Logon e Logoff**||||  
|Auditoria de Bloqueio de Conta|Sim não||Sim não|  
|Auditoria das declarações de dispositivo/usuário||||  
|Auditoria de Modo Estendido do IPsec||||  
|Auditoria de Modo Principal do IPsec|||IF     IF|  
|Auditoria de Modo Rápido do IPsec||||  
|Logoff de Auditoria|Sim não|Sim não|Sim não|  
|Logon de auditoria <sup>1</sup>|Sim Sim|Sim Sim|Sim Sim|  
|Auditoria de Servidor de Política de Rede|Sim Sim|||  
|Auditoria de outros eventos de logon/logoff||||  
|Auditoria de Logon Especial|Sim não|Sim não|Sim Sim|  
|**Acesso a objetos**||||  
|Auditoria de Aplicativo Gerado||||  
|Auditoria de serviços de certificação||||  
|Compartilhamento de Arquivos de Auditoria Detalhado||||  
|Auditoria de Compartilhamento de Arquivos||||  
|Auditoria de Sistema de Arquivos||||  
|Auditoria de Conexão de Plataforma de Filtragem||||  
|Auditoria de Descarte de Pacote de Plataforma de Filtragem||||  
|Auditoria de Manipulação de Identificador||||  
|Auditoria de Objeto Kernel||||  
|Auditoria de Outros Eventos de Acesso a Objetos||||  
|Auditoria de Registro||||  
|Auditoria do armazenamento removível||||  
|Auditoria de SAM||||  
|Auditoria do preparo da política de acesso central||||  
|**Alteração da política**||||  
|Auditoria de Alteração de Políticas de Auditoria|Sim não|Sim Sim|Sim Sim|  
|Auditoria de Alteração de Políticas de Autenticação|Sim não|Sim não|Sim Sim|  
|Auditoria de Alteração de Políticas de Autorização||||  
|Auditoria de Alteração na Política da Plataforma de Filtragem||||  
|Auditoria de Alteração na Política de Nível de Regra MPSSVC|||Sim  |  
|Auditoria de Outros Eventos de Alteração de Políticas||||  
|**Uso de privilégios**||||  
|Auditoria de Uso de Privilégio Não Importante||||  
|Auditoria de outros eventos de uso de privilégios||||  
|Auditoria de Uso de Privilégio Importante||||  
|**Sistema**||||  
|Auditoria do driver IPsec||Sim Sim|Sim Sim|  
|Auditoria de outros eventos do sistema|Sim Sim|||  
|Auditoria de Alteração no Estado de Segurança|Sim não|Sim Sim|Sim Sim|  
|Auditoria de Extensão do Sistema de Segurança||Sim Sim|Sim Sim|  
|Auditoria da integridade do sistema|Sim Sim|Sim Sim|Sim Sim|  
|**Acesso ao objeto global de auditoria**||||  
|Auditoria do driver IPsec||||  
|Auditoria de outros eventos do sistema||||  
|Auditoria de Alteração no Estado de Segurança||||  
|Auditoria de Extensão do Sistema de Segurança||||  
|Auditoria da integridade do sistema||||  

<sup>1</sup> começando com o Windows 10 versão 1809, auditoria de Logon é habilitada por padrão para êxito e falha. Nas versões anteriores do Windows, êxito só é habilitado por padrão.

**Recomendações de configurações de auditoria do Windows Server 2008, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2016**  

|Categoria de política de auditoria ou subcategoria|Padrão do Windows<br /><br />Falha de sucesso|Recomendação de linha de base<br /><br />Falha de sucesso|Recomendação mais forte<br /><br />Falha de sucesso|  
|----------------------------------------|------------------------------------------|--------------------------------------------------|--------------------------------------------------|  
|**Logon de conta**||||  
|Auditoria da validação de credenciais|Não há nenhum|Sim Sim|Sim Sim|  
|Auditoria do serviço de autenticação Kerberos|||Sim Sim|  
|Auditoria das operações do tíquete de serviço Kerberos|||Sim Sim|  
|Auditoria de outros eventos de logon de conta|||Sim Sim|  
|**Gerenciamento de conta**||||  
|Auditoria do gerenciamento de grupo de aplicativos||||  
|Auditoria de Gerenciamento de Conta de Computador||Sim o controlador de domínio|Sim Sim|  
|Auditoria de Gerenciamento de Grupo de Distribuição||||  
|Auditoria de Outros Eventos de Gerenciamento de Contas||Sim Sim|Sim Sim|  
|Auditoria de Gerenciamento de Grupo de Segurança||Sim Sim|Sim Sim|  
|Auditoria de Gerenciamento de Conta de Usuário|Sim não|Sim Sim|Sim Sim|  
|**Acompanhamento detalhado**||||  
|Auditoria de Atividade DPAPI|||Sim Sim|  
|Auditoria de Criação de Processo||Sim não|Sim Sim|  
|Auditoria de Terminação de Processo||||  
|Auditoria de Eventos de RPC||||  
|**Acesso DS**||||  
|Auditoria de Replicação Detalhada do Serviço de Diretório||||  
|Auditoria do acesso ao serviço de diretório||CONTROLADOR DE DOMÍNIO DO CONTROLADOR DE DOMÍNIO|CONTROLADOR DE DOMÍNIO DO CONTROLADOR DE DOMÍNIO|  
|Auditoria de Alterações no Serviço de Diretório||CONTROLADOR DE DOMÍNIO DO CONTROLADOR DE DOMÍNIO|CONTROLADOR DE DOMÍNIO DO CONTROLADOR DE DOMÍNIO|  
|Auditoria de Replicação do Serviço de Diretório||||  
|**Logon e Logoff**||||  
|Auditoria de Bloqueio de Conta|Sim não||Sim não|  
|Auditoria das declarações de dispositivo/usuário||||  
|Auditoria de Modo Estendido do IPsec||||  
|Auditoria de Modo Principal do IPsec|||IF     IF|  
|Auditoria de Modo Rápido do IPsec||||  
|Logoff de Auditoria|Sim não|Sim não|Sim não|  
|Logon de Auditoria|Sim Sim|Sim Sim|Sim Sim|  
|Auditoria de Servidor de Política de Rede|Sim Sim|||  
|Auditoria de outros eventos de logon/logoff|||Sim Sim|  
|Auditoria de Logon Especial|Sim não|Sim não|Sim Sim|  
|**Acesso a objetos**||||  
|Auditoria de Aplicativo Gerado||||  
|Auditoria de serviços de certificação||||  
|Compartilhamento de Arquivos de Auditoria Detalhado||||  
|Auditoria de Compartilhamento de Arquivos||||  
|Auditoria de Sistema de Arquivos||||  
|Auditoria de Conexão de Plataforma de Filtragem||||  
|Auditoria de Descarte de Pacote de Plataforma de Filtragem||||  
|Auditoria de Manipulação de Identificador||||  
|Auditoria de Objeto Kernel||||  
|Auditoria de Outros Eventos de Acesso a Objetos||||  
|Auditoria de Registro||||  
|Auditoria do armazenamento removível||||  
|Auditoria de SAM||||  
|Auditoria do preparo da política de acesso central||||  
|**Alteração da política**||||  
|Auditoria de Alteração de Políticas de Auditoria|Sim não|Sim Sim|Sim Sim|  
|Auditoria de Alteração de Políticas de Autenticação|Sim não|Sim não|Sim Sim|  
|Auditoria de Alteração de Políticas de Autorização||||  
|Auditoria de Alteração na Política da Plataforma de Filtragem||||  
|Auditoria de Alteração na Política de Nível de Regra MPSSVC|||Sim  |  
|Auditoria de Outros Eventos de Alteração de Políticas||||  
|**Uso de privilégios**||||  
|Auditoria de Uso de Privilégio Não Importante||||  
|Auditoria de outros eventos de uso de privilégios||||  
|Auditoria de Uso de Privilégio Importante||||  
|**Sistema**||||  
|Auditoria do driver IPsec||Sim Sim|Sim Sim|  
|Auditoria de outros eventos do sistema|Sim Sim|||  
|Auditoria de Alteração no Estado de Segurança|Sim não|Sim Sim|Sim Sim|  
|Auditoria de Extensão do Sistema de Segurança||Sim Sim|Sim Sim|  
|Auditoria da integridade do sistema|Sim Sim|Sim Sim|Sim Sim|  
|**Acesso ao objeto global de auditoria**||||  
|Auditoria do driver IPsec||||  
|Auditoria de outros eventos do sistema||||  
|Auditoria de Alteração no Estado de Segurança||||  
|Auditoria de Extensão do Sistema de Segurança||||  
|Auditoria da integridade do sistema||||  

## <a name="set-audit-policy-on-workstations-and-servers"></a>Definir política de auditoria em servidores e estações de trabalho  
Todos os planos de gerenciamento de log de eventos devem monitorar os servidores e estações de trabalho. Um erro comum é monitorar somente os controladores de domínio ou servidores. Como geralmente inicialmente de hackers mal-intencionados ocorre em estações de trabalho, estações de trabalho de monitoramento não está ignorando a origem de melhor e mais recente das informações.  

Cuidadosamente, os administradores devem examinar e testar qualquer política de auditoria antes da implementação no ambiente de produção.  

## <a name="events-to-monitor"></a>Eventos a Monitorar  
Uma ID de evento perfeito para gerar um alerta de segurança deve conter os seguintes atributos:  

-   Grande probabilidade de que essa ocorrência indica atividades não autorizadas  

-   Número baixo de falsos positivos  

-   Ocorrência deve resultar em uma resposta de investigação/forense  

Dois tipos de eventos devem ser monitorados e alertados:  

1.  Esses eventos em que até mesmo uma única ocorrência indica atividades não autorizadas  

2.  Um acúmulo de eventos acima de uma linha de base esperada e aceita  

Um exemplo do primeiro evento é:  

Se a Admins. do domínio (DAs) estão proibidos de fazer logon em computadores que não são controladores de domínio, uma única ocorrência de um membro DA fazendo logon em uma estação de trabalho do usuário final deve gerar um alerta e ser investigada. Este tipo de alerta é fácil gerar usando o evento de auditoria de Logon especial 4964 (grupos especiais atribuídos a um novo logon). Outros exemplos de alertas de instância única:  

-   Se o servidor A nunca deve se conectar ao servidor B, o alerta quando eles se conectam uns aos outros.  

-   Alerta se inesperadamente, uma conta normal do usuário final for adicionada a um grupo de segurança confidenciais.  

-   Se os funcionários em um único local factory nunca funcionam durante a noite, alerta quando um usuário faz logon à meia-noite.  

-   Alerta se um serviço não autorizado é instalado em um controlador de domínio.  

-   Investigue se um usuário final regular tentativas de logon diretamente em um SQL Server para os quais não têm nenhum motivo claro para fazer isso.  

-   Se você não têm membros no seu grupo DA e alguém adiciona próprios lá, verifique-lo imediatamente.  

Um exemplo do segundo evento é:  

Um número anormal de logons com falha pode indicar uma ataque de adivinhação de senha. Para uma empresa fornecer um alerta para um número excepcionalmente alto de logons com falha, eles devem primeiro entender os níveis normais de logons com falha dentro de seu ambiente antes de um evento de segurança mal-intencionado.  

Para obter uma lista abrangente de eventos que você deve incluir ao monitorar em busca de sinais de comprometimento, consulte [l Apêndice: Eventos a serem monitorados](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md).  

## <a name="active-directory-objects-and-attributes-to-monitor"></a>Objetos do Active Directory e os atributos ao Monitor  
A seguir é as contas, grupos e atributos que você deve monitorar para ajudá-lo a detectar tentativas de comprometer a sua instalação do Active Directory Domain Services.  

-   Sistemas de desativação ou remoção de software antivírus e antimalware (automaticamente reiniciar a proteção quando ele é desativado manualmente)  

-   Contas de administrador para que as alterações não autorizadas  

-   Atividades que são executadas usando contas com privilégios (automaticamente remover conta quando atividades suspeitas forem concluídas ou alocadas tempo expirou)  

-   Privilegiados e contas de VIP no AD DS. Monitorar as alterações, especialmente as alterações em atributos na guia conta (por exemplo, cn, nome, sAMAccountName, userPrincipalName ou userAccountControl). Além de monitorar as contas, restringir quem pode modificar as contas a serem tão pequeno que um conjunto de usuários administrativos quanto possível.  

Consulte [apêndice l: Eventos a serem monitorados](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md) para obter uma lista de eventos recomendados para monitorar, suas classificações de nível de importância e um resumo de mensagem do evento.  

-   Servidores de grupo usando a classificação de suas cargas de trabalho, que permite que você identifique rapidamente os servidores que devem ser o mais se aproxima monitorados e configurados de forma mais rigorosa  

-   Alterações para as propriedades e os membros dos grupos do AD DS a seguir: Administradores de empresa (EA), Admins. do domínio (DA), os administradores (BA) e administradores de esquema (SA)  

-   Contas desabilitadas com privilégios (como contas de administrador internas no Active Directory e em sistemas de membro) para habilitar as contas  

-   Contas de gerenciamento para todas as gravações de log para a conta  

-   Assistente de configuração de segurança internas para configurar o serviço, registro, auditoria e configurações de firewall para reduzir a superfície de ataque do servidor. Use este assistente se você implementar servidores de salto como parte de sua estratégia de hosts administrativos.  

## <a name="additional-information-for-monitoring-active-directory-domain-services"></a>Informações adicionais para monitoramento de serviços de domínio do Active Directory  
Examine os links a seguir para obter mais informações sobre como monitorar o AD DS:  
  
-   [Auditoria de acesso a objeto global é a mágica](http://blogs.technet.com/b/askds/archive/2011/03/10/global-object-access-auditing-is-magic.aspx) -fornece informações sobre como configurar e usar a política de configuração de auditoria avançada que foi adicionado ao Windows 7 e Windows Server 2008 R2.  

-   [Introduzindo alterações de auditoria no Windows 2008](http://blogs.technet.com/b/askds/archive/2007/10/19/introducing-auditing-changes-in-windows-2008.aspx) -apresenta as alterações de auditoria feitas no Windows 2008.  

-   [Fria truques de auditoria no Vista e no 2008](http://blogs.technet.com/b/askds/archive/2007/11/16/cool-auditing-tricks-in-vista-and-2008.aspx) -explica os novos recursos interessantes de auditoria no Windows Vista e Windows Server 2008 que podem ser usados para solução de problemas ou vendo o que está acontecendo em seu ambiente.  

-   [Loja de conveniência para auditoria no Windows Server 2008 e Windows Vista](http://blogs.technet.com/b/askds/archive/2008/03/27/one-stop-shop-for-auditing-in-windows-server-2008-and-windows-vista.aspx) -contém uma compilação de auditoria de recursos e as informações contidas no Windows Server 2008 e Windows Vista.  

-   [Guia passo a passo de auditoria do AD DS](https://technet.microsoft.com/library/a9c25483-89e2-4202-881c-ea8e02b4b2a5.aspx) -descreve o novo recurso de auditoria de serviços de domínio Active Directory (AD DS) no Windows Server 2008. Ele também fornece procedimentos para implementar esse novo recurso.  

## <a name="general-list-of-security-event-id-recommendation-criticalities"></a>Lista geral dos níveis de importância recomendação ID de evento de segurança  
Todas as recomendações de ID de evento são acompanhadas por um nível de importância de classificação da seguinte maneira:  

**Alta:** IDs de eventos com uma classificação de gravidade alta devem sempre e imediatamente ser alertadas e investigadas.  

**Médio:** Uma ID de evento com uma classificação de gravidade média pode indicar atividades mal-intencionadas, mas ele deve ser acompanhado por alguns outra anormalidade (por exemplo, um número incomum que ocorrem em um determinado período de tempo, ocorrências inesperadas ou ocorrências em um computador que normalmente deve não ser esperado para o evento de log.). Um evento de nível de importância Média também poderá r ser coletado como uma métrica e comparado ao longo do tempo.  

**Baixa:** E ID de evento com eventos de um nível de importância baixa não deve coletar atenção ou geram alertas, a menos que correlacionadas com eventos de média ou alta prioridade.  

Essas recomendações destinam-se a fornecer um guia de linha de base para o administrador. Todas as recomendações devem ser inteiramente examinadas antes da implementação em um ambiente de produção.  

Consulte [apêndice l: Eventos a serem monitorados](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md) para obter uma lista dos eventos recomendados para monitorar, suas classificações de nível de importância e um resumo da mensagem de evento.  
