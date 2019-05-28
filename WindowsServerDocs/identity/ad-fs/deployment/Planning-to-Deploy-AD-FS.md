---
ms.assetid: c87dc32d-ab33-44d2-a46f-f9f878b4e5b4
title: Planejando a implantação do AD FS
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 1459cade5071374ca39d453b9915a68e4bcfe539
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192044"
---
# <a name="planning-to-deploy-ad-fs"></a>Planejando a implantação do AD FS


Depois de coletar informações sobre seu ambiente e decidir sobre um serviços de Federação do Active Directory \(do AD FS\) design seguindo as diretrizes a [guia de Design do AD FS no Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx), Você pode começar a planejar a implantação do design do AD FS da sua organização. Com o design completo e as informações neste tópico, você pode determinar as tarefas a serem executadas para implantar o AD FS em sua organização.  
  
## <a name="reviewing-your-ad-fs-design"></a>Revisando seu design do AD FS  
Se a equipe de design que construída com o AD FS original de design para sua organização for diferente da equipe de implantação que efetivamente fará a implantação, certifique-se de que a equipe de implantação revisará o design final com a equipe de design. Revise os seguintes pontos relacionados ao design:  
  
-   A estratégia da equipe de design a fim de determinar a melhor topologia física para o posicionamento dos servidores de federação em sua rede corporativa ou sua rede de perímetro. A equipe de implantação pode consultar a documentação sobre esse assunto revisando os seguintes tópicos no guia de Design do AD FS:  
  
    -   [A função do banco de dados de configuração do AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)  
  
    -   [Como planejar o posicionamento do servidor de federação](https://technet.microsoft.com/library/dd807069.aspx)  
  
    -   [Como planejar o posicionamento do proxy do servidor de federação](https://technet.microsoft.com/library/dd807130.aspx)  
  
    É possível que a equipe de design deixe a questão do posicionamento do servidor de federação ou do proxy do servidor de federação para a equipe de implantação. A equipe de implantação será, então, responsável por documentar e implementar a topologia física dos servidores.  
  
-   Os motivos dos negócios para a designação da sua organização como provedor de declarações, a terceira parte confiável, ou os dois, no escopo do design documentado do AD FS. Certifique-se de que os membros da equipe de implantação compreenderam as razões por que o AD FS está sendo implantado e quais são outras empresas ou organizações envolvidas na parceria de Federação. Certifique-se de que os membros da equipe de implantação também compreendem as restrições existentes para outras empresas ou organizações \(limitada de hardware, nenhum ambiente de extranet e assim por diante\) que pode limitar o escopo do design de alguma forma. Para obter mais informações sobre organizações parceiras, consulte [Planejando sua implantação](https://technet.microsoft.com/library/dd807083.aspx).  
  
Após o design de equipes e equipes de implantação concordarem sobre essas questões, elas poderão continuar com a implantação do design do AD FS. Para mais informações, consulte [Implementando seu plano de design do AD FS](Implementing-Your-AD-FS-Design-Plan.md).  
