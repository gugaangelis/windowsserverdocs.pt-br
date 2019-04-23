---
title: Solução de problemas - Fiddler - WS-Federation do AD FS
description: Este documento mostra um rastreamento detalhado de uma troca de WS-Federation com o AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/18/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: be1c9f466ec13272d10f0fb9ca31cf326a1ec29a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846897"
---
# <a name="ad-fs-troubleshooting---fiddler---ws-federation"></a>Solução de problemas - Fiddler - WS-Federation do AD FS
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler9.png)

## <a name="step-1-and-2"></a>Etapa 1 e 2
Este é o início de nosso rastreamento.  Nesse quadro, podemos ver o seguinte: ![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler1.png)

Solicitação:

- HTTP GET para nossa terceira parte confiável (de terceiros http://sql1.contoso.com/SampApp)

Resposta:

- A resposta é um HTTP 302 (redirecionamento).  Os dados de transporte no cabeçalho de resposta mostram para onde redirecionar como (https://sts.contoso.com/adfs/ls)
- A URL de redirecionamento contém wa = wsignin 1.0 que nos diz que nosso aplicativo de RP tem criado de uma solicitação de logon do WS-Federation para nós e enviar este /adfs/ls/ponto de extremidade do AD FS.  Isso é conhecido como redirecionamento de associação.
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler2.png)

## <a name="step-3-and-4"></a>Etapa 3 e 4

![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler3.png)

Solicitação:

- HTTP GET para nosso server(sts.contoso.com) do AD FS

Resposta:

- A resposta é um prompt para credenciais.  Isso indica que estamos usando uma autenticação de formulários
- Clicando no WebView da resposta você pode ver as credenciais de prompt.
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler6.png)

## <a name="step-5-and-6"></a>Etapa 5 e 6

![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler4.png)

Solicitação:

- HTTP POST com nosso nome de usuário e senha.  
- Apresentamos nossas credenciais.  Examinando os dados brutos na solicitação, podemos ver as credenciais

Resposta:

- A resposta é encontrado e o MSIAuth cookie criptografado é criado e retornado.  Isso é usado para validar a declaração SAML produzida pelo nosso cliente.  Isso também é conhecido como "cookie de autenticação" e só estará presente quando o AD FS é o Idp.


## <a name="step-7-and-8"></a>Etapa 7 e 8
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler5.png)

Solicitação:

- Agora que podemos ter autenticado podemos fazer outra HTTP GET para o servidor do AD FS e apresentar nosso token de autenticação

Resposta:

- A resposta é um Okey HTTP, que significa que o AD FS tiver autenticado o usuário com base nas credenciais fornecidas
- Além disso, definimos 3 cookies para o cliente
    - MSISAuthenticated contém um valor de carimbo de hora codificada em base64 para quando o cliente foi autenticado.
    - MSISLoopDetectionCookie é usado pelo mecanismo de detecção de loop infinito do AD FS para clientes de palavras irrelevantes que acabaram em um loop de redirecionamento infinito para o servidor de Federação. Os dados do cookie serão um carimbo de hora que é codificado em base64.
    - MSISSignout é usado para controlar o IdP e todas as RPs visitado para a sessão SSO. Esse cookie é utilizado quando uma saída de WS-Federation é invocado. Você pode ver o conteúdo desse cookie usando um decodificador base64.
    
## <a name="step-9-and-10"></a>Etapa 9 e 10
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler7.png) Solicitação:

- HTTP POST

Resposta:

- A resposta é um encontrado

## <a name="step-11-and-12"></a>Etapa 11 e 12
![](media/ad-fs-tshoot-fiddler-ws-fed/fiddler8.png) Solicitação:

- HTTP GET

Resposta:

- A resposta é Okey

## <a name="next-steps"></a>Próximas etapas

- [Solução de problemas do AD FS](ad-fs-tshoot-overview.md)