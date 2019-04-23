---
title: Emissão de declarações do AD FS de solução de problemas-
description: Este documento descreve como solucionar problemas de emissão de token com o AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fdf8851fe9b35f82191458ba3313fda2dc3ee4cf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839657"
---
# <a name="ad-fs-troubleshooting---claims-issuance"></a>Emissão de declarações do AD FS de solução de problemas-
Uma declaração é uma instrução que um sujeito faz sobre si mesmo ou outro assunto.  Declarações emitidas por uma terceira parte confiável e são determinados valores de um ou mais e, em seguida, empacotados em tokens de segurança emitidos pelo servidor do AD FS.  Como há várias partes móveis nesse processo, emissão de declarações pode ser dividido nessas partes-chave.

>[!NOTE]  
>Você pode usar [ClaimsXRay](https://adfshelp.microsoft.com/ClaimsXray/TokenRequest) sobre o [ADFS ajuda](https://adfshelp.microsoft.com) site para ajudar a solucionar problemas de declarações.   

## <a name="token-request"></a>Solicitação de token
Quando você acessa uma terceira ele redirecionará você para o AD FS com uma solicitação de token.  Problemas podem surgir com a solicitação.  Mais notoriamente:

### <a name="the-request-formatting-with-3rd-parties-particularly-saml"></a>A solicitação de formatação com partes 3ª (particularmente SAML)

### <a name="pre-formated-urls-that-have-typos"></a>Pré-formatada URLs que têm erros de digitação
Ao emitir um token do WS-Federaion terceiras essa solicitação de token é fornecido com parâmetros de cadeia de caracteres de consulta de URL.  Se a terceira parte confiável não especifica os parâmetros corretos na URL quando ele faz o redirecionamento para o AD FS, em seguida, ele pode causar um problema com a solicitação.


Em ordem para verificar o formato do token, uma ferramenta do depurador da web pode ser usada.


## <a name="token-response"></a>Resposta de token

## <a name="authentication"></a>Autenticação

## <a name="claim-rule-processing"></a>Processamento da regra de declaração