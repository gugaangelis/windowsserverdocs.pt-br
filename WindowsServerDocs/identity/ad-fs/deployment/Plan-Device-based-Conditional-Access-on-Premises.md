---
ms.assetid: c5eb3fa0-550c-4a2f-a0bc-698b690c4199
title: Planeje o acesso condicional baseado no dispositivo local
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 6303bba92ce4f4f99ae8e5095060b06885e427d6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="plan-device-based-conditional-access-on-premises"></a>Planeje o acesso condicional baseado no dispositivo local

>Aplica-se a: Windows Server 2016

Este documento descreve as políticas de acesso condicional com base em dispositivos em um cenário de híbrido onde os diretórios locais estão conectados ao Azure AD usando Azure AD Connect.     

## <a name="ad-fs-and-hybrid-conditional-access"></a>AD FS e híbrido acesso condicional  

AD FS fornece o componente local diante das políticas de acesso condicional em um cenário híbrida.  Quando você registrar dispositivos com o Azure AD para acesso condicional a recursos de nuvem, o dispositivo do Azure AD Connect escrever volta recurso faz com que as informações de registro de dispositivo disponível no local para políticas do AD FS consumir e impor.  Dessa forma, você tem uma abordagem consistente para políticas de controle de acesso para ambos no local e recursos na nuvem.  

