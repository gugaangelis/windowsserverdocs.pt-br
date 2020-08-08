---
title: Recuperação de floresta do AD-planejando um plano de recuperação da floresta do AD
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 17381f30-55f2-4e00-977a-b701675fa4ff
ms.openlocfilehash: d6637f92dff1542837b42a1406a17555a753bf86
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87972313"
---
# <a name="ad-forest-recovery---devising-an-ad-forest-recovery-plan"></a>Recuperação de floresta do AD-planejando um plano de recuperação da floresta do AD

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Dependendo do ambiente e dos requisitos de negócios, você pode ou não precisar executar todas as etapas descritas neste guia para executar uma recuperação de floresta bem-sucedida. Considerando que este guia serve apenas como modelo de recuperação de floresta, é vital que você planeje um plano de recuperação de floresta personalizado que se adapte ao seu ambiente e atenda às suas necessidades de negócios.

Por exemplo, no plano de recuperação de sua floresta, você deve ter um mapa de topologia detalhado de suas florestas. O mapa deve listar todas as informações sobre os DCs, como seus nomes, suas funções e o status de backup e as relações de confiança entre eles. Para uma ferramenta que você pode usar para criar um mapa de topologia, consulte [diagramar de topologia de Active Directory da Microsoft](https://www.microsoft.com/download/details.aspx?id=13380).

Você deve praticar seu plano de recuperação de floresta pelo menos uma vez por ano. Além disso, é uma boa ideia executar uma análise de recuperação de floresta quando há alterações de associação ao grupo Administradores de empresa ou administradores de domínio. Isso ajuda a garantir que sua equipe de tecnologia da informação (TI) entenda totalmente o plano de recuperação da floresta.

## <a name="next-steps"></a>Próximas etapas

- [Recuperação de floresta do AD – Pré-requisitos](AD-Forest-Recovery-Prerequisties.md)
- [Recuperação de floresta do AD-planejar um plano de recuperação de floresta personalizado](AD-Forest-Recovery-Devising-a-Plan.md)
- [Recuperação de floresta do AD – identificar o problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Recuperação de floresta do AD-determine como recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Recuperação de floresta do AD-executar recuperação inicial](AD-Forest-Recovery-Perform-initial-recovery.md)
- [Recuperação de floresta do AD – Procedimentos](AD-Forest-Recovery-Procedures.md)
- [Recuperação de floresta do AD-perguntas frequentes](AD-Forest-Recovery-FAQ.md)
- [Recuperação de floresta do AD-recuperando um único domínio em uma floresta de multidomínio](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)
- [Recuperação de floresta do AD-recuperação de floresta com controladores de domínio do Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)
