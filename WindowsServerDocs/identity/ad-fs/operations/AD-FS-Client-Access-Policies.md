---
title: Políticas de controle de acesso de cliente no AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 4d7d45b91e866b9df927620f2e214ced248b3361
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87947482"
---
# <a name="controlling-access-to-organizational-data-with-active-directory-federation-services"></a>Controlando o acesso a dados organizacionais com Serviços de Federação do Active Directory (AD FS)

Este documento fornece uma visão geral do controle de acesso com AD FS entre cenários locais, híbridos e de nuvem.

## <a name="ad-fs-and-conditional-access-to-on-premises-resources"></a>AD FS e acesso condicional a recursos locais
Desde a introdução do Serviços de Federação do Active Directory (AD FS), as políticas de autorização estão disponíveis para restringir ou permitir que os usuários acessem recursos com base em atributos da solicitação e no recurso.  Como AD FS foi movido de versão para versão, a forma como essas políticas são implementadas mudou.  Para obter informações detalhadas sobre os recursos de controle de acesso por versão, consulte:
- [Políticas de controle de acesso no AD FS no Windows Server 2016](Access-Control-Policies-in-AD-FS.md)
- [Controle de acesso no AD FS no Windows Server 2012 R2](Manage-Risk-with-Conditional-Access-Control.md)


## <a name="ad-fs-and-conditional-access-in-a-hybrid-organization"></a>AD FS e acesso condicional em uma organização híbrida

AD FS fornece o componente local da política de acesso condicional em um cenário híbrido. As regras de autorização baseadas em AD FS devem ser usadas para recursos não Azure AD, como aplicativos locais federados diretamente para AD FS.  O componente de nuvem é fornecido pelo [acesso condicional do Azure ad](/azure/active-directory/active-directory-conditional-access).  Azure AD Connect fornece o plano de controle conectando os dois.

Por exemplo, quando você registra dispositivos com o Azure AD para acesso condicional a recursos de nuvem, a Azure AD Connect capacidade de write-back do dispositivo disponibiliza informações de registro do dispositivo localmente para que AD FS políticas consumam e imponham.  Dessa forma, você tem uma abordagem consistente para acessar políticas de controle para recursos locais e na nuvem.

![acesso condicional](../deployment/media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)


### <a name="the-evolution-of-client-access-policies-for-office-365"></a>A evolução das políticas de acesso para cliente para o Office 365
Muitos de vocês estão usando políticas de acesso de cliente com AD FS para limitar o acesso ao Office 365 e a outros serviços online da Microsoft com base em fatores como o local do cliente e o tipo de aplicativo cliente que está sendo usado.
- [Políticas de acesso para cliente no Windows Server 2012 R2 AD FS](Access-Control-Policies-W2K12.md)
- [Políticas de acesso para cliente no AD FS 2,0](Access-Control-Policies-in-AD-FS-2.md)

Alguns exemplos dessas políticas incluem:
- Bloquear todo o acesso de cliente de extranet ao Office 365
- Bloquear todo o acesso de cliente de extranet para o Office 365, exceto para dispositivos que acessam o Exchange Online para Exchange Active Sync

Geralmente, a necessidade subjacente por trás dessas políticas é reduzir o risco de perda de dados, garantindo que somente clientes autorizados, aplicativos que não armazenam dados em cache ou dispositivos que podem ser desabilitados remotamente possam obter acesso aos recursos.

Embora as políticas documentadas acima para AD FS funcionem nos cenários específicos documentados, eles têm limitações porque dependem de dados do cliente que não estão disponíveis de forma consistente.  Por exemplo, a identidade do aplicativo cliente só está disponível para serviços baseados no Exchange Online e não para recursos como o SharePoint Online, em que os mesmos dados podem ser acessados por meio do navegador ou de um ' cliente espesso ', como o Word ou o Excel.  Além disso AD FS não está ciente do recurso no Office 365 que está sendo acessado, como o SharePoint Online ou o Exchange Online.

Para resolver essas limitações e fornecer uma maneira mais robusta de usar políticas para gerenciar o acesso a dados corporativos no Office 365 ou em outros recursos baseados no Azure AD, a Microsoft introduziu o acesso condicional do Azure AD.  As políticas de acesso condicional do Azure AD podem ser configuradas para um recurso específico ou para qualquer ou todos os recursos no Office 365, SaaS ou aplicativos personalizados no Azure AD.  Essas políticas dinamizam em relação à confiança do dispositivo, local e outros fatores.

Para saber mais sobre o acesso condicional do Azure AD, consulte [acesso condicional no Azure Active Directory](/azure/active-directory/active-directory-conditional-access)

Uma alteração importante ao habilitar esses cenários é a [autenticação moderna](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/), uma nova maneira de autenticar usuários e dispositivos que funciona da mesma maneira em clientes do Office, Skype, Outlook e navegadores.

## <a name="next-steps"></a>Próximas etapas
Para obter mais informações sobre como controlar o acesso na nuvem e no local, consulte:

- [Acesso condicional no Azure Active Directory](/azure/active-directory/active-directory-conditional-access)
- [Políticas de controle de acesso no AD FS 2016](Access-Control-Policies-in-AD-FS.md)