![Acesso condicional](media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  

### <a name="types-of-registered-devices"></a>Tipos de dispositivos registrados  
Há três tipos de dispositivos registrados, todos que são representados como objetos de dispositivo no Azure AD e podem ser usados para acesso condicional com o AD FS no local também.  

| |Adicione o trabalho ou à escola conta  |Ingresso do Azure AD  |Ingresso Domian do Windows 10    
| --- | --- |--- | --- |
|Descrição    |  Os usuários adicionar seu trabalho ou escola conta para seus dispositivos BYOD interativamente.  **Observação:** Adicionar trabalho ou escola conta é o substituto do Workplace Join no Windows 8/8.1       | Os usuários ingressar o dispositivo de trabalho do Windows 10 no Azure AD.|Dispositivos de ingressado no domínio do Windows 10 automaticamente se registrar com o Azure AD.|           
|Como os usuários fazer logon dispositivo     |  Nenhum logon Windows como a conta corporativa ou escolar.  Faça logon usando uma conta da Microsoft.       |   Faça logon Windows como a conta (corporativa ou escolar) que registrou o dispositivo.      |     Faça logon usando a conta do AD.|      
|Como os dispositivos sejam gerenciados    |      Políticas de MDM (com o registro do Intune adicional)   | Políticas de MDM (com o registro do Intune adicional)        |   Política de grupo, System Center Configuration Manager (SCCM) |
|Tipo do Azure AD confiança|Local de trabalho ingressado|Azure AD associado|Ingressado no domínio  |     
|W10 configurações localização    | Configurações > contas > sua conta > Adicionar uma conta corporativa ou escolar        | Configurações > sistema > sobre > ingressar no Azure AD       |   Configurações > sistema > sobre > ingressar em um domínio |       
|Também está disponível para dispositivos iOS e Android?   |    Sim     |       Não  |   Não   |   

  

Para obter mais informações sobre as diferentes maneiras de registrar dispositivos, veja também:  
* [Usando dispositivos Windows 10 em seu local de trabalho](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-windows10-devices/)  
* [Configurar dispositivos Windows 10 para trabalho](https://jairocadena.com/2016/01/18/setting-up-windows-10-devices-for-work-domain-join-azure-ad-join-and-add-work-or-school-account/)  
[Participe do Windows 10 Mobile ao Azure Active Directory](https://technet.microsoft.com/itpro/windows/manage/join-windows-10-mobile-to-azure-active-directory)  

### <a name="how-windows-10-user-and-device-sign-on-is-different-from-previous-versions"></a>Como o usuário do Windows 10 e o logon do dispositivo é diferente de versões anteriores  
Para Windows 10 e do AD FS 2016, há alguns novos aspectos da autenticação e registro de dispositivo, você deve saber sobre (especialmente se você estiver familiarizado com o registro de dispositivos e "ingressar em local de trabalho" em versões anteriores).  

Primeiro, no Windows 10 e do AD FS no Windows Server 2016, autenticação e registro de dispositivo não é mais baseia-se exclusivamente em um X509 certificado de usuário.  Há um protocolo de novo e mais robusto que fornece a melhor segurança e uma melhor experiência de usuário.  As principais diferenças são que, para o ingresso no domínio do Windows 10 e ingressar Azure AD, há um X509 certificado de computador e uma nova credencial chamado um PRT.  Você pode ler tudo sobre o assunto [aqui](https://jairocadena.com/2016/01/18/how-domain-join-is-different-in-windows-10-with-azure-ad/) e [aqui](https://jairocadena.com/2016/02/01/azure-ad-join-what-happens-behind-the-scenes/).  

Segundo, o Windows 10 e o AD FS 2016 suportam autenticação do usuário usando o Microsoft Passport for Work, que você pode ler sobre [aqui](https://jairocadena.com/2016/03/09/azure-ad-and-microsoft-passport-for-work-in-windows-10/) e [aqui](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-passport-deployment/).  

AD FS 2016 fornece perfeito dispositivo e usuário SSO com base em credenciais PRT e o Passport.  Usando as etapas neste documento, você pode habilitar esses recursos e vê-los funcionar.  

### <a name="device-access-control-policies"></a>Políticas de controle de acesso do dispositivo  
Dispositivos podem ser usados nas regras de controle de acesso do AD FS simples, como:  

- permitir o acesso somente de um dispositivo registrado   
- exigir autenticação de fator de multi quando um dispositivo não está registrado.  

Essas regras, em seguida, podem ser combinadas com outros fatores, como o local de acesso de rede e a autenticação de fator multi, criação de políticas de acesso condicional avançada como:  


- exigir autenticação de fator de multi para registro cancelados dispositivos que acessam de fora da rede corporativa, exceto para membros de um determinado grupo ou grupos  

Com o AD FS 2016, essas políticas podem ser configuradas especificamente para exigir um nível de confiança do dispositivo específico também: qualquer **autenticados**, **gerenciado**, ou **compatível com**.  

Para obter mais informações sobre como configurar o AD FS políticas de controle de acesso, consulte [políticas de controle de acesso no AD FS](../../ad-fs/operations/Access-Control-Policies-in-AD-FS.md).  

#### <a name="authenticated-devices"></a>Dispositivos autenticados  
Dispositivos autenticados são dispositivos registrados que não estão registrados no MDM (Intune e terceiros 3ª MDMs para Windows 10, Intune apenas para iOS e Android).   

Dispositivos autenticados terão o **isManaged** AD FS reivindicar com valor **FALSE**. (Enquanto dispositivos que não são registrados perderá essa declaração.) Dispositivos autenticados (e dispositivos tudo registrados) terá o isKnown AD FS reivindicar com valor **TRUE**.  

#### <a name="managed-devices"></a>Dispositivos gerenciados:   

Dispositivos gerenciados são dispositivos registrados que estão registrados com o MDM.  

Dispositivos gerenciados terão o isManaged AD FS reivindicar com valor **TRUE**.  

#### <a name="devices-compliant-with-mdm-or-group-policies"></a>Dispositivos compatíveis (com políticas de grupo ou MDM)  
Dispositivos compatíveis com são dispositivos registrados que não estiverem apenas registrados com o MDM mas em conformidade com as políticas MDM. (As informações de conformidade se origina com o MDM e são gravadas no Azure AD.)  

Dispositivos compatíveis com terão o **isCompliant** AD FS reivindicar com valor **TRUE**.    

Para obter uma lista completa de dispositivo do AD FS 2016 e declarações de acesso condicional, veja [referência](#reference).  


## <a name="reference"></a>Referência  
#### <a name="complete-list-of-new-ad-fs-2016-and-device-claims"></a>Lista completa de novos declarações do AD FS 2016 e dispositivo  

* https://schemas.microsoft.com/ws/2014/01/identity/claims/anchorclaimtype  
* http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/implicitupn  
* https://schemas.microsoft.com/2014/03/psso  
* https://schemas.microsoft.com/2015/09/prt  
* http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/UPN  
* https://schemas.microsoft.com/ws/2008/06/identity/claims/primarygroupsid  
* https://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid  
* http://schemas.xmlsoap.org/ws/2005/05/Identity/Claims/Name  
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
