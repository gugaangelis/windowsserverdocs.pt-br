---
ms.assetid: c87dc32d-ab33-44d2-a46f-f9f878b4e5b4
title: Planejando a implantação do AD FS
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ed2082706975d58a1535aaeb61e6c5283d23306a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359505"
---
# <a name="planning-to-deploy-ad-fs"></a>Planejando a implantação do AD FS


Depois de coletar informações sobre o seu ambiente e decidir sobre um design Serviços de Federação do Active Directory (AD FS) \(AD FS @ no__t-1 seguindo as orientações no guia de [design do AD FS no Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx), você pode começar a planejar a implantação do o design de AD FS da sua organização. Com o design concluído e as informações neste tópico, você pode determinar quais tarefas executar para implantar AD FS em sua organização.  
  
## <a name="reviewing-your-ad-fs-design"></a>Revisando seu design do AD FS  
Se a equipe de design que construiu o design de AD FS original para sua organização for diferente da equipe de implantação que realmente implementará a implantação, certifique-se de que a equipe de implantação revisará o design final com a equipe de design. Revise os seguintes pontos relacionados ao design:  
  
-   A estratégia da equipe de design a fim de determinar a melhor topologia física para o posicionamento dos servidores de federação em sua rede corporativa ou sua rede de perímetro. A equipe de implantação pode consultar a documentação sobre este assunto revisando os seguintes tópicos no guia de design de AD FS:  
  
    -   [A função do banco de dados de configuração do AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)  
  
    -   [Como planejar o posicionamento do servidor de federação](https://technet.microsoft.com/library/dd807069.aspx)  
  
    -   [Como planejar o posicionamento do proxy do servidor de federação](https://technet.microsoft.com/library/dd807130.aspx)  
  
    É possível que a equipe de design deixe a questão do posicionamento do servidor de federação ou do proxy do servidor de federação para a equipe de implantação. A equipe de implantação será, então, responsável por documentar e implementar a topologia física dos servidores.  
  
-   Os motivos dos negócios para a designação da sua organização como provedor de declarações, a terceira parte confiável, ou os dois, no escopo do design documentado do AD FS. Certifique-se de que os membros da equipe de implantação compreendam os motivos pelos quais AD FS está sendo implantado e quais outras empresas ou organizações estão envolvidas na parceria de Federação. Certifique-se de que os membros da equipe de implantação também compreendam as restrições que existem para as outras empresas ou organizações @no__t hardware 0limited, nenhum ambiente de extranet e assim por diante @ no__t-1 que pode limitar o escopo do design de alguma forma. Para obter mais informações sobre organizações parceiras, consulte [Planejando sua implantação](https://technet.microsoft.com/library/dd807083.aspx).  
  
Depois que as equipes de design e as equipes de implantação concordarem com esses problemas, eles poderão continuar com a implantação do design de AD FS. Para mais informações, consulte [Implementando seu plano de design do AD FS](Implementing-Your-AD-FS-Design-Plan.md).  
