---
title: Solução de problemas AD FS-logon iniciado pelo IDP
description: Este documento descreve como solucionar problemas da página de logon do AD FS.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 89ee45bd0387cf728bc126529169b6b1ca045d6f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366195"
---
# <a name="ad-fs-troubleshooting---idp-initiated-sign-on"></a>Solução de problemas AD FS-logon iniciado pelo IDP
A página de logon AD FS pode ser usada para testar se a autenticação está funcionando ou não.  Isso é feito navegando até a página e entrando.  Além disso, podemos usar a página de entrada para verificar se todas as partes confiáveis SAML 2,0 estão listadas.

## <a name="enable-the-idp-initiated-sign-on-page"></a>Habilitar a página de logon iniciada pelo IDP
Por padrão, AD FS no Windows 2016 não tem a página de logon habilitada.  Para habilitá-lo, você pode usar o comando Set-Adfsproperties do PowerShell.  Use o procedimento a seguir para habilitar a página:

1.  Abrir o Windows PowerShell
2.  Insira: `Get-AdfsProperties` e pressione Enter
3.  Verifique se **EnableIdpInitiatedSignonPage** está definido como false ![false](media/ad-fs-tshoot-initiatedsignon/idp2.png)
4.  No PowerShell, digite: `Set-AdfsProperties -EnableIdpInitiatedSignonPage $true`
5.  Você não verá uma confirmação, portanto, digite Get-Adfsproperties novamente e verifique se **EnableIdpInitatedSignonPage** está definido como true.
![verdadeiro](media/ad-fs-tshoot-initiatedsignon/idp4.png)

## <a name="test-authentication"></a>Testar autenticação
Use o procedimento a seguir para testar AD FS autenticação com a página de logon iniciada pelo IDP.

1.  Abra um navegador da Web e navegue até a página de logon do IDP.  Exemplo: https://sts.contoso.com/adfs/ls/idpinitiatedsignon.htm
2.  Você deve ser solicitado a entrar.  Insira suas credenciais.
![logon](media/ad-fs-tshoot-initiatedsignon/idp5.png)
3.  Se isso tiver sido bem-sucedido, você deverá estar conectado.


## <a name="test-authentication-using-a-seamless-logon-experience"></a>Testar a autenticação usando uma experiência de logon contínuo
Você pode testar a experiência de logon contínuo, certificando-se de que a URL para seus servidores de AD FS são adicionadas à zona da intranet local de suas opções da Internet.  Use o seguinte procedimento:

1.  Em um cliente do Windows 10, clique em Iniciar e digite opções da Internet e selecione opções da Internet.
2.   Clique na guia Segurança, clique em intranet local e clique no botão sites.
![](media/ad-fs-tshoot-initiatedsignon/idp8.png) contínuo
1.  Clique em Avançado.
2.  Insira sua URL e clique em Adicionar.  Clique em fechar.
![Adicionar URL](media/ad-fs-tshoot-initiatedsignon/idp9.png)
1.  Clique em OK.  Clique em OK.  Isso deve fechar as opções da Internet.
2.  Abra um navegador da Web e navegue até a página de logon do IDP.  Exemplo: https://sts.contoso.com/adfs/ls/idpinitiatedsignon.htm
3.  Clique no botão entrar.  Você deve entrar automaticamente e não será solicitado a fornecer as credenciais.
![](media/ad-fs-tshoot-initiatedsignon/idp6.png) contínuo

## <a name="next-steps"></a>Próximas etapas

- [Solução de problemas do AD FS](ad-fs-tshoot-overview.md)
