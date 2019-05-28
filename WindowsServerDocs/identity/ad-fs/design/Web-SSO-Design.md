---
ms.assetid: eb778f63-f7be-438e-8c5e-1fd9b194b967
title: Design SSO da Web
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 29d50a4d1855e609b6ac9ee627256201074a5033
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190716"
---
# <a name="web-sso-design"></a>Design SSO da Web

No único da Web\-sinal\-nos \(SSO\) design nos serviços de Federação do Active Directory \(AD FS\), os usuários devem autenticar somente uma vez para acessar vários AD FS\- protegido de aplicativos ou serviços. Nesse design, todos os usuários são externos e nenhuma relação de confiança de federação existe porque não há organizações de parceiro. Normalmente, você implanta esse design, quando você deseja fornecer acesso de cliente ou consumidor individual a um ou mais aplicativos ou serviços do AD FS – protegido pela Internet, conforme mostrado na ilustração a seguir.  
  
![design sso da Web](media/adfs2_WebSSODesign.gif)  
  
Com o design de SSO da Web, uma organização que normalmente hospeda um AD FS\-aplicativo protegido ou serviço em uma rede de perímetro pode manter um armazenamento separado de contas de cliente na rede de perímetro, o que torna mais fácil isolar o cliente contas das contas de funcionários.  
  
Você pode gerenciar as contas locais para clientes na rede de perímetro usando qualquer um dos serviços de domínio do Active Directory \(AD DS\), SQL Server ou um repositório de atributos personalizados.  
  
Esse design coincide com a meta de implantação em [Provide Your Active Directory Users Access to Your Claims-Aware Applications and Services](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md).  
  
Para obter uma lista de tarefas detalhadas que você pode usar para planejar e implantar seu design de SSO da Web, consulte [lista de verificação: Implementando um Design SSO da Web](../../ad-fs/deployment/Checklist--Implementing-a-Web-SSO-Design.md).  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
