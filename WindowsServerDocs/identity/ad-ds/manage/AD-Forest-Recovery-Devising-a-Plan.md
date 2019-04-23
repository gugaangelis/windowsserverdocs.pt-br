---
title: Plano de recuperação de floresta de recuperação de floresta do AD - elaborar um anúncio
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 17381f30-55f2-4e00-977a-b701675fa4ff
ms.technology: identity-adds
ms.openlocfilehash: edfc874fe030c6394bc8bda95c49e61951e78f43
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881777"
---
# <a name="ad-forest-recovery---devising-an-ad-forest-recovery-plan"></a>Plano de recuperação de floresta de recuperação de floresta do AD - elaborar um anúncio

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Dependendo do ambiente e requisitos de negócios, você pode ou talvez não seja necessário executar todas as etapas descritas neste guia para executar uma recuperação de floresta com êxito. Considerando que este guia serve apenas como um modelo para a recuperação de floresta, é vital que você criar um plano de recuperação de floresta personalizada que atende ao seu ambiente e atenda às suas necessidades de negócios.  
  
Por exemplo, no seu plano de recuperação de floresta, você deve ter um mapa de topologia detalhada de suas florestas. O mapa deve listar todas as informações sobre os controladores de domínio, como seus nomes, suas funções e o status de backup e as relações de confiança entre eles. Para uma ferramenta que você pode usar para criar um mapa de topologia, consulte [Diagrammer de topologia do Microsoft Active Directory](https://www.microsoft.com/download/details.aspx?id=13380).  
  
Você deve treinar seu plano de recuperação de floresta pelo menos uma vez por ano. Além disso, é uma boa ideia para executar uma simulação de recuperação de floresta quando há alterações de associação ao grupo Administradores corporativos ou Admins. do domínio. Isso ajuda a garantir que sua equipe de TI (tecnologia) informações compreende plenamente o plano de recuperação de floresta.

## <a name="next-steps"></a>Próximas etapas

- [Recuperação de floresta do AD - pré-requisitos](AD-Forest-Recovery-Prerequisties.md)  
- [Recuperação de floresta do AD - elaborar um plano de recuperação de floresta personalizado](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Recuperação de floresta do AD - identificar o problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Recuperação de floresta do AD - determinar como recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Recuperação de floresta do AD - executar a recuperação inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)  
- [Recuperação de floresta do AD - perguntas frequentes](AD-Forest-Recovery-FAQ.md)  
- [Recuperação de floresta do AD - recuperação de um único domínio dentro de uma floresta Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Recuperação de floresta do AD - recuperação de floresta com controladores de domínio do Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)
