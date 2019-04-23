---
title: Solução de problemas do AD FS
description: Este documento descreve como solucionar problemas de vários aspectos do AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/12/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 60ecf94b72e58aed4d3718b19f6007cdad1c9578
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840947"
---
# <a name="troubleshooting-ad-fs"></a>Solução de problemas do AD FS
O AD FS tem muitas partes móveis, toca muitas coisas diferentes e tem muitas dependências diferentes.  Naturalmente, isso pode dar origem a vários problemas.  Este documento foi criado para você começar na resolução desses problemas.  Este documento apresenta a você as áreas típicas que você deve se concentrar em como habilitar recursos para várias ferramentas que podem ser usadas para rastrear problemas e informações adicionais.  

>[!NOTE]
>Para obter mais informações, consulte [ADFS ajuda](http://adfshelp.microsoft.com) que fornece ferramentas eficientes em um coloque que torna mais fácil para os usuários e administradores a resolver problemas de autenticação em um ritmo mais rápido. 


## <a name="what-to-check-first"></a>O que verificar primeiro
Antes de você se aprofundar em solução de problemas detalhada, há algumas coisas que você deve verificar primeiro.  São eles:
- **Configuração de DNS** -você pode resolver o nome do serviço de Federação?  Isso deve resolver para endereço IP de qualquer um do balanceador de carga ou o endereço IP de um dos servidores do AD FS no farm.  Para obter mais informações, consulte [AD FS de solução de problemas - DNS](ad-fs-tshoot-dns.md).
- **Pontos de extremidade do AD FS** -você pode navegar para os pontos de extremidade do AD FS?  Navegando para isso, você pode determinar se o servidor da web do AD FS está respondendo às solicitações.  Se você pode obter a esse arquivo, você sabe que o AD FS está atendendo às solicitações em 443 bem.  Para obter mais informações, consulte [AD FS de solução de problemas - pontos de extremidade](ad-fs-tshoot-endpoints.md).
- **Iniciado por IDP logon** -pode fazer logon e autenticar por meio da página Idp-Initiated logon?  Você precisa garantir que esta página foi habilitada porque ela está desabilitada por padrão.  Use `Set-AdfsProperties -EnableIdPInitiatedSignOn $true` para habilitar a página.  Se você pode entrar e se autenticar, em seguida, você sabe que o AD FS está funcionando nessa área.  Para obter mais informações, consulte [AD FS de solução de problemas - logon](ad-fs-tshoot-initiatedsignon.md).
##  <a name="common-troubleshooting-areas"></a>Áreas de solução de problemas comuns

|Nome|Descrição|
|-----|-----|
|[Auditoria e log de eventos](ad-fs-tshoot-logging.md)|Use os Logs de eventos do Windows para exibir o nível e baixa informações de alto nível por meio dos logs de rastreamento e de administrador.  Ele também pode ser usado para exibir a auditoria de segurança.|
|[Conectividade do SQL](ad-fs-tshoot-sql.md)|Informações sobre como testar a conectividade entre servidores do AD FS e os bancos de dados do SQL de back-end|
|[Emissão de declarações](ad-fs-tshoot-claims-issuance.md)|Informações sobre como determinar se o AD FS é emitir declarações corretamente.|
|[Detecção de loop](ad-fs-tshoot-loop.md)|Informações sobre como determinar e impedindo que os usuários que estão sendo devolvida e para trás entre o Idp e uma RP.|
|[Certificados](ad-fs-tshoot-certs.md)|Problemas de certificado Typcial que podem surgir|
|[Fiddler](ad-fs-tshoot-fiddler.md)|Informações sobre como instalar e usar o Fiddler|
|[WS-Federation com o Fiddler](ad-fs-tshoot-fiddler-ws-fed.md)|Rastreamento do Fiddler detalhado de uma interação de WS-Federation|
|[Regras de declaração](ad-fs-tshoot-claims-rules.md)|Informações sobre como solucionar problemas de regras de declaração e sua sintaxe|
|[Autenticação integrada do Windows](ad-fs-tshoot-iwa.md)|Informações sobre como solucionar problemas de autenticação integrada.|
|[Azure AD](ad-fs-tshoot-azure.md)|Informações sobre como solucionar problemas de interação de AD FS com o Azure AD.|
|[Analisador do AD FS diagnóstico](ad-fs-diagnostics-analyzer.md)|Analisador de diagnóstico Ajuda do AD FS pode ajudar a executar verificações básicas do AD FS usando o módulo do PowerShell de diagnóstico. 