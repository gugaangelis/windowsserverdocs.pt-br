---
ms.assetid: 299e4fb9-8f1a-4275-ad7d-dad4f1594657
title: Passo a passo - ingresso no local de trabalho com um dispositivo iOS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0a8643bab913dfec07c6bbea0c068e1240f16c5b
ms.sourcegitcommit: a2260c96b0e49519d180c7382b921ce8ddb053fe
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/04/2018
---
# <a name="walkthrough-workplace-join-with-an-ios-device"></a>Passo a passo: Workplace Join com um dispositivo iOS

>Aplica-se a: Windows Server 2012 R2

Este tópico demonstra Workplace Join em um dispositivo iOS. Você deve concluir as etapas a [configurar o ambiente de laboratório do AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md) seção antes de você pode experimentar este passo a passo. Você pode usar o dispositivo para acessar o mesmo aplicativo da web de empresa que acessada no [passo a passo: Workplace Join com um dispositivo Windows](Walkthrough--Workplace-Join-with-a-Windows-Device.md).

## <a name="join-an-ios-device-with-workplace-join"></a>Ingressar um dispositivo iOS com o ingresso no local de trabalho

> [!IMPORTANT]
> Quando DRS local é configurado, o dispositivo iOS deve confiar no certificado de camada de soquete segura (SSL) que foi usado para configurar os serviços de Federação do Active Directory (AD FS) em [etapa 2: configurar o servidor de Federação (ADFS1) com o serviço de registro de dispositivo](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4), para Workplace Join ter sucesso.
> 
> -   Se o certificado SSL do AD FS foi emitido por uma autoridade de certificação (CA) do teste, você deve instalar o certificado de autoridade de certificação em seu dispositivo iOS.
> -   Se o certificado de autoridade de certificação é publicado em um site, você pode procurar o site do seu dispositivo iOS e instalar o certificado.

Nesta demonstração, ingressar o dispositivo para a área de trabalho.

#### <a name="to-join-an-ios-device-to-a-workplace"></a>Para participar de um dispositivo iOS para um local de trabalho

1.  -   **Quando o serviço de registro de dispositivo do Azure Active Directory é o DRS configurado:** abrir Apple Safari e navegue até o ponto de extremidade do perfil OTA de serviço de registro de dispositivo do Azure Active Directory para dispositivos iOS, <`https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/<yourdomainname` > onde <`yourdomainname`> é o nome de domínio que você tenha configurado com o Active Directory do Azure. Por exemplo, se o nome de domínio for contoso.com, a URL seria: `https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com`

    -   **Quando o local DRS é o DRS configurado**: abrir Apple Safari e navegue até o ponto de extremidade de serviço de registro de dispositivo (DRS) OTA perfil para dispositivos iOS,`https://adf1s.contoso.com/enrollmentserver/otaprofile`

    Há muitas maneiras de se comunicar essa URL para seus usuários. Uma maneira recomendada é publicar a URL em um aplicativo personalizado acesso negado mensagem no AD FS. Isso é abordado na seção futura: [criar uma política de acesso do aplicativo e personalizada mensagem de acesso negado](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup#create-an-application-access-policy-and-custom-access-denied-message)

2.  A página da Web usando uma conta de domínio da empresa de logon: **roberth@contoso.com**e a senha:**P@ssword**.

3.  Você precisará instalar um perfil. Sobre o **instalar perfil** de tela, clique em **instalar**.

4.  Quando solicitado a confirmar a instalação do perfil, clique em **instalar agora**.

5.  Se seu dispositivo exigir um PIN desbloquear o dispositivo, você precisará inserir o PIN.

6.  A instalação de perfil é concluída quando você vir a **perfil instalado** tela. Clique em **feito**.

    Retorne ao Safari. Uma mensagem informando que você pode fechar ou sair do Safari.

> [!TIP]
> Para exibir ou remover o perfil Workplace Join, navegue até **configurações**, clique em **geral**e clique em **perfis** em seu dispositivo iOS.

## <a name="see-also"></a>Consulte também


- [Junte-se a empresa de qualquer dispositivo para SSO e perfeito segundo fator de autenticação em todos os aplicativos da empresa](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
- [Configurar o ambiente de laboratório do AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)
- [Passo a passo: Associação de local de trabalho com um dispositivo Windows](Walkthrough--Workplace-Join-with-a-Windows-Device.md)



