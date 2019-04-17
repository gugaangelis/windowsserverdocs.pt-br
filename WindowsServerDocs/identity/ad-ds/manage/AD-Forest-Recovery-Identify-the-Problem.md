---
title: "AD floresta recuperação - identificar o problema"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: 1e9ede12dade0b5f1149eae784620520a8866e30
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="identify-the-problem"></a>Identificar o problema

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2
  
 Quando aparecerem sintomas de uma falha de toda a floresta, como em logs de eventos ou outras soluções de monitoramento, trabalhar com Support da Microsoft para determinar a causa da falha e avaliar todas as possíveis soluções.  
 
## <a name="examples-of-forest-wide-failures"></a>Exemplos de falhas de toda a floresta 
  
-   Todos os controladores de domínio foram corrompidos logicamente ou fisicamente danificados para um ponto que continuidade de negócios é impossível; Por exemplo, todos os aplicativos de negócios que dependem do AD DS estão funcionais.  
  
-   Um administrador desonesto tenha comprometido o ambiente do Active Directory.  
  
-   Um invasor intencionalmente — ou um administrador acidentalmente — executa um script que se espalha corrupção de dados em toda a floresta.  
  
-   Um invasor intencionalmente — ou um administrador acidentalmente — estende o esquema do Active Directory com alterações mal-intencionados ou conflitantes.  
  
-   Um invasor conseguiu instalar um software mal-intencionado em controladores de domínio e você tiver sido informado pelo Microsoft Support para recuperar a floresta de backup.  
  
    > [!IMPORTANT]
    >  Este documento não abrange recomendações de segurança sobre como recuperar uma floresta que foi invadida ou comprometida. Em geral, é recomendável para técnicas de mitigação siga Pass-the-Hash para proteger o ambiente. Para obter mais informações, consulte [ataques Mitigating Pass-the-Hash (PtH) e outras técnicas de roubo de credenciais](https://www.microsoft.com/download/details.aspx?id=36036).  
  
-   Nenhum dos controladores de domínio pode replicar com seus parceiros de replicação.  
  
-   As alterações não podem ser feitas no AD DS em qualquer controlador de domínio.  
  
-   Novos controladores de domínio não podem ser instalados em qualquer domínio.  
  
## <a name="next-steps"></a>Próximas etapas
-   [Recuperação de floresta do AD - pré-requisitos](AD-Forest-Recovery-Prerequisties.md)  
-   [Recuperação de floresta do AD - descobrindo um plano de recuperação personalizada floresta](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Recuperação de floresta do AD - identificar o problema](AD-Forest-Recovery-Identify-the-Problem.md)
-   [Recuperação de floresta do AD - determinar como recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [Recuperação de floresta do AD - realizar uma recuperação inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)  
-   [Recuperação de floresta do AD - perguntas frequentes](AD-Forest-Recovery-FAQ.md)  
-   [Recuperação de floresta do AD - recuperando um único domínio em uma floresta Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [AD floresta recuperação - floresta com controladores de domínio do Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md) 
