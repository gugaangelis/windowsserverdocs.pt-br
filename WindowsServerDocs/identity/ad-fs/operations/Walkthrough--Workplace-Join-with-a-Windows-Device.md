---
ms.assetid: c17d143b-86b4-47c0-b76e-1862dda8f0bd
title: Passo a passo - ingresso no local de trabalho com um dispositivo Windows
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fd222eb47982591e051594e8a572443b65c0357f
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="walkthrough-workplace-join-with-a-windows-device"></a>Passo a passo: Associação de local de trabalho com um dispositivo Windows

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Este tópico demonstra como usar Workplace Join para conectar seu dispositivo Windows com seu local de trabalho e como acessar um aplicativo da web usando o logon único. Você deve concluir as etapas a [configurar o ambiente de laboratório do AD FS no Windows Server 2012 R2](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md) seção antes de você pode experimentar este passo a passo.

## <a name="access-the-web-application-before-device-registration"></a>Acessar o aplicativo da web antes que o registro de dispositivo
Neste passo a passo, você acessar um aplicativo web da empresa antes de ingressar o dispositivo para a área de trabalho. A página da Web exibe as declarações que foram incluídas no seu token de segurança. Observe que a lista de declarações não inclui nenhuma informação sobre seu dispositivo. Você também pode observar que você não tenha o logon único.

#### <a name="to-access-the-web-application-before-you-use-workplace-join-on-your-device"></a>Para acessar o aplicativo web antes de usar Workplace Join em seu dispositivo

1.  Faça logon Client1 com sua conta da Microsoft.

2.  Abrir o Internet Explorer e navegue até seu aplicativo genérico requerimentos judiciais ou Extrajudiciais, **https://webserv1.contoso.com/claimapp**.

3.  A página da Web usando uma conta de domínio da empresa de logon: **roberth@contoso.com**, senha:**P@ssword**.

4.  A página da Web lista todas as declarações em seu token de segurança. Declarações do usuário estão presentes em seu token de segurança.

5.  Feche o Internet Explorer.

6.  Abra o Internet Explorer e navegue até o mesmo aplicativo requerimentos judiciais ou Extrajudiciais, **https://webserv1.contoso.com/claimapp**.

7.  Observe que você precisará inserir novamente suas credenciais. Você não estiver conectado ao local de trabalho de um dispositivo com o ingresso no local de trabalho e, portanto, não têm o logon único.

## <a name="join-your-device-with-workplace-join"></a>Ingressar o dispositivo com o ingresso no local de trabalho

> [!IMPORTANT]
> Para Workplace Join ter sucesso, o computador cliente (Client1) deve confiar no certificado SSL que foi usado para configurar os serviços de Federação do Active Directory (AD FS) em [etapa 2: configurar o servidor de federação com o serviço de registro do dispositivo (ADFS1)](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md#BKMK_4). Também deve ser capaz de validar informações de revogação para o certificado. Se você tiver problemas com o ingresso no local de trabalho, você pode exibir o log de eventos em Client1.
> 
> Para ver o log de eventos, abra o Visualizador de eventos, expanda **Logs de aplicativos e serviços**, expanda **Microsoft**, expanda **Windows**e clique em **Workplace Join**.

#### <a name="to-join-your-device-with-workplace-join"></a>Para participar de seu dispositivo com o ingresso no local de trabalho

1.  Faça logon Client1 com sua conta da Microsoft.

2.  No **iniciar** tela, abra o **botões** bar e selecione o **configurações** botão. Selecione **alterar configurações do PC**.

3.  Sobre o **configurações do PC** página, selecione **rede**e, em seguida, clique em **empresa**.

4.  No **digite seu ID de usuário para acessar o local de trabalho ou ativar o gerenciamento de dispositivo**, digite **roberth@contoso.com**e clique em **ingressar**.

5.  Quando solicitado a fornecer credenciais, digite **roberth@contoso.com**e a senha:**P@ssword**. Clique em **Okey**.

6.  Agora você deve ver a mensagem: "Este dispositivo ingressou em sua rede local de trabalho."

### <a name="access-the-web-application-after-joining-the-workplace"></a>Acessar o aplicativo da web depois de se inscrever o local de trabalho
Nesta parte da demonstração, você pode acessar um aplicativo web da empresa de seu dispositivo está conectado com o ingresso no local de trabalho. A página da Web exibe as declarações que foram incluídas no seu token de segurança. Observe que a lista de requerimentos judiciais ou Extrajudiciais inclui informações de dispositivo e usuário. Você também pode observar que agora você tem o logon único.

##### <a name="to-access-the-web-application-after-joining-the-workplace"></a>Para acessar o aplicativo da web depois de se inscrever o local de trabalho

1.  Fazer logon **Client1** com sua conta da Microsoft.

2.  Abrir o Internet Explorer e navegue até seu aplicativo genérico requerimentos judiciais ou Extrajudiciais, **https://webserv1.contoso.com/claimapp**.

3.  A página da Web usando uma conta de domínio da empresa de logon: **roberth@contoso.com**, senha:**P@ssword**.

4.  A página da Web lista declarações em seu token de segurança. O token contém declarações de dispositivo e usuário.

5.  Feche o Internet Explorer.

6.  Abra o Internet Explorer e navegue até o mesmo aplicativo requerimentos judiciais ou Extrajudiciais, **https://webserv1.contoso.com/claimapp**.

7.  Observe que você está **não** solicitado a inserir novamente suas credenciais. Você está conectado de um dispositivo com o ingresso no local de trabalho e, portanto, tenha o logon único.

## <a name="see-also"></a>Consulte também
[Junte-se a empresa de qualquer dispositivo para SSO e perfeita segundo fator de autenticação em todos os aplicativos da empresa](Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)<ph x="2">
</ph>[configurar o ambiente de laboratório do AD FS no Windows Server 2012 R2](../deployment/Set-up-the-lab-environment-for-AD-FS-in-Windows-Server-2012-R2.md)<ph x="4">
</ph>[passo a passo: Workplace Join com um dispositivo iOS](Walkthrough--Workplace-Join-with-an-iOS-Device.md)



