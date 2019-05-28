---
ms.assetid: 299e4fb9-8f1a-4275-ad7d-dad4f1594657
title: 'Passo a passo: ingresso no local com um dispositivo iOS'
description: ''
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/18/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 42b71667758f392d641c5262e34322f8b21cfad9
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66188906"
---
# <a name="walkthrough-workplace-join-with-an-ios-device"></a>Passo a passo: Ingresso no Local de Trabalho com um dispositivo iOS


> [!IMPORTANT] 
> Esse método é relevante para clientes de local completo apenas. Híbrido ou clientes somente em nuvem não devem usar esse método para registrar seus dispositivos iOS. E esse método não é compatível quando os clientes locais decidirem mover para a nuvem. O dispositivo deve ser cancelado e registrado com a nuvem. 

Este tópico demonstra o Ingresso no Local de Trabalho em um dispositivo iOS. Você deve concluir as etapas a [configurar o ambiente de laboratório para o AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md) seção antes que você pode experimentar este passo a passo. Você pode usar o dispositivo para acessar o mesmo aplicativo web da empresa acessado em [passo a passo: Ingresso no local de um dispositivo Windows](Walkthrough--Workplace-Join-with-a-Windows-Device.md).


## <a name="join-an-ios-device-with-workplace-join"></a>Ingressar em um dispositivo iOS com o Ingresso no Local de Trabalho

> [!IMPORTANT]
> Quando DRS local é configurado, o dispositivo iOS deve confiar no certificado de Secure Socket Layer (SSL) que foi usado para configurar os serviços de Federação do Active Directory (AD FS) no [etapa 2: Configurar o servidor de Federação (ADFS1) with Device Registration Service](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4), para o ingresso seja bem-sucedido.
> 
> -   Se o certificado SSL do AD FS tiver sido emitido em uma AC (autoridade de certificação) de teste, instale o certificado de autoridade de certificação em seu dispositivo iOS.
> -   Se seu certificado de autoridade de certificação estiver publicado em um site, você poderá navegar pelo site em seu dispositivo iOS e instalar o certificado.

Nesta demonstração, você ingressa o dispositivo no local de trabalho.

#### <a name="to-join-an-ios-device-to-a-workplace"></a>Para ingressar um dispositivo iOS em um local de trabalho

1.  -   **Quando o serviço Registro de Dispositivo do Active Directory do Azure é o DRS configurado:** Abra o Apple Safari e navegue até o ponto de extremidade do perfil por satélite de serviço de registro de dispositivo do Active Directory do Azure para dispositivos iOS, <`https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/<yourdomainname` > em que <`yourdomainname`> é o nome de domínio que você configurou com o Azure Active Directory. Por exemplo, se seu nome de domínio for contoso.com, a URL seria: `https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com`

    -   **Quando DRS local é o DRS configurado**: Abra o Apple Safari e navegue até o ponto de extremidade perfil por satélite de serviço de registro de dispositivo (DRS) para dispositivos iOS, `https://adf1s.contoso.com/enrollmentserver/otaprofile`

    Há muitas maneiras de se comunicar esta URL aos usuários. Uma maneira recomendada é publicando a URL em uma mensagem negada de acesso de aplicativo personalizado no AD FS. Isso é abordado na seção a seguir: [Criar uma política de acesso do aplicativo e o acesso negado a mensagem personalizada](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup#create-an-application-access-policy-and-custom-access-denied-message)

2.  Faça logon na página da Web usando uma conta de domínio da empresa: **roberth@contoso.com** e a senha: **P@ssword**.

3.  Aparecerá um aviso para que você instale um perfil. Na tela **Instalar Perfil** , clique em **Instalar**.

4.  Quando solicitado a confirmar a instalação do perfil, clique em **Instalar Agora**.

5.  Se seu dispositivo requer um PIN para ser desbloqueado, você será solicitado a inserir seu PIN.

6.  A instalação do perfil é concluída quando você aparecer a tela **Perfil Instalado** . Clique em **Concluído**.

    Retorne ao Safari. Uma mensagem informa que você pode fechar ou sair do Safari.

> [!TIP]
> Para exibir ou remover o perfil do Ingresso no Local de Trabalho, navegue até **Configurações**, clique em **Geral** e em **Perfis** em seu dispositivo iOS.

## <a name="see-also"></a>Consulte também


- [Ingresse no local de trabalho de qualquer dispositivo de SSO contínuo e fatores de autenticação em aplicativos da empresa](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
- [Configurar o ambiente de laboratório para o AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)
- [Passo a passo: Ingressar no local de trabalho com um dispositivo do Windows](Walkthrough--Workplace-Join-with-a-Windows-Device.md)



