---
ms.assetid: ddfbbda3-30ca-4537-af12-667efc6f63ff
title: "Configurar métodos de autenticação adicional do AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 65baa6f94e1efa1fa337eab477b18f947cf0bb2d
ms.sourcegitcommit: 36d7b1dfd7da8e9f303d007a628e76149de000f2
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/21/2018
---
# <a name="configure-additional-authentication-methods-for-ad-fs"></a>Configurar métodos de autenticação adicional do AD FS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Para habilitar a autenticação multifator (MFA), você deve selecionar pelo menos um método de autenticação adicional. Por padrão, em serviços de Federação do Active Directory (AD FS) no Windows Server 2012 R2, você pode selecionar a autenticação de certificado (em outras palavras, autenticação com base em um cartão inteligente) como um método de autenticação adicional.

> [!NOTE]
> Se você selecionar a autenticação de certificado, certifique-se de que os certificados de cartão inteligente tem sido configurados com segurança e têm requisitos de pin.

Você sabia que o Microsoft Azure forneça funcionalidade semelhante na nuvem? Saiba mais sobre [soluções de identidade do Microsoft Azure ](http://aka.ms/m2w274).<br /><br />Crie uma solução de identidade híbrida no Microsoft Azure:<br /> - [Saiba mais sobre autenticação multifator Azure.](http://aka.ms/ey6o9r)<br /> - [Gerencie identidades para ambientes híbridos de floresta única usando a autenticação de nuvem.](http://aka.ms/g1jat8)<br /> - [Gerencie o risco com autenticação multifator adicional para aplicativos confidenciais.](http://aka.ms/kt1bbm)|

## <a name="microsoft-and-third-party-additional-authentication-methods"></a>Métodos de autenticação adicional Microsoft e serviços de terceiros
Você também pode configurar e permitir que a Microsoft e métodos de autenticação de terceiros no AD FS no Windows Server 2012 R2. Depois de instalado e registrado com o AD FS, você pode impor MFA como parte da política de autenticação global ou por confiar-terceiros.

Abaixo está uma lista alfabética da Microsoft e de fornecedores de terceiros com ofertas de MFA atualmente disponível do AD FS no Windows Server 2012 R2.

||||
|-|-|-|
|**Provedor**|**Oferta**|**Link para saber mais**|
|Gemalto|Serviços de segurança e identidade Gemalto|[http://www.gemalto.com/identity](http://www.gemalto.com/identity)|
|inWebo tecnologias|inWebo serviço de autenticação empresarial|[inWebo autenticação empresarial](http://www.inwebo.com)|
|Pessoas de logon|Conector de pessoas MFA API de logon para AD FS 2012 R2 (beta pública)|[https://www.loginpeople.com](https://www.loginpeople.com)|
|Microsoft Corp|MFA do Microsoft Azure|[Guia passo a passo: Gerenciar o risco com autenticação multifator adicional para aplicativos confidenciais](https://technet.microsoft.com/library/dn280946.aspx) (veja a etapa 3)|
|RSA, a divisão de segurança da EMC|Agente de autenticação RSA SecurID para serviços de Federação do Microsoft Active Directory|[Agente de autenticação RSA SecurID para serviços de Federação do Microsoft Active Directory](http://www.emc.com/security/rsa-securid/rsa-authentication-agents/microsoft-ad-fs.htm)|
|SafeNet, Inc.|Agente de serviço (SAS) de autenticação SafeNet do AD FS|[SafeNet o serviço de autenticação: Guia de configuração do AD FS agente](http://www.safenet-inc.com/resources/integration-guide/data-protection/Safenet_Authentication_Service/SafeNet_Authentication_Service__AD_FS_Agent_Configuration_Guide/?langtype=1033)|
|Swisscom|Serviço de autenticação de ID móvel e serviços de assinatura|[Serviço de autenticação de ID móvel](http://swisscom.ch/mid)|
|Symantec|Validação da Symantec e serviço de proteção de ID (VIP)|[Validação da Symantec e serviço de proteção de ID (VIP)](http://www.symantec.com/vip-authentication-service)|

## <a name="custom-authentication-method-for-ad-fs-in-windows-server-2012-r2"></a>Método de autenticação personalizado do AD FS no Windows Server 2012 R2
Agora podemos fornecer instruções para criar seu próprio método de autenticação personalizado do AD FS no Windows Server 2012 R2. Para obter mais informações, consulte [criar um método de autenticação personalizado para o AD FS no Windows Server 2012 R2](https://go.microsoft.com/fwlink/?LinkID=511980).

## <a name="see-also"></a>Consulte também
[Gerenciar o risco com autenticação multifator adicional para aplicativos confidenciais](Manage-Risk-with-Additional-Multi-Factor-Authentication-for-Sensitive-Applications.md)


