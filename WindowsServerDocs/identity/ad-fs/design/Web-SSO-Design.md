---
ms.assetid: eb778f63-f7be-438e-8c5e-1fd9b194b967
title: Design SSO da Web
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 1b8344594c9fc477ed8424c716ec8d7f7fd91ef3
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="web-sso-design"></a>Design SSO da Web

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Na Web \(SSO\) Sign\ Single\ no design em serviços de Federação do Active Directory \(AD FS\), os usuários devem autenticar somente uma vez para acessar vários aplicativos protegidos FS\ AD ou serviços. Neste design todos os usuários são externos, e nenhuma confiança de Federação existe porque não há nenhuma organizações parceiras. Normalmente, você implantar esse design quando você deseja fornecer acesso individual de consumidor ou cliente para um ou mais aplicativos ou os Serviços AD FS – protegidas pela Internet, conforme mostrado na ilustração a seguir.  
  
![Design de sso da Web](media/adfs2_WebSSODesign.gif)  
  
Com o SSO da Web de design, uma organização que normalmente hospeda um aplicativo protegido FS\ AD ou serviço em uma rede do perímetro pode manter um repositório separado de contas de cliente na rede de perímetro, o que torna mais fácil isolar as contas de cliente de contas do funcionário.  
  
Você pode gerenciar as contas locais para os clientes na rede de perímetro usando os serviços de domínio do Active Directory \(AD DS\), SQL Server ou um repositório de atributo personalizado.  
  
Esse design coincide com o objetivo de implantação no [fornecer o Active Directory aos usuários acesso a seus aplicativos com reconhecimento de declarações e serviços](Provide-Your-Active-Directory-Users-Access-to-Your-Claims-Aware-Applications-and-Services.md).  
  
Para obter uma lista de tarefas detalhadas que você pode usar para planejar e implantar seu design de SSO da Web, consulte [lista de verificação: Implementando um Design de SSO Web](../../ad-fs/deployment/Checklist--Implementing-a-Web-SSO-Design.md).  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
