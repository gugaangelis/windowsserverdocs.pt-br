---
ms.assetid: c5eb3fa0-550c-4a2f-a0bc-698b690c4199
title: Planejar acesso condicional com base em dispositivo no local
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3a78334f64d9e51515757b01f2d788bf87f67a35
ms.sourcegitcommit: cd12ace92e7251daaa4e9fabf1d8418632879d38
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66501614"
---
# <a name="plan-device-based-conditional-access-on-premises"></a>Planejar acesso condicional com base em dispositivo no local


Este documento descreve as políticas de acesso condicional com base em dispositivos em um cenário híbrido em que os diretórios locais são conectados ao Azure AD usando o Azure AD Connect.     

## <a name="ad-fs-and-hybrid-conditional-access"></a>Acesso condicional do AD FS e híbrida  

O AD FS fornece o componente local diante das políticas de acesso condicional em um cenário híbrido.  Ao registrar dispositivos com o Azure AD para acesso condicional aos recursos de nuvem, o dispositivo do Azure AD Connect write-back recurso disponibiliza as informações de registro de dispositivo no local para as políticas do AD FS consumir e impor.  Dessa forma, você tem uma abordagem consistente para políticas de controle de acesso para ambos os locais e recursos de nuvem.  

![Acesso condicional](media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  

### <a name="types-of-registered-devices"></a>Tipos de dispositivos registrados  
Há três tipos de dispositivos registrados, que são representados como objetos de dispositivo no AD do Azure e pode ser usado para acesso condicional com o AD FS no local também.  

| |Adicionar trabalho ou de estudante  |Ingressar no Azure AD  |Ingresso no domínio 10 Windows    
| --- | --- |--- | --- |
|Descrição    |  Os usuários adicionar seu trabalho ou de estudante para seus dispositivos BYOD interativamente.  **Observação:** Adicionar conta corporativa ou escolar é o substituto do ingresso no local no Windows 8/8.1       | Os usuários ingressar seus dispositivos de trabalho do Windows 10 ao Azure AD.|Dispositivos de ingressados no domínio do Windows 10 registrados automaticamente no Azure AD.|           
|Como os usuários fazem logon no dispositivo     |  Nenhum logon para o Windows como a conta corporativa ou de estudante.  Faça logon usando uma conta da Microsoft.       |   Faça logon no Windows como a conta (ou de estudante) que registrou o dispositivo.      |     Faça logon usando a conta do AD.|      
|Como os dispositivos são gerenciados    |      Políticas de MDM (com o registro no Intune adicional)   | Políticas de MDM (com o registro no Intune adicional)        |   Diretiva de grupo, System Center Configuration Manager (SCCM) |
|Tipo de relação de confiança do AD do Azure|Ingressados no local|Ingressado no Azure AD|Ingressado no domínio  |     
|Local de configurações de W10    | Configurações > contas > sua conta > Adicionar uma conta corporativa ou de estudante        | Configurações > sistema > sobre > ingressar no Azure AD       |   Configurações > sistema > sobre > ingressar em um domínio |       
|Também está disponível para dispositivos iOS e Android?   |    Sim     |       Não  |   Não   |   

  

Para obter mais informações sobre as diferentes maneiras de registrar dispositivos, consulte também:  
* [Usando dispositivos Windows 10 no local de trabalho](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-windows10-devices/)  
* [Como configurar dispositivos Windows 10 para trabalho](https://jairocadena.com/2016/01/18/setting-up-windows-10-devices-for-work-domain-join-azure-ad-join-and-add-work-or-school-account/)  
[Junte-se do Windows 10 Mobile ao Azure Active Directory](https://technet.microsoft.com/itpro/windows/manage/join-windows-10-mobile-to-azure-active-directory)  

### <a name="how-windows-10-user-and-device-sign-on-is-different-from-previous-versions"></a>Como usuário do Windows 10 e o dispositivo de logon é diferente das versões anteriores  
Para Windows 10 e o AD FS 2016, há alguns novos aspectos de autenticação e registro de dispositivo, você deve saber sobre (especialmente se você estiver familiarizado com o registro de dispositivo e "ingresso no local" em versões anteriores).  

Primeiro, no Windows 10 e o AD FS no Windows Server 2016, autenticação e registro de dispositivo não for mais baseia-se exclusivamente em um X509 certificado de usuário.  Há um protocolo de nova e mais robusto que fornece melhor segurança e uma experiência do usuário.  As principais diferenças são que, para o ingresso no domínio do Windows 10 e o ingresso no Azure AD, há um X509 certificado de computador e uma nova credencial chamado um PRT.  Você pode ler tudo sobre ela [aqui](https://jairocadena.com/2016/01/18/how-domain-join-is-different-in-windows-10-with-azure-ad/) e [aqui](https://jairocadena.com/2016/02/01/azure-ad-join-what-happens-behind-the-scenes/).  

Em segundo lugar, o Windows 10 e o AD FS 2016 suporte à autenticação de usuário usando o Microsoft Passport for Work, que você pode ler sobre [aqui](https://jairocadena.com/2016/03/09/azure-ad-and-microsoft-passport-for-work-in-windows-10/) e [aqui](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/).  

AD FS 2016 fornece contínua do dispositivo e usuário SSO, com base em credenciais PRT e Passport.  Usando as etapas neste documento, você pode habilitar esses recursos e vê-los a funcionar.  

### <a name="device-access-control-policies"></a>Políticas de controle de acesso do dispositivo  
Dispositivos podem ser usados nas regras de controle de acesso do AD FS simples, como:  

- permitir o acesso somente de um dispositivo registrado   
- exigir multi-factor authentication, quando um dispositivo não está registrado  

Essas regras, em seguida, podem ser combinadas com outros fatores, como o local de acesso de rede e multi-factor authentication, criando políticas de acesso condicional avançado, como:  


- exigir multi-factor authentication para acessar de fora da rede corporativa, exceto para membros de um determinado grupo ou grupos de dispositivos não registrados  

Com o AD FS 2016, essas políticas podem ser configuradas especificamente para exigir um nível de confiança de determinado dispositivo também: ambos **autenticado**, **gerenciados**, ou **compatível com**.  

Para obter mais informações sobre como configurar o AD FS políticas de controle de acesso, consulte [políticas de controle de acesso no AD FS](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md).  

#### <a name="authenticated-devices"></a>Dispositivos autenticados  
Dispositivos autenticados são dispositivos registrados que não estão registrados no MDM (Intune e 3ª parte MDMs para Windows 10, o Intune somente para iOS e Android).   

Dispositivos autenticados terão a **isManaged** com valor de declaração do AD FS **falso**. (Enquanto que os dispositivos que não estão registrados não terá essa declaração.)  Dispositivos autenticados (e todos os dispositivos registrados) terá o isKnown AD FS de declaração com o valor **verdadeira**.  

#### <a name="managed-devices"></a>Dispositivos gerenciados:   

Dispositivos gerenciados são dispositivos registrados que estão registrados no MDM.  

Dispositivos gerenciados terão o isManaged AD FS de declaração com o valor **verdadeira**.  

#### <a name="devices-compliant-with-mdm-or-group-policies"></a>Dispositivos em conformidade (com o MDM ou diretivas de grupo)  
Dispositivos compatíveis sejam dispositivos registrados que não estão apenas registrados com o MDM, mas em conformidade com as políticas MDM. (As informações de conformidade é originada com o MDM e são gravadas para o Azure AD.)  

Dispositivos em conformidade e terão a **isCompliant** com valor de declaração do AD FS **verdadeiro**.    

Para obter uma lista completa de dispositivos do AD FS 2016 e declarações de acesso condicional, consulte [referência](#reference).  


## <a name="reference"></a>Referência  
#### <a name="complete-list-of-new-ad-fs-2016-and-device-claims"></a>Lista completa de novas declarações do AD FS 2016 e dispositivo  

* https://schemas.microsoft.com/ws/2014/01/identity/claims/anchorclaimtype  
* http://schemas.xmlsoap.org/ws/2005/05/identity/claims/implicitupn  
* https://schemas.microsoft.com/2014/03/psso  
* https://schemas.microsoft.com/2015/09/prt  
* http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn  
* https://schemas.microsoft.com/ws/2008/06/identity/claims/primarygroupsid  
* https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid  
* http://schemas.xmlsoap.org/ws/2005/05/identity/claims/name  
* https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname  
* https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/registrationid  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/displayname  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/identifier  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/ostype  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/osversion  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/ismanaged  
* https://schemas.microsoft.com/2012/01/devicecontext/claims/isregistereduser  
* https://schemas.microsoft.com/2014/02/devicecontext/claims/isknown  
* https://schemas.microsoft.com/2014/02/deviceusagetime  
* https://schemas.microsoft.com/2014/09/devicecontext/claims/iscompliant  
* https://schemas.microsoft.com/2014/09/devicecontext/claims/trusttype  
* https://schemas.microsoft.com/claims/authnmethodsreferences  
* https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-user-agent  
* https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-endpoint-absolute-path  
* https://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork  
* https://schemas.microsoft.com/2012/01/requestcontext/claims/client-request-id  
* https://schemas.microsoft.com/2012/01/requestcontext/claims/relyingpartytrustid  
* https://schemas.microsoft.com/2012/01/requestcontext/claims/x-ms-client-ip  
* https://schemas.microsoft.com/2014/09/requestcontext/claims/userip  
* https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod  
