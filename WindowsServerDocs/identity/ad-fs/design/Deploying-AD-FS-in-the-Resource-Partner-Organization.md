---
ms.assetid: 41d6b897-1e72-4522-aad6-eece1154a154
title: Implantando AD FS herdados na organização do parceiro de recurso
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 63afcbf825aadcc8207793e6e7ccb3cdc48cacbb
ms.sourcegitcommit: de8fea497201d8f3d995e733dfec1d13a16cb8fa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87863789"
---
# <a name="deploying-legacy-ad-fs-in-the-resource-partner-organization"></a>Implantando AD FS herdados na organização do parceiro de recurso

A organização do parceiro de recurso no Serviços de Federação do Active Directory (AD FS) \( AD FS \) representa a organização cujos servidores Web podem ser protegidos por um \- servidor de Federação do lado do recurso. O servidor de Federação no parceiro de recurso usa os tokens de segurança que são produzidos pelo parceiro de conta para fornecer declarações aos servidores Web que estão localizados no parceiro de recurso.  
  
Em cenários em que você precisa fornecer acesso a serviços federados ou aplicativos a muitos usuários diferentes — quando alguns usuários residem em diferentes organizações — você pode configurar o servidor de Federação de recursos para que possa implantar vários parceiros de conta.  
  
Para obter mais informações sobre como instalar e configurar a organização do parceiro de recurso, consulte [Checklist: Configuring the Resource Partner Organization](../../ad-fs/deployment/Checklist--Configuring-the-Resource-Partner-Organization.md).  
  
## <a name="in-this-section"></a>Nesta seção  
  
-   [Analisar a função do servidor de federação no parceiro de recurso](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md)  
  
-   [Analisar a função do proxy do servidor de federação no parceiro de recurso](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Resource-Partner.md)  
  
-   [Determinar sua estratégia de aplicativo federado no parceiro de recurso](Determine-Your-Federated-Application-Strategy-in-the-Resource-Partner.md)  
  

## <a name="see-also"></a>Consulte Também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
