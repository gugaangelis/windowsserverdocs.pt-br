---
title: AD FS de solução de problemas - iniciado pelo Idp de logon
description: Este documento descreve como solucionar problemas na página de entrada do AD FS.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 61e9adc708e95a6ab4a82550280737b2f4bad0a1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844937"
---
# <a name="ad-fs-troubleshooting---idp-initiated-sign-on"></a>AD FS de solução de problemas - iniciado pelo Idp de logon
Página de logon do AD FS pode ser usada para testar se a autenticação está funcionando.  Isso é feito navegando até a página e entrar.  Além disso, podemos usar a página de entrada para verificar se todas as partes de terceira parte confiável do SAML 2.0 estão listadas.

## <a name="enable-the-idp-intiated-sign-on-page"></a>Habilitar a página de logon do Idp iniciada
Por padrão, o AD FS no Windows 2016 não tem o sinal de página habilitada.  Para habilitá-lo, você pode usar o comando do PowerShell Set-AdfsProperties.  Use o procedimento a seguir para habilitar a página:

1.  Abrir do Windows PowerShell
2.  Digite: `Get-AdfsProperties` e pressione enter
3.  Verifique **EnableIdpInitiatedSignonPage** é definido como false ![False](media/ad-fs-tshoot-initiatedsignon/idp2.png)
4.  No PowerShell, digite:  `Set-AdfsProperties -EnableIdpInitiatedSignonPage $true`
5.  Você não verá uma confirmação de então, digite Get-AdfsProperties novamente e verifique **EnableIdpInitatedSignonPage** é definido como true.
![True](media/ad-fs-tshoot-initiatedsignon/idp4.png)

## <a name="test-authentication"></a>Testar autenticação
Use o procedimento a seguir para testar a autenticação do AD FS com o sinal Idp-Initiated na página.

1.  Abra um navegador da web e navegue até a página de logon do Idp.  Exemplo:  https://sts.contoso.com/adfs/ls/idpinitiatedsignon.htm
2.  Você deve ser solicitado a entrar.  Insira suas credenciais.
![logon](media/ad-fs-tshoot-initiatedsignon/idp5.png)
3.  Se isso tiver sido bem-sucedida, você deve estar conectado.


## <a name="test-authentication-using-a-seamless-logon-experience"></a>Testar a autenticação usando uma experiência perfeita de logon
Você pode testar a experiência de logon perfeita, certificando-se de que a URL para seus servidores do AD FS são adicionados a zona de intranet local das suas opções de internet.  Use o seguinte procedimento:

1.  Em um cliente do Windows 10, clique em Iniciar e digite opções da internet e selecione opções da internet.
2.   Clique na guia Segurança, clique em intranet local e clique no botão sites.
![Contínuo](media/ad-fs-tshoot-initiatedsignon/idp8.png)
1.  Clique em Avançado.
2.  Insira a url e clique em Adicionar.  Clique em Fechar.
![Adicionar url](media/ad-fs-tshoot-initiatedsignon/idp9.png)
1.  Clique em Okey.  Clique em Okey.  Isso deve fechar as opções da internet.
2.  Abra um navegador da web e navegue até a página de logon do Idp.  Exemplo:  https://sts.contoso.com/adfs/ls/idpinitiatedsignon.htm
3.  Clique no botão de entrada.  Você deve entrar automaticamente e não ser solicitado a fornecer credenciais.
![Contínuo](media/ad-fs-tshoot-initiatedsignon/idp6.png)

## <a name="next-steps"></a>Próximas etapas

- [Solução de problemas do AD FS](ad-fs-tshoot-overview.md)