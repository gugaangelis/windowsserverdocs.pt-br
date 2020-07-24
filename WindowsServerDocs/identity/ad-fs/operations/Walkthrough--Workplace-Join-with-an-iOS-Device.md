---
ms.assetid: 299e4fb9-8f1a-4275-ad7d-dad4f1594657
title: Walkthrough-Workplace Join com um dispositivo iOS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 10/18/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 8c478e31c3a86203f6c5f249185659caf9881723
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86963468"
---
# <a name="walkthrough-workplace-join-with-an-ios-device"></a>Passo a passo: Ingresso no Local de Trabalho com um dispositivo iOS


> [!IMPORTANT] 
> Esse método é relevante somente para clientes totalmente locais. Clientes híbridos ou somente de nuvem não devem usar esse método para registrar seus dispositivos iOS. E esse método não é compatível quando os clientes locais decidem mudar para a nuvem. O registro do dispositivo deve ser cancelado e registrado na nuvem. 

Este tópico demonstra o Ingresso no Local de Trabalho em um dispositivo iOS. Você deve concluir as etapas na seção [Configurar o ambiente de laboratório para AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md) para poder experimentar este passo a passos. Você pode usar o dispositivo para acessar o mesmo aplicativo Web da empresa que você acessou em [passo a passos: Workplace Join com um dispositivo Windows](Walkthrough--Workplace-Join-with-a-Windows-Device.md).


## <a name="join-an-ios-device-with-workplace-join"></a>Ingressar em um dispositivo iOS com o Ingresso no Local de Trabalho

> [!IMPORTANT]
> Quando o DRS local é configurado, o dispositivo iOS deve confiar no certificado SSL (Secure Socket Layer) usado para configurar o AD FS (Serviços de Federação do Active Directory) no [Step 2: Configure the federation server (ADFS1) with Device Registration Service](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4), para que o Ingresso no Local de Trabalho tenha êxito.
> 
> -   Se o certificado SSL do AD FS tiver sido emitido em uma AC (autoridade de certificação) de teste, instale o certificado de autoridade de certificação em seu dispositivo iOS.
> -   Se seu certificado de autoridade de certificação estiver publicado em um site, você poderá navegar pelo site em seu dispositivo iOS e instalar o certificado.

Nesta demonstração, você ingressa o dispositivo no local de trabalho.

#### <a name="to-join-an-ios-device-to-a-workplace"></a>Para ingressar um dispositivo iOS em um local de trabalho

1. -   **Quando registro de dispositivos do Azure Active Directory serviço é o DRS configurado:** Abra o Apple Safari e navegue até Registro de Dispositivos do Azure Active Directory ponto de extremidade de perfil do serviço over-the-Air para dispositivos iOS, <`https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/<yourdomainname` > em que <`yourdomainname`> é o nome de domínio que você configurou com o Azure Active Directory. Por exemplo, se seu nome de domínio for contoso.com, a URL seria: `https://enterpriseregistration.windows.net/enrollmentserver/otaprofile/contoso.com`

   -   **Quando o DRS local é o DRS configurado**: Abra o Apple Safari e navegue até o ponto de extremidade do perfil do DRS (serviço de registro de dispositivo) por meio do ar para dispositivos IOS,`https://adf1s.contoso.com/enrollmentserver/otaprofile`

   Há muitas maneiras de se comunicar esta URL aos usuários. Uma maneira recomendada é publicando a URL em uma mensagem de acesso de aplicativo negado personalizada no AD FS. Isso é abordado na próxima seção: [criar uma política de acesso de aplicativo e uma mensagem de acesso negado personalizado](/azure/active-directory/active-directory-device-registration-on-premises-setup#create-an-application-access-policy-and-custom-access-denied-message)

2. Faça logon na página da Web usando uma conta de domínio da empresa: <strong>roberth@contoso.com</strong> e senha: <strong>P@ssword</strong> .

3. Aparecerá um aviso para que você instale um perfil. Na tela **Instalar Perfil**, clique em **Instalar**.

4. Quando solicitado a confirmar a instalação do perfil, clique em **Instalar Agora**.

5. Se seu dispositivo requer um PIN para ser desbloqueado, você será solicitado a inserir seu PIN.

6. A instalação do perfil é concluída quando você aparecer a tela **Perfil Instalado**. Clique em **Concluído**.

   Retorne ao Safari. Uma mensagem informa que você pode fechar ou sair do Safari.

> [!TIP]
> Para exibir ou remover o perfil do Ingresso no Local de Trabalho, navegue até **Configurações**, clique em **Geral** e em **Perfis** em seu dispositivo iOS.

## <a name="see-also"></a>Consulte Também


- [Ingresso no Local de Trabalho em qualquer dispositivo de SSO e autenticação de dois fatores contínua em aplicativos da empresa](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)
- [Configurar o ambiente de laboratório para o AD FS no Windows Server 2012 R2](../../ad-fs/deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)
- [Passo a passo: Ingressar no local de trabalho com um dispositivo do Windows](Walkthrough--Workplace-Join-with-a-Windows-Device.md)
