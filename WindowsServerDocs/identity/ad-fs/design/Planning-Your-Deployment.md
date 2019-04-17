---
ms.assetid: bb9b9e18-bf2f-4115-be77-9a165944db41
title: "Planejar sua implantação"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5c7cec9ad92605f3dc98f8ce8fb7853a7ae61299
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="planning-your-deployment"></a>Planejar sua implantação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Quando você planeja a colaboração \(federation\-based\) cross\ organizacionais usando os serviços de Federação do Active Directory \(AD FS\), determine primeiro se sua organização irá hospedar um recurso da Web para serem acessados por outras empresas na Internet ou se você fornecerá acesso ao recurso da Web para os funcionários em sua organização. Essa determinação afeta como implantar o AD FS, e é fundamental no planejamento de sua infraestrutura do AD FS.  
  
> [!NOTE]  
> Certifique-se de que a função organização reproduz no contrato de Federação é claramente compreendida por todas as partes.  
  
Para o [federados Design de SSO da Web](Federated-Web-SSO-Design.md), AD FS usa termos como *parceiro de conta* \ (também conhecido como *provedor de identidade* em snap\-in\ o gerenciamento do AD FS) e *parceiro de recurso* \ (também conhecido como *terceiro* em snap\-in\ o gerenciamento do AD FS) para ajudar a diferenciar a organização que hospeda as contas \(the account partner\) da organização que hospeda os recursos baseados em Web\ \(the resource partner\).  
  
No [Web SSO Design](Web-SSO-Design.md), a organização atua em ambas as as conta parceiro e recurso parceiro funções porque fornece seus usuários com acesso a seus aplicativos.  
  
Os tópicos seguintes explicam que alguns do AD FS parceiro conceitos da organização. Eles também contêm links para tópicos no guia de implantação do AD FS que contêm informações sobre como configurar e definir as organizações de parceiros de conta e organizações de parceiro de recurso com base em suas metas de implantação do AD FS.  
  
## <a name="in-this-section"></a>Nesta seção  
  
-   [Práticas recomendadas para segura de planejamento e implantação do AD FS](Best-Practices-for-Secure-Planning-and-Deployment-of-AD-FS.md)  
  
-   [Planejamento de interoperabilidade com o AD FS 1. x](Planning-for-Interoperability-with-AD-FS-1.x.md)  
  
-   [Quando usar a delegação de identidade](When-to-Use-Identity-Delegation.md)  
  
-   [Implantando o AD FS na organização do parceiro de conta](Deploying-AD-FS-in-the-Account-Partner-Organization-2012.md)  
  
-   [Implantando o AD FS na organização do parceiro de recurso](Deploying-AD-FS-in-the-Resource-Partner-Organization-2012.md)  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)


