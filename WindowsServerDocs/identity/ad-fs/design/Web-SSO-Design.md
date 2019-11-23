---
ms.assetid: eb778f63-f7be-438e-8c5e-1fd9b194b967
title: Design SSO da Web
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d7f52cd36588a1e5de4536a760c38c147dd1e003
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407830"
---
# <a name="web-sso-design"></a>Design SSO da Web

No\-de logon único de\-da Web em \(design de\) SSO no serviços de Federação do Active Directory (AD FS) \(AD FS\), os usuários devem autenticar apenas uma vez para acessar vários aplicativos AD FS\-ou serviços protegidos. Nesse design, todos os usuários são externos e nenhuma relação de confiança de federação existe porque não há organizações de parceiro. Normalmente, você implanta esse design quando deseja fornecer acesso individual de consumidor ou cliente a um ou AD FS mais serviços ou aplicativos protegidos pela Internet, conforme mostrado na ilustração a seguir.  
  
![design de SSO da Web](media/adfs2_WebSSODesign.gif)  
  
Com o design de SSO da Web, uma organização que normalmente hospeda um AD FS\-aplicativo ou serviço protegido em uma rede de perímetro pode manter um armazenamento separado de contas de clientes na rede de perímetro, o que torna mais fácil isolar contas de clientes de contas de funcionários.  
  
Você pode gerenciar as contas locais de clientes na rede de perímetro usando o Active Directory Domain Services \(AD DS\), SQL Server ou um repositório de atributos personalizado.  
  
Esse design coincide com a meta de implantação em [Provide Your Active Directory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md).  
  
Para obter uma lista de tarefas detalhadas que você pode usar para planejar e implantar o design de SSO da Web, consulte [Checklist: Implementing a Web SSO Design](../../ad-fs/deployment/Checklist--Implementing-a-Web-SSO-Design.md).  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
