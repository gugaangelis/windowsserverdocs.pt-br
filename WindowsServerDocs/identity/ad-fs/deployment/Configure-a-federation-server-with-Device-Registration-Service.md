---
ms.assetid: fdd1c1fd-55aa-4eb8-ae84-53f811de042c
title: "Configurar um servidor de federação com o serviço de registro de dispositivo"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 511a039afd47cf7570fffdcaf17842e0eccc5683
ms.sourcegitcommit: 9278435cbfa8dbeb30d0557ed0d27832b154edd2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/09/2018
---
# <a name="configure-a-federation-server-with-device-registration-service"></a>Configurar um servidor de federação com o serviço de registro de dispositivo

>Aplica-se a: Windows Server 2012 R2

Você pode habilitar o serviço de registro de dispositivo \(DRS\) em seu servidor de Federação depois de concluir os procedimentos [etapa 4: configurar um servidor de Federação](https://technet.microsoft.com/library/dn303424.aspx). O serviço de registro de dispositivo fornece um mecanismo de carregamento por segundo perfeito fator de autenticação, persistente único sign\ em \(SSO\) e acesso condicional aos consumidores que exigem acesso aos recursos da empresa. Para saber mais sobre DRS, consulte [ingressar no local de trabalho de qualquer dispositivo para SSO e perfeita segundo fator de autenticação em todos os aplicativos da empresa](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)  
  
## <a name="prepare-your-active-directory-forest-to-support-devices"></a>Preparar floresta do Active Directory para dar suporte a dispositivos  
  
> [!NOTE]  
> Essa é uma operação de tempo de one\ que você deve executar para preparar a floresta do Active Directory para dar suporte a dispositivos. Você deve estar conectado com permissões de administrador da empresa e floresta do Active Directory deve ter o esquema do Windows Server 2012 R2 para concluir este procedimento.  
>   
> Além disso, DRS exige que você tenha pelo menos um servidor de catálogo global no domínio raiz da floresta. O servidor de catálogo global é necessário para executar Initialize\ ADDeviceRegistration e durante a autenticação do AD FS. AD FS inicializa uma representação de memória in\ do objeto DRS config em cada solicitação de autenticação e se o objeto DRS config não pode ser encontrado em um controlador de domínio no domínio atual, a solicitação será tentada contra o GC no qual os objetos DRS foram provisionados durante Initialize\ ADDeviceRegistration.  
  
#### <a name="to-prepare-the-active-directory-forest"></a>Para preparar a floresta do Active Directory  
  
1.  Em seu servidor de federação, abra uma janela de comando do Windows PowerShell e digite:  
  
    ```  
    Initialize-ADDeviceRegistration  
    ```  
  
2.  Quando solicitado para ServiceAccountName, insira o nome da conta de serviço que você selecionou como a conta de serviço do AD FS.  Se for uma conta gMSA, insira a conta no **domain\\accountname$** formato. Para uma conta de domínio, use o formato **domain\\accountname**.  
  
## <a name="enable-device-registration-service-on-a-federation-server-farm-node"></a>Habilitar o serviço de registro do dispositivo em um nó de farm de servidor de Federação  
  
> [!NOTE]  
> Você deve estar conectado com permissões de administrador de domínio para concluir este procedimento.  
  
#### <a name="to-enable-device-registration-service"></a>Para habilitar o serviço de registro de dispositivo  
  
1.  Em seu servidor de federação, abra uma janela de comando do Windows PowerShell e digite:  
  
    ```  
    Enable-AdfsDeviceRegistration  
    ```  
  
2.  Repita essa etapa em cada nó federação farm seu farm AD FS..  
  
## <a name="enable-seamless-second-factor-authentication"></a>Habilitar perfeita segunda autenticação de fator  
Perfeita segunda autenticação multifator é um aprimoramento do AD FS que fornece um nível adicional de proteção de acesso a recursos corporativos e aplicativos de dispositivos externos que estejam tentando acessá-los. Quando um dispositivo pessoal é o local de trabalho ingressado, ele se torna um dispositivo 'conhecido' e os administradores podem usar essas informações para orientar o acesso condicional e gate acesso aos recursos.  
  
#### <a name="to-enable-seamless-second-factor-authentication-persistent-single-sign-on-sso-and-conditional-access-for-workplace-joined-devices"></a>Para habilitar a contínuo segundo fator de autenticação, persistente único sign\ em \(SSO\) e acesso condicional para dispositivos que ingressaram no local de trabalho  
  
1.  No console de gerenciamento do AD FS, navegue até políticas de autenticação. Selecione Editar autenticação primária Global. Marque a caixa de seleção ao lado de habilitar a autenticação de dispositivo e, em seguida, clique em Okey.  
  
## <a name="update-the-web-application-proxy-configuration"></a>Atualizar a configuração de Proxy de aplicativo Web  
  
> [!IMPORTANT]  
> Você não precisa publicar o serviço de registro de dispositivo para o Proxy de aplicativo Web.  O serviço de registro de dispositivo estará disponível por meio do Proxy de aplicativo da Web depois que ele está habilitado em um servidor de Federação.  Talvez seja necessário concluir este procedimento para atualizar a configuração de Proxy de aplicativo Web se ele foi implantado antes de ativar o serviço de registro do dispositivo.  
  
#### <a name="to-update-the-web-application-proxy-configuration"></a>Para atualizar a configuração de Proxy de aplicativo Web  
  
1.  Em seu servidor Proxy de aplicativo Web, abra uma janela de comando do Windows PowerShell e digite  
  
    ```  
    Update-WebApplicationProxyDeviceRegistration  
    ```  
  
2.  Quando solicitado a fornecer credenciais, insira as credenciais de uma conta que tenha direitos administrativos em seus servidores de Federação.  
  
## <a name="see-also"></a>Consulte também 

[AD FS implantação](../../ad-fs/AD-FS-Deployment.md)  

[Guia de implantação do Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Implantando um Farm de servidores de Federação](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

