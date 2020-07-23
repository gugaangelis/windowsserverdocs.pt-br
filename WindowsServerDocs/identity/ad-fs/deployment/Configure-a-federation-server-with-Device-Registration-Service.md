---
ms.assetid: fdd1c1fd-55aa-4eb8-ae84-53f811de042c
title: Configurar um servidor de federação com o Serviço de Registro de Dispositivos
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 7e9634cff6f8bf92dcc556914b72657142f9a25e
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966028"
---
# <a name="configure-a-federation-server-with-device-registration-service"></a>Configurar um servidor de federação com o Serviço de Registro de Dispositivos

Você pode habilitar \( o serviço de registro de dispositivo DRS \) no servidor de Federação depois de concluir os procedimentos na [etapa 4: configurar um servidor de Federação](/previous-versions/orphan-topics/ws.11/dn303424(v=ws.11)). O serviço de registro de dispositivo fornece um mecanismo de integração para autenticação de segundo fator contínua, SSO de logon único persistente \- \( \) e acesso condicional aos consumidores que exigem acesso aos recursos da empresa. Para obter mais informações sobre o DRS, consulte [ingressar no local de trabalho de qualquer dispositivo para SSO e autenticação de segundo fator direta entre aplicativos da empresa](../../ad-fs/operations/Join-to-Workplace-from-Any-Device-for-SSO-and-Seamless-Second-Factor-Authentication-Across-Company-Applications.md)  
  
## <a name="prepare-your-active-directory-forest-to-support-devices"></a>Preparar sua floresta do Active Directory para dar suporte a dispositivos  
  
> [!NOTE]  
> Essa é uma \- operação única que você deve executar para preparar sua floresta Active Directory para dar suporte a dispositivos. Você deve fazer logon com permissões de administrador corporativo e sua floresta do Active Directory deve ter o esquema do Windows Server 2012 R2 para concluir esse procedimento.  
>   
> Além disso, o DRS requer que você tenha pelo menos um servidor de catálogo global no domínio raiz da floresta. O servidor de catálogo global é necessário para executar o ADDeviceRegistration de inicialização \- e durante a autenticação AD FS. AD FS Inicializa uma \- representação na memória do objeto de configuração do DRS em cada solicitação de autenticação e, se o objeto de configuração do DRS não puder ser encontrado em um DC no domínio atual, a solicitação será tentada em relação ao GC no qual os objetos do DRS foram provisionados durante a inicialização \- ADDeviceRegistration.  
  
#### <a name="to-prepare-the-active-directory-forest"></a>Para preparar a floresta Active Directory  
  
1.  No servidor de federação, abra uma janela de comando do Windows PowerShell e digite:  
  
    ```  
    Initialize-ADDeviceRegistration  
    ```  
  
2.  Quando for solicitado ServiceAccountName, insira o nome da conta do serviço que você selecionou como a conta de serviço para o AD FS.  Se for uma conta do gMSA, insira a conta no formato de **domínio \\ AccountName $** . Para uma conta de domínio, use o formato **domínio \\ AccountName**.  
  
## <a name="enable-device-registration-service-on-a-federation-server-farm-node"></a>Habilitar o serviço de registro de dispositivo em um nó de farm de servidores de Federação  
  
> [!NOTE]  
> Você deve estar conectado com permissões de administrador de domínio para concluir este procedimento.  
  
#### <a name="to-enable-device-registration-service"></a>Para habilitar o serviço de registro de dispositivo  
  
1.  No servidor de federação, abra uma janela de comando do Windows PowerShell e digite:  
  
    ```  
    Enable-AdfsDeviceRegistration  
    ```  
  
2.  Repita essa etapa em cada nó de farm de Federação no farm de AD FS..  
  
## <a name="enable-seamless-second-factor-authentication"></a>Habilitar a autenticação de segundo fator direta  
A autenticação de segundo fator direta é um aprimoramento no AD FS que fornece um nível adicional de proteção de acesso a recursos corporativos e aplicativos de dispositivos externos que estão tentando acessá-los. Quando um dispositivo pessoal é associado ao local de trabalho, ele se torna um dispositivo "conhecido" e os administradores podem usar essas informações para direcionar o acesso condicional e o acesso à porta para recursos.  
  
#### <a name="to-enable-seamless-second-factor-authentication-persistent-single-sign-on-sso-and-conditional-access-for-workplace-joined-devices"></a>Para habilitar a autenticação de segundo fator contínua, SSO persistente de logon único \- \( \) e acesso condicional para dispositivos ingressados no local de trabalho  
  
1.  No console de gerenciamento do AD FS, navegue até políticas de autenticação. Selecione Editar Autenticação Primária Global. Marque a caixa de seleção ao lado de Habilitar Autenticação de Dispositivo e clique em OK.  
  
## <a name="update-the-web-application-proxy-configuration"></a>Atualizar a configuração do proxy de aplicativo Web  
  
> [!IMPORTANT]  
> Você não precisa publicar o serviço de registro de dispositivo no proxy de aplicativo Web.  O serviço de registro de dispositivo estará disponível por meio do proxy de aplicativo Web quando ele estiver habilitado em um servidor de Federação.  Talvez seja necessário concluir este procedimento para atualizar a configuração do proxy de aplicativo Web se ela tiver sido implantada antes de habilitar o serviço de registro de dispositivo.  
  
#### <a name="to-update-the-web-application-proxy-configuration"></a>Para atualizar a configuração do proxy de aplicativo Web  
  
1.  No servidor proxy de aplicativo Web, abra uma janela de comando do Windows PowerShell e digite  
  
    ```  
    Update-WebApplicationProxyDeviceRegistration  
    ```  
  
2.  Quando solicitado a fornecer credenciais, insira as credenciais de uma conta que tenha direitos administrativos em seus servidores de Federação.  
  
## <a name="see-also"></a>Consulte Também 

[Implantação do AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guia de Implantação do AD FS do Windows Server 2012 R2](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Como implantar um farm de servidores de federação](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  
