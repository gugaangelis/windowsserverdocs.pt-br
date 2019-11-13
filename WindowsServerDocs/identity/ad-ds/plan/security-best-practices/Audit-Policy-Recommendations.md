---
ms.assetid: 0abe0976-4b49-45d6-a7b3-81d28bdb8210
title: Recomendações de política de auditoria
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: c902f9a438afd226dfb58dbde9f3c00d57b0ed58
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408592"
---
# <a name="audit-policy-recommendations"></a>Recomendações de política de auditoria

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10, Windows 8.1, Windows 7

Esta seção aborda as configurações da política de auditoria padrão do Windows, as configurações de política de auditoria recomendadas e as recomendações mais agressivas da Microsoft para produtos de estação de trabalho e de servidor.  

As recomendações de linha de base do SCM mostradas aqui, junto com as configurações que recomendamos para ajudar a detectar o comprometimento, devem ser apenas um guia de linha de base inicial para os administradores. Cada organização deve tomar suas próprias decisões sobre as ameaças que enfrentam, suas tolerâncias de risco aceitáveis e quais categorias ou subcategorias de diretiva de auditoria eles devem habilitar. Para obter mais informações sobre ameaças, consulte o [Guia de ameaças e contramedidas](https://technet.microsoft.com/library/hh125921(v=ws.10).aspx). Os administradores sem uma diretiva de auditoria cuidadosa em vigor são incentivados a começar com as configurações recomendadas aqui e, em seguida, modificar e testar, antes de implementar em seu ambiente de produção.  

As recomendações são para computadores de classe empresarial, que a Microsoft define como computadores que têm requisitos de segurança médios e exigem um alto nível de funcionalidade operacional. As entidades que precisam de requisitos de segurança mais altos devem considerar políticas de auditoria mais agressivas.  

> [!NOTE]  
> Os padrões do Microsoft Windows e as recomendações de linha de base foram tirados da [ferramenta Microsoft Security Compliance Manager](https://technet.microsoft.com/library/cc677002.aspx).  

As configurações de política de auditoria de linha de base a seguir são recomendadas para computadores de segurança normal que não são conhecidos em ativos, ataques bem-sucedidos por determinados adversários ou malware.  

## <a name="recommended-audit-policies-by-operating-system"></a>Políticas de auditoria recomendadas por sistema operacional  
Esta seção contém tabelas que listam as recomendações de configuração de auditoria que se aplicam aos seguintes sistemas operacionais:  

-   Windows Server 2016 

-   Windows Server 2012  

-   Windows Server 2012 R2  

-   Windows Server 2008  

-   Windows 10

-   Windows 8.1  

-   Windows 7  

Essas tabelas contêm a configuração padrão do Windows, as recomendações de linha de base e as recomendações mais fortes para esses sistemas operacionais.  

**Legenda das tabelas de política de auditoria**  

|||  
|-|-|  
|**Anotações**|**Recomendação**|  
|SIM|Habilitar em cenários gerais|  
|NÃO|Não **habilitar em** cenários gerais|  
|QUE|Habilitar, se necessário, para um cenário específico, ou se uma função ou recurso para o qual a auditoria é desejada estiver instalado no computador|  
|DC|Habilitar em controladores de domínio|  
|Ficará|Nenhuma recomendação|  

**Recomendações de configurações de auditoria do Windows 10, do Windows 8 e do Windows 7**  

**Política de auditoria**  

|Categoria ou subcategoria da política de auditoria|Padrão do Windows<br /><br />Falha de êxito|Recomendação de linha de base<br /><br />Falha de êxito|Recomendação mais forte<br /><br />Falha de êxito|  
|----------------------------------------|------------------------------------------|--------------------------------------------------|--------------------------------------------------|  
|**Logon da conta**||||  
|Validação de Credenciais de Auditoria|Não|Sim não|Sim Sim|  
|Auditoria do serviço de autenticação Kerberos|||Sim Sim|  
|Auditoria das operações do tíquete de serviço Kerberos|||Sim Sim|  
|Auditoria de outros eventos de logon de conta|||Sim Sim|  
|**Gerenciamento de contas**||||  
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
|**Acesso ao DS**||||  
|Auditoria de Replicação Detalhada do Serviço de Diretório||||  
|Auditoria do acesso ao serviço de diretório||||  
|Auditoria de Alterações no Serviço de Diretório||||  
|Auditoria de Replicação do Serviço de Diretório||||  
|**Logon e logoff**||||  
|Auditoria de Bloqueio de Conta|Sim não||Sim não|  
|Auditoria das declarações de dispositivo/usuário||||  
|Auditoria de Modo Estendido do IPsec||||  
|Auditoria de Modo Principal do IPsec|||SE FOR|  
|Auditoria de Modo Rápido do IPsec||||  
|Logoff de Auditoria|Sim não|Sim não|Sim não|  
|Logon de auditoria <sup>1</sup>|Sim Sim|Sim Sim|Sim Sim|  
|Auditoria de Servidor de Política de Rede|Sim Sim|||  
|Auditoria de outros eventos de logon/logoff||||  
|Auditoria de Logon Especial|Sim não|Sim não|Sim Sim|  
|**Acesso a objeto**||||  
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
|**Alteração de política**||||  
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
|**Auditoria de acesso a objetos globais**||||  
|Auditoria do driver IPsec||||  
|Auditoria de outros eventos do sistema||||  
|Auditoria de Alteração no Estado de Segurança||||  
|Auditoria de Extensão do Sistema de Segurança||||  
|Auditoria da integridade do sistema||||  

<sup>1</sup> a partir do Windows 10 versão 1809, o logon de auditoria é habilitado por padrão para êxito e falha. Nas versões anteriores do Windows, apenas êxito é habilitado por padrão.

**Recomendações de configurações de auditoria do Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008**  

|Categoria ou subcategoria da política de auditoria|Padrão do Windows<br /><br />Falha de êxito|Recomendação de linha de base<br /><br />Falha de êxito|Recomendação mais forte<br /><br />Falha de êxito|  
|----------------------------------------|------------------------------------------|--------------------------------------------------|--------------------------------------------------|  
|**Logon da conta**||||  
|Validação de Credenciais de Auditoria|Não|Sim Sim|Sim Sim|  
|Auditoria do serviço de autenticação Kerberos|||Sim Sim|  
|Auditoria das operações do tíquete de serviço Kerberos|||Sim Sim|  
|Auditoria de outros eventos de logon de conta|||Sim Sim|  
|**Gerenciamento de contas**||||  
|Auditoria do gerenciamento de grupo de aplicativos||||  
|Auditoria de Gerenciamento de Conta de Computador||Sim DC|Sim Sim|  
|Auditoria de Gerenciamento de Grupo de Distribuição||||  
|Auditoria de Outros Eventos de Gerenciamento de Contas||Sim Sim|Sim Sim|  
|Auditoria de Gerenciamento de Grupo de Segurança||Sim Sim|Sim Sim|  
|Auditoria de Gerenciamento de Conta de Usuário|Sim não|Sim Sim|Sim Sim|  
|**Acompanhamento detalhado**||||  
|Auditoria de Atividade DPAPI|||Sim Sim|  
|Auditoria de Criação de Processo||Sim não|Sim Sim|  
|Auditoria de Terminação de Processo||||  
|Auditoria de Eventos de RPC||||  
|**Acesso ao DS**||||  
|Auditoria de Replicação Detalhada do Serviço de Diretório||||  
|Auditoria do acesso ao serviço de diretório||CONTROLADOR DE DOMÍNIO DC|CONTROLADOR DE DOMÍNIO DC|  
|Auditoria de Alterações no Serviço de Diretório||CONTROLADOR DE DOMÍNIO DC|CONTROLADOR DE DOMÍNIO DC|  
|Auditoria de Replicação do Serviço de Diretório||||  
|**Logon e logoff**||||  
|Auditoria de Bloqueio de Conta|Sim não||Sim não|  
|Auditoria das declarações de dispositivo/usuário||||  
|Auditoria de Modo Estendido do IPsec||||  
|Auditoria de Modo Principal do IPsec|||SE FOR|  
|Auditoria de Modo Rápido do IPsec||||  
|Logoff de Auditoria|Sim não|Sim não|Sim não|  
|Logon de Auditoria|Sim Sim|Sim Sim|Sim Sim|  
|Auditoria de Servidor de Política de Rede|Sim Sim|||  
|Auditoria de outros eventos de logon/logoff|||Sim Sim|  
|Auditoria de Logon Especial|Sim não|Sim não|Sim Sim|  
|**Acesso a objeto**||||  
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
|**Alteração de política**||||  
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
|**Auditoria de acesso a objetos globais**||||  
|Auditoria do driver IPsec||||  
|Auditoria de outros eventos do sistema||||  
|Auditoria de Alteração no Estado de Segurança||||  
|Auditoria de Extensão do Sistema de Segurança||||  
|Auditoria da integridade do sistema||||  

## <a name="set-audit-policy-on-workstations-and-servers"></a>Definir política de auditoria em estações de trabalho e servidores  
Todos os planos de gerenciamento de log de eventos devem monitorar estações de trabalho e servidores. Um erro comum é monitorar apenas os servidores ou controladores de domínio. Como o ataque mal-intencionado geralmente ocorre em estações de trabalho, não o monitoramento de estações de trabalho está ignorando a melhor e mais antiga fonte de informações.  

Os administradores devem revisar e testar uma diretiva de auditoria de uma consideração antes da implementação em seu ambiente de produção.  

## <a name="events-to-monitor"></a>Eventos a Monitorar  
Uma ID de evento perfeita para gerar um alerta de segurança deve conter os seguintes atributos:  

-   Alta probabilidade de que a ocorrência indique atividade não autorizada  

-   Número baixo de falsos positivos  

-   A ocorrência deve resultar em uma resposta investigativa/forense  

Dois tipos de eventos devem ser monitorados e alertados:  

1.  Esses eventos em que mesmo uma única ocorrência indica atividade não autorizada  

2.  Um acúmulo de eventos acima de uma linha de base esperada e aceita  

Um exemplo do primeiro evento é:  

Se os administradores de domínio (DAs) forem proibidos de fazer logon em computadores que não são controladores de domínio, uma única ocorrência de um membro do dos que faz logon em uma estação de trabalho do usuário final deverá gerar um alerta e ser investigado. Esse tipo de alerta é fácil de gerar usando o evento de logon especial de auditoria 4964 (grupos especiais foram atribuídos a um novo logon). Outros exemplos de alertas de instância única incluem:  

-   Se o servidor A nunca deve se conectar ao servidor B, alertar quando eles se conectarem entre si.  

-   Alertar se uma conta de usuário final normal for adicionada inesperadamente a um grupo de segurança confidencial.  

-   Se os funcionários na localização de fábrica A nunca funcionarem à noite, alerte quando um usuário fizer logon à meia-noite.  

-   Alertar se um serviço não autorizado estiver instalado em um controlador de domínio.  

-   Investigue se um usuário final regular tenta fazer logon diretamente em um SQL Server para o qual não há motivo claro para isso.  

-   Se você não tiver membros em seu grupo de DA, e alguém se adicionar, verifique-o imediatamente.  

Um exemplo do segundo evento é:  

Um número Aberrant de logons com falha pode indicar um ataque de adivinhação de senha. Para que uma empresa forneça um alerta para um número excepcionalmente alto de logons com falha, eles devem primeiro compreender os níveis normais de logons com falha em seu ambiente antes de um evento de segurança mal-intencionado.  

Para obter uma lista abrangente de eventos que você deve incluir ao monitorar sinais de comprometimento, consulte o [Apêndice L: eventos a serem monitorados](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md).  

## <a name="active-directory-objects-and-attributes-to-monitor"></a>Active Directory objetos e atributos a serem monitorados  
A seguir estão as contas, os grupos e os atributos que você deve monitorar para ajudá-lo a detectar tentativas de comprometer sua instalação de Active Directory Domain Services.  

-   Sistemas para desabilitar ou remover software antivírus e antimalware (reiniciar automaticamente a proteção quando ele é desabilitado manualmente)  

-   Contas de administrador para alterações não autorizadas  

-   Atividades executadas usando contas com privilégios (remover automaticamente a conta quando atividades suspeitas forem concluídas ou o tempo alocado tiver expirado)  

-   Contas com e com privilégios de VIP no AD DS. Monitore alterações, especialmente alterações em atributos na guia conta (por exemplo, CN, nome, sAMAccountName, userPrincipalName ou userAccountControl). Além de monitorar as contas, restrinja quem pode modificar as contas para o menor conjunto de usuários administrativos possível.  

Consulte o [Apêndice L: eventos para monitorar](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md) uma lista de eventos recomendados a serem monitorados, suas classificações de criticalidade e um resumo de mensagem de evento.  

-   Agrupe os servidores pela classificação de suas cargas de trabalho, o que permite identificar rapidamente os servidores que devem ser mais bem monitorados e mais rigorosamente configurados  

-   Alterações nas propriedades e Associação dos seguintes grupos de AD DS: administradores corporativos (EA), administradores de domínio (DA), administradores (BA) e administradores de esquema (SA)  

-   Contas com privilégios desabilitadas (como contas de administrador internas no Active Directory e em sistemas Membros) para habilitar as contas  

-   Contas de gerenciamento para registrar em log todas as gravações na conta  

-   Assistente de configuração de segurança interna para configurar o serviço, o registro, a auditoria e as configurações de firewall para reduzir a superfície de ataque do servidor. Use este assistente se você implementar servidores de salto como parte da sua estratégia de host administrativo.  

## <a name="additional-information-for-monitoring-active-directory-domain-services"></a>Informações adicionais para monitoramento Active Directory Domain Services  
Examine os links a seguir para obter informações adicionais sobre o monitoramento AD DS:  
  
-   A [auditoria de acesso a objetos globais é mágica](http://blogs.technet.com/b/askds/archive/2011/03/10/global-object-access-auditing-is-magic.aspx) -fornece informações sobre como configurar e usar a configuração avançada da política de auditoria que foi adicionada ao Windows 7 e ao windows Server 2008 R2.  

-   [Apresentando alterações de auditoria no windows 2008](http://blogs.technet.com/b/askds/archive/2007/10/19/introducing-auditing-changes-in-windows-2008.aspx) -apresenta as alterações de auditoria feitas no Windows 2008.  

-   [Truques de auditoria interessantes no Vista e 2008](http://blogs.technet.com/b/askds/archive/2007/11/16/cool-auditing-tricks-in-vista-and-2008.aspx) -explica os novos recursos interessantes de auditoria no Windows Vista e no windows Server 2008 que podem ser usados para solucionar problemas ou ver o que está acontecendo em seu ambiente.  

-   A [loja única para auditoria no Windows server 2008 e no Windows Vista](http://blogs.technet.com/b/askds/archive/2008/03/27/one-stop-shop-for-auditing-in-windows-server-2008-and-windows-vista.aspx) -contém uma compilação de recursos de auditoria e informações contidas no windows Server 2008 e no Windows Vista.  

-   [Guia passo a passo de AD DS auditoria](https://technet.microsoft.com/library/a9c25483-89e2-4202-881c-ea8e02b4b2a5.aspx) – descreve o novo recurso de auditoria de Active Directory Domain Services (AD DS) no Windows Server 2008. Ele também fornece procedimentos para implementar esse novo recurso.  

## <a name="general-list-of-security-event-id-recommendation-criticalities"></a>Lista geral de Criticalidades de recomendação de ID de evento de segurança  
Todas as recomendações de ID de evento são acompanhadas por uma classificação de criticalidade da seguinte maneira:  

**Alta:** As IDs de evento com uma classificação de criticalidade alta devem ser sempre e imediatamente alertados e investigados.  

**Médio:** Uma ID de evento com uma classificação de criticalidade média pode indicar atividades mal-intencionadas, mas deve ser acompanhada por alguma outra anormalidade (por exemplo, um número incomum ocorrendo em um período de tempo específico, ocorrências inesperadas ou ocorrências em um computador que normalmente não seria esperado para registrar o evento.). Um evento de criticalidade médio também pode ser coletado como uma métrica e comparado ao longo do tempo.  

**Baixo:** E a ID do evento com poucos eventos de criticalidade não devem obter atenção ou causar alertas, a menos que correlacionem-se com eventos de criticalidade médios ou altos.  

Essas recomendações destinam-se a fornecer um guia de linha de base para o administrador. Todas as recomendações devem ser examinadas minuciosamente antes da implementação em um ambiente de produção.  

Consulte o [Apêndice L: eventos para monitorar](../../../ad-ds/plan/Appendix-L--Events-to-Monitor.md) uma lista dos eventos recomendados a serem monitorados, suas classificações de criticalidade e um resumo de mensagem de evento.  
