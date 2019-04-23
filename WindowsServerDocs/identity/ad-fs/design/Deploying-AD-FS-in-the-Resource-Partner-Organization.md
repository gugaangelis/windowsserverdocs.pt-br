---
ms.assetid: 41d6b897-1e72-4522-aad6-eece1154a154
title: Implantando o AD FS na organização do parceiro de recurso
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 4a556c07e7d6e0bec4c947ea9d1a75eef9964cef
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877597"
---
# <a name="deploying-ad-fs-in-the-resource-partner-organization"></a>Implantando o AD FS na organização do parceiro de recurso

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

A organização do parceiro de recurso nos serviços de Federação do Active Directory \(do AD FS\) representa a organização cujos servidores Web podem ser protegidos por um recurso\-servidor de federação de lado. O servidor de federação no parceiro de recurso usa os tokens de segurança que são produzidos pelo parceiro de conta fornecer declarações aos servidores Web que estão localizados no parceiro de recurso.  
  
Em cenários em que você precisa fornecer acesso a serviços federados ou aplicativos para vários usuários diferentes — quando alguns usuários residem em diferentes organizações — você pode configurar o servidor de federação de recurso para que você possa implantar vários parceiros de conta.  
  
Para obter mais informações sobre como instalar e configurar uma organização do parceiro de recurso, consulte [lista de verificação: Configurando a organização do parceiro de recurso](../../ad-fs/deployment/Checklist--Configuring-the-Resource-Partner-Organization.md).  
  
## <a name="in-this-section"></a>Nesta seção  
  
-   [Examine a função do servidor de federação no parceiro de recurso](Review-the-Role-of-the-Federation-Server-in-the-Resource-Partner.md)  
  
-   [Review the Role of the Federation Server Proxy in the Resource Partner](Review-the-Role-of-the-Federation-Server-Proxy-in-the-Resource-Partner.md)  
  
-   [Determinar sua estratégia de aplicativo federado no parceiro de recurso](Determine-Your-Federated-Application-Strategy-in-the-Resource-Partner.md)  
  

## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
