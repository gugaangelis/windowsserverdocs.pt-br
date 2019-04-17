---
ms.assetid: c87dc32d-ab33-44d2-a46f-f9f878b4e5b4
title: "Planejar a implantação do AD FS"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: ca9e53d7d98f3ae5e6b7b329e52d4979e8c10215
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="planning-to-deploy-ad-fs"></a>Planejar a implantação do AD FS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


Depois de coletar informações sobre seu ambiente e decidir em um design de \(AD FS\) de serviços de Federação do Active Directory, seguindo as orientações a [guia de Design do AD FS no Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx), você pode começar a planejar a implantação do design do AD FS da sua organização. Com o design concluído e as informações neste tópico, você pode determinar quais tarefas a serem executadas para implantar o AD FS em sua organização.  
  
## <a name="reviewing-your-ad-fs-design"></a>Revisando seu design do AD FS  
Se a equipe de design que construído o original do AD FS de design para sua organização é diferente da equipe de implantação que realmente implementar a implantação, certifique-se de que a equipe de implantação revisa o design final com a equipe de design. Revise os seguintes pontos sobre o design:  
  
-   Estratégia da equipe de design para determinar a melhor topologia física para o posicionamento dos servidores de Federação em sua rede corporativa ou perímetro. A equipe de implantação pode consultar a documentação sobre esse assunto, analisando os tópicos a seguir no guia de Design do AD FS:  
  
    -   [A função do banco de dados de configuração do AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md)  
  
    -   [Planejar o posicionamento do servidor de Federação](https://technet.microsoft.com/library/dd807069.aspx)  
  
    -   [Planejar o posicionamento de Proxy do servidor de Federação](https://technet.microsoft.com/library/dd807130.aspx)  
  
    É possível que a equipe de design pode deixar o assunto do servidor de Federação ou posicionamento de proxy do servidor de federação para a equipe de implantação. A equipe de implantação, em seguida, é responsável por documentando e implementando a topologia física dos servidores.  
  
-   As razões comerciais para designação da sua organização como um provedor de declarações, o terceiro ou ambos, dentro do escopo do design do AD FS documentado. Certifique-se de que os membros da equipe de implantação entendam os motivos por que o AD FS está sendo implantado e quais outras empresas ou organizações são envolvidos na parceria federação. Certifique-se de que os membros da equipe de implantação também compreenderam as restrições existentes para outras empresas ou organizações \ (limitado hardware, nenhum ambiente extranet e forth\ afirmativo) que pode limitar o escopo do design de alguma forma. Para saber mais sobre as organizações parceiras, consulte [planejar a implantação](https://technet.microsoft.com/library/dd807083.aspx).  
  
Após o design equipes e equipes de implantação têm esses problemas, ele poderá prosseguir com a implantação do design do AD FS. Para obter mais informações, consulte [implementar seu plano de Design do AD FS](Implementing-Your-AD-FS-Design-Plan.md).  
