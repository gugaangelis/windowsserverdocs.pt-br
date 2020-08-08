---
ms.assetid: c17d143b-86b4-47c0-b76e-1862dda8f0bd
title: Walkthrough-Workplace Join com um dispositivo Windows
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 4f2e3dbf5754b4b33aec972e40a91cbe67c82220
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87966973"
---
# <a name="walkthrough-workplace-join-with-a-windows-device"></a>Passo a passo: Ingresso no local de trabalho com um dispositivo do Windows

Este tópico demonstra como usar a Ingresso no Local de Trabalho para conectar seu dispositivo Windows com seu local de trabalho e como acessar um aplicativo Web usando Logon Único. Você deve concluir as etapas na seção [Configurar o ambiente de laboratório para AD FS no Windows Server 2012 R2](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md) para poder experimentar este passo a passos.

## <a name="access-the-web-application-before-device-registration"></a>Acesse o aplicativo Web antes do registro do dispositivo
Neste passo a passo, você acessa um aplicativo Web da empresa antes de unir seu dispositivo ao local de trabalho. A página da Web exibe as declarações incluídas no seu token de segurança. Observe que a lista de declarações não inclui qualquer informação sobre o seu dispositivo. Você também pode observar que não tem Logon Único.

#### <a name="to-access-the-web-application-before-you-use-workplace-join-on-your-device"></a>Para acessar o aplicativo Web antes de usar o Ingresso no Local de Trabalho no dispositivo

1. Faça logon em Client1 com sua conta da Microsoft.

2. Abra o Internet Explorer e navegue até seu aplicativo de declarações genérico, **https://webserv1.contoso.com/claimapp** .

3. Faça logon na página da Web usando uma conta de domínio da empresa: <strong>roberth@contoso.com</strong> , senha: <strong>P@ssword</strong> .

4. A página da Web lista todos os créditos em seu token de segurança. Apenas declarações do usuário estão presentes em seu token de segurança.

5. Feche o Internet Explorer.

6. Abra o Internet Explorer e navegue até o mesmo aplicativo de declarações, **https://webserv1.contoso.com/claimapp** .

7. Observe que você será solicitado a digitar suas credenciais novamente. Você não está conectado ao local de trabalho em um dispositivo com Ingresso no Local de Trabalho, portanto, não tem Logon Único.

## <a name="join-your-device-with-workplace-join"></a>Una seu dispositivo com Ingresso no Local de Trabalho

> [!IMPORTANT]
> Para que o Ingresso no Local de Trabalho seja bem-sucedido, o computador cliente (Client1) deve confiar no certificado SSL que foi usado para configurar o AD FS (Serviços de Federação do Active Directory) na [Step 2: Configure the Federation Server with Device Registration Service (ADFS1)](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4). Ele também deve conseguir validar as informações de revogação para o certificado. Se você tiver algum problema com o ingresso no local de trabalho, é possível exibir o log de eventos no Client1.
>
> Para ver o log de eventos, abra o Visualizador de Eventos, expanda **Logs de Aplicativos e Serviços**, expanda **Microsoft**, expanda **Windows** e clique em **Ingresso no Local de Trabalho**.

#### <a name="to-join-your-device-with-workplace-join"></a>Para ingressar seu dispositivo com Ingresso no Local de Trabalho

1. Faça logon em Client1 com sua conta da Microsoft.

2. Na tela **Início**, abra a barra **Botões** e selecione o botão **Configurações**. Selecione **Mudar Configurações do PC**.

3. Na página **Configurações do PC**, selecione **Rede** e depois clique em **Local de Trabalho**.

4. Na caixa **Inserir o userid para obter acesso ao local de trabalho ou ativar o gerenciamento de dispositivos** , digite <strong>roberth@contoso.com</strong> e clique em **ingressar**.

5. Quando você for solicitado a fornecer credenciais, digite <strong>roberth@contoso.com</strong> e senha: <strong>P@ssword</strong> . Clique em **OK**.

6. Agora você deve ver a mensagem: "Este dispositivo ingressou em sua rede do local de trabalho."

### <a name="access-the-web-application-after-joining-the-workplace"></a>Acesse o aplicativo Web depois de ingressar no local de trabalho
Nesta parte da demonstração, você acessa um aplicativo Web da empresa de seu dispositivo que está conectado com Ingressar no Local de Trabalho. A página da Web exibe as declarações incluídas no seu token de segurança. Observe que a lista de declarações inclui informações do dispositivo e do usuário. Você também pode observar que agora tem Logon Único.

##### <a name="to-access-the-web-application-after-joining-the-workplace"></a>Para acessar o aplicativo Web depois de ingressar no local de trabalho

1. Faça logon em **Client1** com sua conta da Microsoft.

2. Abra o Internet Explorer e navegue até seu aplicativo de declarações genérico, **https://webserv1.contoso.com/claimapp** .

3. Faça logon na página da Web usando uma conta de domínio da empresa: <strong>roberth@contoso.com</strong> , senha: <strong>P@ssword</strong> .

4. A página da Web lista as declarações em seu token de segurança. Seu token contém declarações do usuário e do dispositivo.

5. Feche o Internet Explorer.

6. Abra o Internet Explorer e navegue até o mesmo aplicativo de declarações, **https://webserv1.contoso.com/claimapp** .

7. Observe que você **não** é solicitado a digitar suas credenciais novamente. Você está conectado ao local de trabalho com base em um dispositivo com Ingresso no Local de Trabalho e, portanto, tem Logon Único.

## <a name="see-also"></a>Consulte Também
[Ingresse no local de trabalho de qualquer dispositivo para SSO e autenticação de segundo fator direta entre aplicativos](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md) 
 da empresa [Configurar o ambiente de laboratório para AD FS no Windows Server 2012 R2](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md) 
 [Walkthrough: Workplace Join com um dispositivo IOS](Walkthrough--Workplace-Join-with-an-iOS-Device.md)



