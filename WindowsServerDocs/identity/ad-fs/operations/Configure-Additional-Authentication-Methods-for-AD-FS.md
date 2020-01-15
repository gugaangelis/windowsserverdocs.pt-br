---
ms.assetid: ddfbbda3-30ca-4537-af12-667efc6f63ff
title: Configure métodos de autenticação adicionais para AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/26/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 89a1f745e1e928a5e5bd79adb94e41fd9da399d9
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75948541"
---
# <a name="configure-additional-authentication-methods-for-ad-fs"></a>Configure métodos de autenticação adicionais para AD FS

Para habilitar o Multi-Factor Authentication (MFA), você deve selecionar pelo menos um método de autenticação adicional. Por padrão, no Serviços de Federação do Active Directory (AD FS) (AD FS) no Windows Server 2012 R2, você pode selecionar autenticação de certificado (em outras palavras, autenticação baseada em cartão inteligente) como um método de autenticação adicional.

> [!NOTE]
> Se você selecionar a Autenticação de Certificado, verifique se os certificados de cartão inteligente foram provisionados de forma segura e se têm requisitos de PIN.

Você sabia que o Microsoft Azure fornece uma funcionalidade semelhante na nuvem? Saiba mais sobre as [Soluções de identidade do Microsoft Azure](https://aka.ms/m2w274).<br /><br />Crie uma solução de identidade híbrida no Microsoft Azure:<br /> - [saiba mais sobre a autenticação multifator do Azure.](https://aka.ms/ey6o9r)<br /> - [gerenciar identidades para ambientes híbridos de floresta única usando a autenticação de nuvem.](https://aka.ms/g1jat8)<br /> - [gerenciar riscos com a autenticação multifator adicional para aplicativos confidenciais.](https://aka.ms/kt1bbm)

## <a name="microsoft-and-third-party-additional-authentication-methods"></a>Métodos de autenticação adicionais de terceiros e da Microsoft
Você também pode configurar e habilitar métodos de autenticação da Microsoft e de terceiros no AD FS no Windows Server 2012 R2. Uma vez instalado e registrado com AD FS, você pode impor a MFA como parte da política de autenticação global ou de terceiros.

Segue abaixo uma lista em ordem alfabética dos provedores da Microsoft e de terceiros com ofertas AMF atualmente disponíveis para AD FS no Windows Server 2012 R2.

|Provedor|Oferta|Link para saber mais|
|-|-|-| 
|aPersona|Autenticação multifator aPersona adaptável para SSO do Microsoft ADFS|[Adaptador ADFS aPersona ASM](https://www.apersona.com/adfs)|
|Duo Security|Adaptador de MFA do duo para AD FS|[Autenticação Duo para AD FS](https://duo.com/docs/adfs)|
|Futurae|Pacote de autenticação futurae para AD FS|[Autenticação forte do futurae](https://futurae.com)|
|Gemalto|Identidade e Serviços de Segurança da Gemalto|[http://www.gemalto.com/identity](http://www.gemalto.com/identity)|
|Tecnologias inWebo|Serviço inWebo Enterprise Authentication|[Autenticação corporativa do inWebo](http://www.inwebo.com)|
|Login People|Conector API de Login People MFA para AD FS 2012 R2 (beta público)|[https://www.loginpeople.com](https://www.loginpeople.com)|
|Microsoft Corp.|Microsoft Azure MFA|[Guia Passo a Passo: Gerenciar Riscos com a Autenticação Multifator Adicional para Aplicativos Confidenciais](https://technet.microsoft.com/library/dn280946.aspx) (consulte a etapa 3)|
Mideye | Provedor de autenticação Mideye para ADFS | [Mideye autenticação de dois fatores com o Microsoft Active Directory Serviço de Federação](https://www.mideye.com/support/administrators/documentation/integration/microsoft-adfs/)|
|Okta | Okta MFA para Serviços de Federação do Active Directory (AD FS) | [Okta MFA para Serviços de Federação do Active Directory (AD FS) (ADFS)](https://help.okta.com/en/prod/Content/Topics/integrations/adfs-okta-int.htm)|
|One Identity| Starling 2FA AD FS|[Adaptador de AD FS Starling 2FA](https://www.oneidentity.com/products/starling-two-factor-authentication/)|
|One Identity| Defender AD FS|[Adaptador de AD FS do defender](https://www.oneidentity.com/products/defender/)|
|Ping Identity|Adaptador de MFA de pingid para AD FS|[Adaptador de MFA de pingid para AD FS](https://documentation.pingidentity.com/pingid/pingidAdminGuide/index.shtml#pid_c_PingIDforADFSSSO.html)|
|RSA, A Divisão de Segurança da EMC|RSA SecurID Authentication Agent para Serviços de Federação do Microsoft Active Directory|[Agente de autenticação RSA SecurID para Microsoft Serviços de Federação do Active Directory (AD FS)](http://www.emc.com/security/rsa-securid/rsa-authentication-agents/microsoft-ad-fs.htm)|
|SafeNet, Inc.|SafeNet Authentication Service (SAS) Agent para AD FS|[Serviço de autenticação SafeNet: guia de configuração do agente de AD FS](http://www.safenet-inc.com/resources/integration-guide/data-protection/Safenet_Authentication_Service/SafeNet_Authentication_Service__AD_FS_Agent_Configuration_Guide/?langtype=1033)|
|SecureMFA|Provedor de OTP SecureMFA| [Provedores de autenticação multifator do ADFS](https://www.securemfa.com/)|
|Swisscom|Serviço de Autenticação de ID Móvel e Serviços de Assinatura|[Serviço de autenticação de ID móvel](http://swisscom.ch/mid)|
|Symantec|Symantec Validation e ID Protection Service (VIP)|[Protocolo VIP (validação e serviço de proteção de ID) da Symantec](http://www.symantec.com/vip-authentication-service)|
|Trusona|Essencial (MFA com senha) e executivo (essencial + verificação de identidade)| [Autenticação multifator Trusona](https://www.trusona.com/solution-overview/)|


## <a name="custom-authentication-method-for-ad-fs-in-windows-server-2012-r2"></a>Método de autenticação personalizado para AD FS em Windows Server 2012 R2
Agora, nós fornecemos instruções para construir o seu próprio método de autenticação personalizado para AD FS no Windows Server 2012 R2. Para obter mais informações, consulte [Compilar um Método de autenticação personalizado para AD FS no Windows Server 2012 R2](https://go.microsoft.com/fwlink/?LinkID=511980).

## <a name="see-also"></a>Veja também
[Gerenciar riscos com Autenticação Multifator adicional para aplicativos confidenciais](Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)


