---
ms.assetid: c5eb3fa0-550c-4a2f-a0bc-698b690c4199
title: Planejar acesso condicional com base em dispositivo no local
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: f3850454f9e2e426ce2d00112adf90f0d2530d8f
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86964738"
---
# <a name="plan-device-based-conditional-access-on-premises"></a>Planejar acesso condicional com base em dispositivo no local


Este documento descreve as políticas de acesso condicional com base em dispositivos em um cenário híbrido em que os diretórios locais são conectados ao Azure AD usando Azure AD Connect.     

## <a name="ad-fs-and-hybrid-conditional-access"></a>AD FS e acesso condicional híbrido  

O AD FS fornece o componente local das políticas de acesso condicional em um cenário híbrido.  Quando você registra dispositivos com o Azure AD para acesso condicional a recursos de nuvem, a Azure AD Connect capacidade de write-back do dispositivo disponibiliza as informações de registro do dispositivo localmente para que AD FS políticas consumam e imponham.  Dessa forma, você tem uma abordagem consistente para acessar políticas de controle para recursos locais e na nuvem.  

![acesso condicional](media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  

### <a name="types-of-registered-devices"></a>Tipos de dispositivos registrados  
Há três tipos de dispositivos registrados, todos os quais são representados como objetos de dispositivo no Azure AD e podem ser usados para acesso condicional com AD FS local também.  

| |Adicionar conta corporativa ou de estudante  |Ingresso no AD do Azure  |Ingresso no domínio do Windows 10    
| --- | --- |--- | --- |
|Descrição    |  Os usuários adicionam sua conta corporativa ou de estudante ao seu dispositivo BYOD interativamente.  **Observação:** Adicionar conta corporativa ou de estudante é a substituição para Workplace Join no Windows 8/8.1       | Os usuários ingressam no dispositivo de trabalho do Windows 10 no Azure AD.|Os dispositivos ingressados no domínio do Windows 10 se registram automaticamente no Azure AD.|           
|Como os usuários fazem logon no dispositivo     |  Não há logon no Windows como a conta corporativa ou de estudante.  Faça logon usando um conta Microsoft.       |   Faça logon no Windows como a conta (corporativa ou de estudante) que registrou o dispositivo.      |     Faça logon usando a conta do AD.|      
|Como os dispositivos são gerenciados    |      Políticas de MDM (com registro adicional do Intune)   | Políticas de MDM (com registro adicional do Intune)        |   Política de Grupo, Configuration Manager |
|Tipo de confiança do Azure AD|Ingressado no local de trabalho|Adicionado ao Azure AD|Ingressado no domínio  |     
|Local das configurações do W10    | Configurações > contas > sua conta > adicionar uma conta corporativa ou de estudante        | Configurações > > do sistema sobre > ingressar no Azure AD       |   Configurações > > do sistema sobre > ingressar em um domínio |       
|Também disponível para dispositivos iOS e Android?   |    Sim     |       Não  |   Não   |   

  

Para obter mais informações sobre as diferentes maneiras de registrar dispositivos, consulte também:  
* [Usando dispositivos com Windows 10 no local de trabalho](/azure/active-directory/devices/overview)  
* [Configurando dispositivos Windows 10 para trabalho](https://jairocadena.com/2016/01/18/setting-up-windows-10-devices-for-work-domain-join-azure-ad-join-and-add-work-or-school-account/)  
[Adicionar o Windows 10 Mobile ao Azure Active Directory](/windows/client-management/join-windows-10-mobile-to-azure-active-directory)  

### <a name="how-windows-10-user-and-device-sign-on-is-different-from-previous-versions"></a>Como o usuário e o dispositivo Windows 10 entram em diferentes das versões anteriores  
Para o Windows 10 e o AD FS 2016, há alguns novos aspectos do registro e da autenticação do dispositivo que você deve saber (especialmente se você estiver muito familiarizado com o registro de dispositivos e "ingresso no local de trabalho" em versões anteriores).  

Primeiro, no Windows 10 e no AD FS no Windows Server 2016, o registro e a autenticação do dispositivo não são mais baseados apenas em um certificado de usuário X509.  Há um protocolo novo e mais robusto que fornece melhor segurança e uma experiência de usuário mais direta.  As principais diferenças são que, para ingresso no domínio do Windows 10 e ingresso no Azure AD, há um certificado de computador X509 e uma nova credencial chamada de PRT.  Você pode ler tudo sobre ele [aqui](https://jairocadena.com/2016/01/18/how-domain-join-is-different-in-windows-10-with-azure-ad/) e [aqui](https://jairocadena.com/2016/02/01/azure-ad-join-what-happens-behind-the-scenes/).  

Em segundo lugar, o Windows 10 e o AD FS 2016 dão suporte à autenticação de usuário usando Microsoft Passport for Work, que você pode ler [aqui](https://jairocadena.com/2016/03/09/azure-ad-and-microsoft-passport-for-work-in-windows-10/) e [aqui](/windows/security/identity-protection/hello-for-business/hello-identity-verification).  

O AD FS 2016 fornece um SSO de usuário e dispositivo contínuo com base nas credenciais de PRT e do Passport.  Usando as etapas deste documento, você pode habilitar esses recursos e vê-los funcionando.  

### <a name="device-access-control-policies"></a>Políticas de controle de acesso do dispositivo  
Os dispositivos podem ser usados em regras simples de controle de acesso de AD FS, como:  

- permitir acesso somente de um dispositivo registrado   
- exigir autenticação multifator quando um dispositivo não estiver registrado  

Essas regras podem então ser combinadas com outros fatores, como o local de acesso à rede e a autenticação multifator, criando políticas de acesso condicional avançadas, como:  


- exigir autenticação multifator para dispositivos não registrados que acessam de fora da rede corporativa, exceto para membros de um grupo ou grupos específicos  

Com o AD FS 2016, essas políticas podem ser configuradas especificamente para exigir um nível de confiança de dispositivo específico também: **autenticado**, **gerenciado**ou em **conformidade**.  

Para obter mais informações sobre como configurar políticas de controle de acesso AD FS, consulte [políticas de controle de acesso em AD FS](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md).  

#### <a name="authenticated-devices"></a>Dispositivos autenticados  
Dispositivos autenticados são dispositivos registrados que não estão registrados no MDM (Intune e MDMs de terceiros para Windows 10, Intune somente para iOS e Android).   

Os dispositivos autenticados terão a declaração de AD FS **IsManaged** com o valor **false**. (Enquanto os dispositivos que não estão registrados não terão essa declaração.)  Dispositivos autenticados (e todos os dispositivos registrados) terão a declaração Issei AD FS com o valor **true**.  

#### <a name="managed-devices"></a>Dispositivos gerenciados:   

Os dispositivos gerenciados são dispositivos registrados no MDM.  

Os dispositivos gerenciados terão a declaração de AD FS isgerencied com o valor **true**.  

#### <a name="devices-compliant-with-mdm-or-group-policies"></a>Dispositivos em conformidade (com políticas de MDM ou de grupo)  
Os dispositivos em conformidade são dispositivos registrados que não só são registrados com o MDM, mas estão em conformidade com as políticas de MDM. (As informações de conformidade originam-se no MDM e são gravadas no Azure AD.)  

Os dispositivos em conformidade terão a Declaração **IsCompliant** AD FS com o valor **true**.    

Para obter uma lista completa de dispositivos AD FS 2016 e declarações de acesso condicional, consulte [referência](#reference).  


## <a name="reference"></a>Referência  
#### <a name="complete-list-of-new-ad-fs-2016-and-device-claims"></a>Lista completa de novos AD FS 2016 e declarações de dispositivo  

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
