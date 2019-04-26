---
ms.assetid: ''
title: Políticas de controle de acesso de cliente no AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cc91cd9a446c8ca30471b65374ca99a7bd49d369
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882367"
---
# <a name="controlling-access-to-organizational-data-with-active-directory-federation-services"></a>Controlando o acesso aos dados organizacionais com serviços de Federação do Active Directory

Este documento fornece uma visão geral do controle de acesso com o AD FS entre locais, cenários de nuvem e híbridos.  

## <a name="ad-fs-and-conditional-access-to-on-premises-resources"></a>AD FS e o acesso condicional aos recursos locais 
Desde a introdução dos serviços de Federação do Active Directory, as políticas de autorização estão disponíveis para restringir ou permitir aos usuários acesso a recursos com base nos atributos da solicitação e o recurso.  Como o AD FS foi movida com a versão, como essas políticas são implementadas foi alterado.  Para obter informações detalhadas sobre os recursos de controle de acesso por versão, consulte:
- [Políticas de controle de acesso no AD FS no Windows Server 2016](Access-Control-Policies-in-AD-FS.md)
- [Controle de acesso no AD FS no Windows Server 2012 R2](Manage-Risk-with-Conditional-Access-Control.md)


## <a name="ad-fs-and-conditional-access-in-a-hybrid-organization"></a>O AD FS e o acesso condicional em uma organização híbrida  

O AD FS fornece o componente local diante da política de acesso condicional em um cenário híbrido. Regras de autorização do AD FS com base devem ser usadas para os recursos não Azure AD, como no local aplicativos federados diretamente a AD FS.  O componente de nuvem é fornecido pela [acesso condicional do Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access).  Azure AD Connect fornece o plano de controle, conectar os dois.

Por exemplo, ao registrar dispositivos com o Azure AD para acesso condicional aos recursos de nuvem, o dispositivo do Azure AD Connect write-back recurso disponibiliza as informações de registro de dispositivo no local para as políticas do AD FS consumir e impor.  Dessa forma, você tem uma abordagem consistente para políticas de controle de acesso para ambos os locais e recursos de nuvem.  

![Acesso condicional](../deployment/media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  


### <a name="the-evolution-of-client-access-policies-for-office-365"></a>A evolução das políticas de acesso de cliente para o Office 365
Muitos de vocês estão usando políticas de acesso de cliente com o AD FS para limitar o acesso ao Office 365 e outros serviços Online da Microsoft com base em fatores como o local do cliente e o tipo de aplicativo cliente que está sendo usado.  
- [Políticas de acesso do cliente no AD FS do Windows Server 2012 R2](Access-Control-Policies-W2K12.md)
- [Políticas de acesso do cliente no AD FS 2.0](Access-Control-Policies-in-AD-FS-2.md)

Alguns exemplos dessas políticas incluem:
- Bloquear todo o acesso à extranet do cliente para o Office 365
- Bloquear todo o acesso extranet do cliente para o Office 365, exceto para dispositivos que acessam o Exchange Online para Exchange Active Sync

Geralmente, a necessidade de subjacente por trás dessas políticas é reduzir o risco de vazamento de dados garantindo que somente clientes autorizados, aplicativos que não armazena em cache os dados, ou dispositivos que podem ser desabilitados remotamente podem obter acesso aos recursos.

Enquanto as políticas documentadas acima para o AD FS trabalham em cenários específicos documentados, eles têm limitações porque dependem de dados do cliente que não estejam consistentemente disponíveis.  Por exemplo, a identidade do aplicativo cliente só está disponível para serviços do Exchange Online com base e não para recursos como o SharePoint Online, onde os mesmos dados podem ser acessados por meio do navegador ou um cliente fino, como o Word ou Excel.  Além disso, o AD FS é ciente do recurso sendo acessado, como o SharePoint Online ou Exchange Online do Office 365.

Para superar essas limitações e fornecem uma maneira mais eficiente usar políticas para gerenciar o acesso aos dados corporativos no Office 365 ou outros recursos do Azure AD com base, a Microsoft introduziu o acesso condicional do Azure AD.  As políticas de acesso condicional do AD do Azure podem ser configuradas para um recurso específico, ou para qualquer ou todos os recursos dentro do Office 365, SaaS ou aplicativos personalizados no Azure AD.  Essas políticas de dinamizar a relação de confiança do dispositivo, localização e outros fatores.

Para obter mais informações sobre o acesso condicional do Azure AD, consulte [acesso condicional no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access)

É uma alteração de chave habilitar estes cenários [autenticação moderna](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/), uma nova maneira de autenticar usuários e dispositivos que funciona da mesma maneira em clientes do Office, Skype, Outlook e navegadores.

## <a name="next-steps"></a>Próximas etapas
Para obter mais informações sobre como controlar o acesso entre a nuvem e no local, consulte:

- [Acesso condicional no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access)
- [Políticas de controle de acesso no AD FS 2016](Access-Control-Policies-in-AD-FS.md)
