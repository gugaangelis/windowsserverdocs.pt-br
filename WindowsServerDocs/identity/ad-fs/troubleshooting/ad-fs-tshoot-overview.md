---
title: Solução de problemas do AD FS
description: Este documento descreve como solucionar problemas de vários aspectos do AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/12/2018
ms.topic: article
ms.openlocfilehash: ba18341448720403f1572ee485ec33bec84b4324
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87953065"
---
# <a name="troubleshooting-ad-fs"></a>Solução de problemas do AD FS
AD FS tem muitas partes móveis, apresenta muitas coisas diferentes e tem várias dependências diferentes.  Naturalmente, isso pode dar ao aumento de vários problemas.  Este documento foi projetado para ajudá-lo a começar a solucionar esses problemas.  Este documento apresentará a você as áreas típicas nas quais você deve se concentrar, como habilitar recursos para informações adicionais e várias ferramentas que podem ser usadas para rastrear problemas.

>[!NOTE]
>Para obter informações adicionais, consulte [ajuda do ADFS](https://adfshelp.microsoft.com) que fornece ferramentas efetivas em um único lugar que facilita para os usuários e administradores resolver problemas de autenticação em um ritmo mais rápido.


## <a name="what-to-check-first"></a>O que verificar primeiro
Antes de se aprofundar na solução de problemas aprofundada, há algumas coisas que você deve verificar primeiro.  Eles são:
- **Configuração de DNS** -você pode resolver o nome do serviço de Federação?  Isso deve ser resolvido para o endereço IP do balanceador de carga ou para o endereço IP de um dos servidores de AD FS no seu farm.  Para obter mais informações [, consulte AD FS solução de problemas-DNS](ad-fs-tshoot-dns.md).
- **AD FS pontos de extremidade** – você pode navegar até os pontos de extremidade de AD FS?  Ao navegar até isso, você pode determinar se seu servidor Web AD FS está respondendo às solicitações.  Se você puder chegar a esse arquivo, saberá que AD FS está atendendo solicitações acima de 443.  Para obter mais informações [, consulte AD FS solução de problemas-pontos de extremidade](ad-fs-tshoot-endpoints.md).
- **Logon iniciado pelo IDP** -você pode fazer logon e autenticar por meio da página de logon iniciada pelo IDP?  Você precisa garantir que essa página tenha sido habilitada porque está desabilitada por padrão.  Use `Set-AdfsProperties -EnableIdPInitiatedSignOn $true` para habilitar a página.  Se você puder entrar e autenticar, saberá que AD FS está funcionando nessa área.  Para obter mais informações [, consulte AD FS solução de problemas-conexão](ad-fs-tshoot-initiatedsignon.md).
  ##  <a name="common-troubleshooting-areas"></a>Áreas de solução de problemas comuns

|Nome|Descrição|
|-----|-----|
|[Log de eventos e auditoria](ad-fs-tshoot-logging.md)|Use os logs de eventos do Windows para exibir informações de alto nível e de nível baixo por meio dos logs de administração e de rastreamento.  Ele também pode ser usado para exibir a auditoria de segurança.|
|[Conectividade do SQL](ad-fs-tshoot-sql.md)|Informações sobre como testar a conectividade entre seus servidores de AD FS e os bancos de dados SQL de back-end|
|[Emissão de declarações](ad-fs-tshoot-claims-issuance.md)|Informações sobre como determinar se AD FS está emitindo declarações corretamente.|
|[Detecção de loop](ad-fs-tshoot-loop.md)|Informações sobre como determinar e impedir que os usuários sejam devolvidos entre o IDP e um RP.|
|[Certificados](ad-fs-tshoot-certs.md)|Problemas de certificado Typcial que podem surgir|
|[Fiddler](ad-fs-tshoot-fiddler.md)|Informações sobre como instalar e usar o Fiddler|
|[WS-Federation com Fiddler](ad-fs-tshoot-fiddler-ws-fed.md)|Rastreamento de Fiddler detalhado de uma interação do WS-Federation|
|[Regras de declaração](ad-fs-tshoot-claims-rules.md)|Informações sobre como solucionar problemas de regras de declaração e sua sintaxe|
|[Autenticação Integrada do Windows](ad-fs-tshoot-iwa.md)|Informações sobre como solucionar problemas de autenticação integrada.|
|[Azure AD](ad-fs-tshoot-azure.md)|Informações sobre como solucionar problemas de AD FS interação com o Azure AD.|
|[Analisador de diagnóstico de AD FS](ad-fs-diagnostics-analyzer.md)|AD FS o help Diagnostics Analyzer pode ajudar a executar verificações básicas de AD FS usando o módulo diagnóstico do PowerShell.