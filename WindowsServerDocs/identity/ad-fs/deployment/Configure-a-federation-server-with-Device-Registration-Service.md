---
ms.assetid: fdd1c1fd-55aa-4eb8-ae84-53f811de042c
title: Configurar um servidor de federação com o Serviço de Registro de Dispositivos
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f1367f03ea8a9ba96bfe4bae1c324deff92576f0
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192263"
---
# <a name="configure-a-federation-server-with-device-registration-service"></a>Configurar um servidor de federação com o Serviço de Registro de Dispositivos

Você pode habilitar o serviço de registro de dispositivo \(DRS\) no servidor de Federação depois de concluir os procedimentos [etapa 4: Configurar um servidor de Federação](https://technet.microsoft.com/library/dn303424.aspx). O serviço de registro de dispositivo fornece um mecanismo de integração contínua autenticação multifator, o logon único persistente\-na \(SSO\)e o acesso condicional para os consumidores que necessitam de acesso à empresa recursos. Para obter mais informações sobre o DRS, consulte [ingresse no local de trabalho de qualquer dispositivo para SSO e contínuo segundo fator de autenticação em aplicativos da empresa](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)  
  
## <a name="prepare-your-active-directory-forest-to-support-devices"></a>Preparar a floresta do Active Directory para dar suporte a dispositivos  
  
> [!NOTE]  
> Isso é um arquivo\-tempo a operação que você deve executar para preparar a floresta do Active Directory para dar suporte a dispositivos. Você deve estar conectado com permissões de administrador corporativo e sua floresta do Active Directory deve ter o esquema do Windows Server 2012 R2 para concluir este procedimento.  
>   
> Além disso, o DRS requer que você tenha pelo menos um servidor de catálogo global no domínio raiz da floresta. O servidor de catálogo global é necessário para executar inicialização\-ADDeviceRegistration e durante a autenticação do AD FS. AD FS inicializa no\-representação de memória da configuração DRS do objeto em cada solicitação de autenticação e se o objeto de configuração DRS não pode ser encontrado em um controlador de domínio no domínio atual, a solicitação é tentada contra o GC no qual os objetos do DRS foram provisionado durante a inicialização\-ADDeviceRegistration.  
  
#### <a name="to-prepare-the-active-directory-forest"></a>Para preparar a floresta do Active Directory  
  
1.  No servidor de federação, abra uma janela de comando do Windows PowerShell e digite:  
  
    ```  
    Initialize-ADDeviceRegistration  
    ```  
  
2.  Quando for ServiceAccountName solicitado, insira o nome da conta de serviço que você selecionou como a conta de serviço do AD FS.  Se for uma conta gMSA, insira a conta de **domínio\\accountname$** formato. Para uma conta de domínio, use o formato **domínio\\accountname**.  
  
## <a name="enable-device-registration-service-on-a-federation-server-farm-node"></a>Habilitar o serviço de registro de dispositivo em um nó de farm de servidor de Federação  
  
> [!NOTE]  
> Você deve fazer logon com permissões de administrador de domínio para concluir este procedimento.  
  
#### <a name="to-enable-device-registration-service"></a>Para habilitar o serviço de registro de dispositivo  
  
1.  No servidor de federação, abra uma janela de comando do Windows PowerShell e digite:  
  
    ```  
    Enable-AdfsDeviceRegistration  
    ```  
  
2.  Repita essa etapa em cada nó do farm de federação no farm do AD FS...  
  
## <a name="enable-seamless-second-factor-authentication"></a>Habilitar contínuo autenticação de dois fatores  
Contínuo autenticação de dois fatores é um aprimoramento no AD FS que fornece um nível adicional de proteção de acesso a recursos corporativos e aplicativos de dispositivos externos que estão tentando para acessá-los. Quando um dispositivo pessoal é ingressado no local de trabalho, ele se torna um dispositivo 'conhecido' e os administradores podem usar essas informações para conduzir acesso condicional e o acesso aos recursos.  
  
#### <a name="to-enable-seamless-second-factor-authentication-persistent-single-sign-on-sso-and-conditional-access-for-workplace-joined-devices"></a>Para habilitar o segundo contínuo autenticação multifator, persistente SSO\-na \(SSO\) e acesso condicional para dispositivos ingressados no local de trabalho  
  
1.  No console de gerenciamento do AD FS, navegue até as políticas de autenticação. Selecione Editar autenticação primária Global. Marque a caixa de seleção ao lado de habilitar a autenticação de dispositivo e, em seguida, clique em Okey.  
  
## <a name="update-the-web-application-proxy-configuration"></a>Atualizar a configuração de Proxy de aplicativo Web  
  
> [!IMPORTANT]  
> Não é preciso publicar o serviço de registro de dispositivo para o Proxy de aplicativo Web.  O serviço de registro de dispositivo estará disponível por meio do Proxy de aplicativo Web depois de ter habilitado em um servidor de Federação.  Você talvez precise concluir este procedimento para atualizar a configuração de Proxy de aplicativo Web se ele tiver sido implantado antes de habilitar o serviço de registro do dispositivo.  
  
#### <a name="to-update-the-web-application-proxy-configuration"></a>Para atualizar a configuração de Proxy de aplicativo Web  
  
1.  No servidor de Proxy de aplicativo Web, abra uma janela de comando do Windows PowerShell e digite  
  
    ```  
    Update-WebApplicationProxyDeviceRegistration  
    ```  
  
2.  Quando solicitado a fornecer credenciais, insira as credenciais de uma conta que tenha direitos administrativos aos servidores de Federação.  
  
## <a name="see-also"></a>Consulte também 

[Implantação do AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guia de implantação do Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Como implantar um farm de servidores de federação](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

