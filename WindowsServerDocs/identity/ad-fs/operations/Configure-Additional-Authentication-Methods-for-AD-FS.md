---
ms.assetid: ddfbbda3-30ca-4537-af12-667efc6f63ff
title: Configure métodos de autenticação adicionais para AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 10/04/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 73de8908677b3f74651b10c29ef2abe62e484694
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868407"
---
# <a name="configure-additional-authentication-methods-for-ad-fs"></a>Configure métodos de autenticação adicionais para AD FS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Para habilitar o Multi-Factor Authentication (MFA), você deve selecionar pelo menos um método de autenticação adicional. Por padrão, no Active Directory Federation Services (AD FS) no Windows Server 2012 R2, você pode selecionar a autenticação de certificado (em outras palavras, autenticação baseada em cartão inteligente) como um método de autenticação adicional.

> [!NOTE]
> Se você selecionar a Autenticação de Certificado, verifique se os certificados de cartão inteligente foram provisionados de forma segura e se têm requisitos de PIN.

Você sabia que o Microsoft Azure fornece uma funcionalidade semelhante na nuvem? Saiba mais sobre as [Soluções de identidade do Microsoft Azure](http://aka.ms/m2w274).<br /><br />Criar uma solução de identidade híbrida no Microsoft Azure:<br /> - [Saiba mais sobre a autenticação multifator do Azure.](http://aka.ms/ey6o9r)<br /> - [Gerencie identidades para ambientes híbridos de floresta única usando a autenticação de nuvem.](http://aka.ms/g1jat8)<br /> - [Gerencie riscos com autenticação multifator adicional para aplicativos confidenciais.](http://aka.ms/kt1bbm)

## <a name="microsoft-and-third-party-additional-authentication-methods"></a>Métodos de autenticação adicionais de terceiros e da Microsoft
Você também pode configurar e habilitar métodos de autenticação de terceiros no AD FS no Windows Server 2012 R2 e Microsoft. Depois de instalado e registrado com o AD FS, você pode impor MFA como parte da política de autenticação global ou por parte confiável.

Segue abaixo uma lista em ordem alfabética dos provedores da Microsoft e de terceiros com ofertas AMF atualmente disponíveis para AD FS no Windows Server 2012 R2.

|Provedor|Oferta|Link para saber mais|
|-|-|-| 
|aPersona|aPersona adaptável a autenticação multifator para que o SSO do Microsoft ADFS|[aPersona ASM adaptador do AD FS](https://www.apersona.com/adfs)|
|Segurança Duo|Adaptador do MFA Duo para o AD FS|[Duo autenticação do AD FS](https://duo.com/docs/adfs)|
|Gemalto|Identidade e Serviços de Segurança da Gemalto|[http://www.gemalto.com/identity](http://www.gemalto.com/identity)|
|Tecnologias inWebo|Serviço inWebo Enterprise Authentication|[inWebo Enterprise autenticação](http://www.inwebo.com)|
|Login People|Conector API de Login People MFA para AD FS 2012 R2 (beta público)|[https://www.loginpeople.com](https://www.loginpeople.com)|
|Microsoft Corp.|Microsoft Azure MFA|[Guia passo a passo: Gerenciando riscos com a Multi-Factor Authentication adicional para aplicativos sensíveis](https://technet.microsoft.com/library/dn280946.aspx) (consulte a etapa 3)|
Mideye | Provedor de autenticação mideye para ADFS | [Mideye autenticação de dois fatores com o serviço de Federação do Microsoft Active Directory](https://www.mideye.com/support/administrators/documentation/integration/microsoft-adfs/)|
|Okta | MFA Okta para serviços de Federação do Active Directory | [Serviços de MFA do Okta para federação do Active Directory (ADFS)](https://help.okta.com/en/prod/Content/Topics/integrations/adfs-okta-int.htm)|
|Uma identidade| Starling 2FA do AD FS|[Starling 2FA adaptador do AD FS](https://www.oneidentity.com/products/starling-two-factor-authentication/)|
|Uma identidade| Defender AD FS|[Adaptador do Defender AD FS](https://www.oneidentity.com/products/defender/)|
|Identidade de ping|MFA PingID adaptador do AD FS|[MFA PingID adaptador do AD FS](https://documentation.pingidentity.com/pingid/pingidAdminGuide/index.shtml#pid_c_PingIDforADFSSSO.html)|
|RSA, A Divisão de Segurança da EMC|RSA SecurID Authentication Agent para Serviços de Federação do Microsoft Active Directory|[RSA SecurID Authentication Agent para serviços de Federação do Microsoft Active Directory](http://www.emc.com/security/rsa-securid/rsa-authentication-agents/microsoft-ad-fs.htm)|
|SafeNet, Inc.|SafeNet Authentication Service (SAS) Agent para AD FS|[SafeNet Authentication Service: Guia de configuração de agente do AD FS](http://www.safenet-inc.com/resources/integration-guide/data-protection/Safenet_Authentication_Service/SafeNet_Authentication_Service__AD_FS_Agent_Configuration_Guide/?langtype=1033)|
|SecureMFA|Provedor de OTP SecureMFA| [Provedores de autenticação multifator AD FS](https://www.securemfa.com/)|
|Swisscom|Serviço de Autenticação de ID Móvel e Serviços de Assinatura|[Serviço de autenticação de ID móvel](http://swisscom.ch/mid)|
|Symantec|Symantec Validation e ID Protection Service (VIP)|[Symantec Validation e ID Protection Service (VIP)](http://www.symantec.com/vip-authentication-service)|
|Trusona|Essential (sem senha MFA) e executivo (essencial + prova de identidade)| [A Trusona a autenticação multifator](https://www.trusona.com/solution-overview/)|


## <a name="custom-authentication-method-for-ad-fs-in-windows-server-2012-r2"></a>Método de autenticação personalizado para AD FS em Windows Server 2012 R2
Agora, nós fornecemos instruções para construir o seu próprio método de autenticação personalizado para AD FS no Windows Server 2012 R2. Para obter mais informações, consulte [Compilar um Método de autenticação personalizado para AD FS no Windows Server 2012 R2](https://go.microsoft.com/fwlink/?LinkID=511980).

## <a name="see-also"></a>Consulte também
[Gerencie riscos com autenticação multifator adicional para aplicativos confidenciais](Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)


