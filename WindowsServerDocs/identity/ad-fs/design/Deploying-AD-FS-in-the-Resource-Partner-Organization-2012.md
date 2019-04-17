---
ms.assetid: 39acccd9-0402-49ca-8ce1-b239e1e7e455
title: "Implantando o AD FS na organização do parceiro de recurso"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a20fab1cca4c33485fd599de5525c7a718e9598e
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="deploying-ad-fs-in-the-resource-partner-organization"></a>Implantando o AD FS na organização do parceiro de recurso

>Aplica-se a: Windows Server 2012

A organização de parceiro de recurso de serviços de Federação do Active Directory \(AD FS\) representa a organização cujos servidores Web podem estar protegidos por um servidor de Federação resource\ lado. O servidor de federação no parceiro de recurso usa os tokens de segurança que são gerados por parceiro de conta para fornecer declarações para os servidores Web que estão localizados no parceiro de recurso.  
  
Em cenários em que você precisa fornecer acesso aos serviços federados ou aplicativos para muitos usuários diferentes — quando alguns usuários residem em diferentes organizações — você pode configurar o servidor de Federação do recurso para que você pode implantar vários parceiros de conta.  
  
Para obter mais informações sobre como instalar e configurar uma organização de parceiro de recurso, consulte [lista de verificação: Configurando a organização do parceiro de recurso](../../ad-fs/deployment/Checklist--Configuring-the-Resource-Partner-Organization.md).  
  
## <a name="in-this-section"></a>Nesta seção  
  
-   [Examine a função do servidor de federação no parceiro de recurso](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md)  
  
-   [Examine a função do Proxy do servidor de federação no parceiro de recurso](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Resource-Partner.md)  
  
-   [Determinar sua estratégia de aplicativo federado no parceiro de recurso](Determine-Your-Federated-Application-Strategy-in-the-Resource-Partner.md)  
  

## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
