---
title: Solução de problemas de AD FS-emissão de declarações
description: Este documento descreve como solucionar problemas de emissão de token com AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.openlocfilehash: 9eb5ce1ee92e828cc1fd6ceb40ddddec453afe87
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954182"
---
# <a name="ad-fs-troubleshooting---claims-issuance"></a>Solução de problemas de AD FS-emissão de declarações
Uma declaração é uma instrução que um assunto faz sobre si mesmo ou outro assunto.  As declarações são emitidas por uma terceira parte confiável e recebem um ou mais valores e, em seguida, são empacotadas em tokens de segurança que são emitidos pelo servidor de AD FS.  Como há várias partes móveis nesse processo, a emissão de declarações pode ser dividida nessas partes-chave.

>[!NOTE]
>Você pode usar o [ClaimsXRay](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest) no site de [ajuda do ADFS](https://adfshelp.microsoft.com) para ajudar a solucionar problemas de declarações.

## <a name="token-request"></a>Solicitação de token
Quando você for para uma terceira parte confiável, ela será redirecionada para AD FS com uma solicitação de token.  Problemas podem surgir com a solicitação.  Mais notavelmente:

### <a name="the-request-formatting-with-3rd-parties-particularly-saml"></a>A formatação da solicitação com terceiros (particularmente SAML)

### <a name="pre-formated-urls-that-have-typos"></a>URLs previamente formatadas com erros de digitação
Ao emitir um token de terceiras partes confiáveis do WS-Federaion, a solicitação de token vem entre os parâmetros de cadeia de caracteres de consulta de URL.  Se a terceira parte confiável não especificar os parâmetros corretos nessa URL quando ele fizer o redirecionamento para AD FS, isso poderá causar um problema com a solicitação.


Para verificar o formato do token, uma ferramenta de depurador da Web pode ser usada


## <a name="token-response"></a>Resposta de token

## <a name="authentication"></a>Autenticação

## <a name="claim-rule-processing"></a>Processamento de regra de declaração