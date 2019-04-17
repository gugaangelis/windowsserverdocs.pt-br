---
ms.assetid: 
title: "Políticas de controle de acesso do cliente no AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5e1e1b907e6fccbf2b9906106d3360bd9a6fd69d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="controlling-access-to-organizational-data-with-active-directory-federation-services"></a>Controlar o acesso aos dados organizacionais com serviços de Federação do Active Directory

Este documento fornece uma visão geral do controle de acesso com o AD FS no local, cenários de nuvem híbrida.  

## <a name="ad-fs-and-conditional-access-to-on-premises-resources"></a>AD FS e acesso condicional a recursos locais 
Desde o lançamento dos serviços de Federação do Active Directory, as políticas de autorização estão disponíveis para restringir ou permitir que os usuários acessem recursos com base em atributos da solicitação e o recurso.  Como o AD FS foi movido para cada versão, como estas políticas são implementadas foi alterada.  Para obter informações detalhadas sobre os recursos de controle de acesso por versão veja:
- [Políticas de controle de acesso no AD FS no Windows Server 2016](Access-Control-Policies-in-AD-FS.md)
- [Controle de acesso no AD FS no Windows Server 2012 R2](Manage-Risk-with-Conditional-Access-Control.md)


## <a name="ad-fs-and-conditional-access-in-a-hybrid-organization"></a>AD FS e acesso condicional em uma organização híbrida  

AD FS fornece o componente local diante da política de acesso condicional em um cenário híbrida. Regras de autorização do AD FS com base em devem ser usadas para recursos não Azure AD, como no local aplicativos federados diretamente para o AD FS.  O componente em nuvem é fornecido pelo [Azure AD Conditional Access](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access).  Azure AD Connect fornece o plano de controle conectando os dois.

Por exemplo, quando você registrar dispositivos com o Azure AD para acesso condicional a recursos de nuvem, o dispositivo do Azure AD Connect gravar de volta recurso faz com que as informações de registro de dispositivo disponível no local para políticas do AD FS consumir e impor.  Dessa forma, você tem uma abordagem consistente para políticas de controle de acesso para ambos no local e recursos na nuvem.  

![Acesso condicional](../deployment/media/Plan-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  


### <a name="the-evolution-of-client-access-policies-for-office-365"></a>A evolução das políticas de acesso do cliente para o Office 365
Muitos de vocês estão usando políticas de acesso do cliente com o AD FS para limitar o acesso ao Office 365 e outros serviços Online da Microsoft com base em fatores como o local do cliente e o tipo de aplicativo cliente que está sendo usado.  
- [Políticas de acesso do cliente no Windows Server 2012 R2 AD FS](Access-Control-Policies-W2K12.md)
- [Políticas de acesso do cliente no AD FS 2.0](Access-Control-Policies-in-AD-FS-2.md)

Alguns exemplos dessas políticas incluem:
- Bloquear todo o acesso de cliente extranet para o Office 365
- Bloquear todo o acesso extranet de cliente para o Office 365, exceto para dispositivos que acessam o Exchange Online para Exchange Active Sync

Muitas vezes subjacente por trás dessas políticas é necessário reduzir os riscos de perda de dados, garantindo apenas clientes autorizados, aplicativos que não armazenar em cache os dados, ou dispositivos que podem ser desabilitados remotamente podem obter acesso aos recursos.

Embora as políticas documentadas acima do AD FS funcionam nos cenários específicos documentados, eles têm limitações porque eles dependem dos dados de cliente que não esteja disponíveis consistentemente.  Por exemplo, a identidade do aplicativo cliente só foi disponibilizada para serviços do Exchange Online com base e não para recursos como SharePoint Online, onde os mesmos dados podem ser acessados por meio do navegador ou um cliente fino como Word ou Excel.  Além disso, o AD FS é sem reconhecimento do recurso sendo acessado, como o SharePoint Online ou o Exchange Online do Office 365.

Para lidar com essas limitações e fornecer uma maneira mais eficiente usar políticas para gerenciar o acesso aos dados comerciais no Office 365 ou outros recursos do Azure AD com base, a Microsoft introduziu o Azure AD Conditional Access.  Políticas de acesso condicional do AD Azure podem ser configuradas para um recurso específico, ou para qualquer ou todos os recursos no Office 365, SaaS ou aplicativos personalizados no Azure AD.  Essas políticas de pivô em confiança no dispositivo, localização e outros fatores.

Para saber mais sobre o Azure AD Conditional Access, consulte [acesso condicional no Active Directory do Azure](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access)

Uma alteração de chave habilitar esses cenários é [autenticação moderna](https://blogs.office.com/2015/11/19/updated-office-365-modern-authentication-public-preview/), uma nova maneira de autenticação de usuários e dispositivos que funciona da mesma maneira em todos os clientes do Office, Skype, Outlook e navegadores.

## <a name="next-steps"></a>Próximas etapas
Para obter mais informações sobre como controlar o acesso em nuvem e no local, veja:

- [Acesso condicional no Active Directory do Azure](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-conditional-access)
- [Políticas de controle de acesso no AD FS 2016](Access-Control-Policies-in-AD-FS.md)
