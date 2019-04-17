---
ms.assetid: 0f7e56fe-1cbc-43ff-bb87-f4672137ed7f
title: "AD FS e Azure a autenticação multifator"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/09/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 67ce41bbd351bedc8d87e31ddb79c48338e44de9
ms.sourcegitcommit: 877a50cd8d6e727048cdfac9b614a98ac3220876
ms.translationtype: HT
ms.contentlocale: pt-BR
---
# <a name="ad-fs-and-azure-multi-factor-authentication"></a>AD FS e Azure a autenticação multifator

>Aplica-se a: Windows Server 2016

AD FS no Windows Server 2016 apresenta e aproveita os recursos de autenticação multifator que foram introduzidos com o Windows Server 2012 R2.   Introduzindo um adaptador Azure MFA interno, instalação e configuração para usar o Azure MFA como a autenticação principal provedor nunca foi tão simples.

## <a name="why-use-ad-fs-with-azure-mfa"></a>Por que usar o AD FS com MFA do Azure
Atualmente, há algumas limitações com usando uma autenticação segura com o AD FS.  Com o AD FS no Windows Server 2016, elas são resolvidas e as organizações podem aproveitar usuing MFA do Azure com sua implementação do AD FS.  As organizações podem tirar proveito das seguintes opções:

-   Um farm AD FS facilmente pode ser configurado para usar a autenticação multifator com o Azure MFA sem exigir configuração excessiva ou infraestrutura adicional.

-   A experiência interna de MFA é fácil de instalar o Windows Server 2016

-   MFA Azure pode ser configurado como o provedor de autenticação primária possibilita um mecanismo de autenticação alternativo para intranet ou extranet.

## <a name="how-it-works"></a>Como funciona
Em ordem mfa do Azure ser usado como o provedor de autenticação principal, o provedor de autenticação será tratado como o outro integrado provedores.  Ou seja, assim como formulários ou autenticação de certificado.  A única diferença será que o provedor será implementado como um provedor de autenticação externo por meio de uma interface que é exposto somente para o provedor de MFA do Azure para implementação.

A imagem a seguir mostra o fluxo de dados entre o AD FS no Windows Server 2016 e MFA do Azure.

![AD FS e MFA](../media/AD-FS-and-Azure-Multi-Factor-Authentication/ADFS_MFA_1.png)

## <a name="before-you-get-started"></a>Antes de começar
A seguir está uma lista de informações que você deve estar ciente das antes de tentar usar a MFA do Azure como o provedor de autenticação primária.

-   Para configurar o Azure MFA como um método de autenticação primária, você precisa ter o Active Directory do Azure.

-   Configurando o Azure MFA, como o método de autenticação principal é feito usando o Azure AD Connect.  Você deve usar um dos seguintes métodos para configurar e configurá-lo

-   Esse recurso não está disponível no modo misto.  Você o anúncio inteiro farm FS deve ser aWindows Server 2016

## <a name="using-azure-ad-connect-to-setup-azure-mfa-as-primary-authentication-provider"></a>Usando o Azure AD Connect para configurar o Azure MFA como provedor de autenticação principal
Como usar o Azure MFA como seu provedor de autenticação principal é um cenário de híbrido, você pode configurar e configure-o usando o Assistente do Azure AD Connect.  Dependendo se você estiver apenas configurando ou já tem Azure AD Connect no lugar você ainda pode usá-lo para instalar e configurar o AD FS para usar a MFA do Azure.  Use os procedimentos abaixo

### <a name="setting-up-azure-mfa-with-azure-ad-connect---new-installation-of-azure-ad-connect-and-ad-fs"></a>Configurando o MFA do Azure com o Azure AD Connect - nova instalação do Azure AD Connect e AD FS
Se esta for uma nova instalação, simplyIf isso é uma nova instalação, siga as instruções para configurar federação Azure AD Connect e do AD FS que pode ser encontrada [aqui](https://azure.microsoft.com/en-us/documentation/articles/active-directory-aadconnect-get-started-custom/).   Quando você chega à tela Opções de análise, certifique-se de que configurar o Azure MFA do AD FS está marcada e clique em confirmar.   Azure MFA como o provedor de autenticação primária será configurado em automatically.When chegar à tela de opções de análise, certifique-se de que **configurar o Azure MFA do AD FS** está marcada e clique em **confirmar**.   Azure MFA como o provedor de autenticação primária será configurado em automaticamente.

![AD FS e MFA](../media/AD-FS-and-Azure-Multi-Factor-Authentication/ADFS_MFA_2.png)

### <a name="setting-up-azure-mfa-with-azure-ad-connect---new-installation-of-azure-ad-connect-and-existing-ad-fs"></a>Configurando o MFA do Azure com o Azure AD Connect - nova instalação do Azure AD Connect e existente AD FS
Se você já tem a instalação do AD FS e são agora ida para instalar o Azure AD Connect, você precisará basta seguir as instruções para configurar federação Azure AD Connect e do AD FS que pode ser encontrada [aqui](https://azure.microsoft.com/en-us/documentation/articles/active-directory-aadconnect-get-started-custom/) e certifique-se de selecionar um farm de servidores do AD FS existente.   Quando você chega à tela Opções de análise, certifique-se de que **configurar o Azure MFA do AD FS** está marcada e clique em **confirmar.** Azure MFA como o provedor de autenticação primária será configurado em automaticamente.  .

![AD FS e MFA](../media/AD-FS-and-Azure-Multi-Factor-Authentication/ADFS_MFA_3.png)

### <a name="setting-up-azure-mfa-with-azure-ad-connect---existing-azure-ad-connect-and-existing-ad-fs"></a>Configurando o MFA do Azure com o Azure AD Connect - existente Azure AD Connect e existente AD FS
Se você já tiver instalado Azure AD Connect e AD FS, em seguida, basta executar o Assistente do Azure AD Connect novamente nas tarefas adicionais e selecione **configurar o Azure MFA para meu farm AD FS** siga e término da execução por meio do assistente.

![AD FS e MFA](../media/AD-FS-and-Azure-Multi-Factor-Authentication/ADFS_MFA_4.png)

## <a name="setting-up-azure-mfa-with-server-manager-in-windows-server-2016"></a>Configurando o MFA do Azure com o Gerenciador do servidor no Windows Server 2016


