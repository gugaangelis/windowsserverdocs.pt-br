---
title: AD floresta recuperação - identificar o problema
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.openlocfilehash: fc285fb355b6539aac56c7c410de1e1f7da48f00
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88939656"
---
# <a name="identify-the-problem"></a>Identificar o problema

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Quando os sintomas de uma falha em toda a floresta aparecem, como nos logs de eventos ou outras soluções de monitoramento, trabalhe com Suporte da Microsoft para determinar a causa da falha e avalie todas as possíveis soluções.

## <a name="examples-of-forest-wide-failures"></a>Exemplos de falhas em toda a floresta

- Todos os DCs foram logicamente corrompidos ou fisicamente danificados em um ponto em que a continuidade dos negócios é impossível; por exemplo, todos os aplicativos de negócios que dependem de AD DS não são funcionais.
- Um administrador não autorizado comprometeu o ambiente de Active Directory.
- Um invasor intencionalmente — ou um administrador acidentalmente — executa um script que espalha dados corrompidos na floresta.
- Um invasor intencionalmente — ou um administrador acidentalmente — estende o esquema de Active Directory com alterações mal-intencionadas ou conflitantes.
- Um invasor conseguiu instalar software mal-intencionado em DCs, e você foi avisado pelo Suporte da Microsoft para recuperar a floresta do backup.

   > [!IMPORTANT]
   >  Este documento não aborda as recomendações de segurança sobre como recuperar uma floresta que foi invadida ou comprometida. Em geral, é recomendável seguir as técnicas de mitigação de passagem de hash para proteger o ambiente. Para obter mais informações, consulte [mitigar ataques de Pass-the-hash (PtH) e outras técnicas de roubo de credenciais](https://www.microsoft.com/download/details.aspx?id=36036).

- Nenhum dos DCs pode replicar com seus parceiros de replicação.
- Não é possível fazer alterações para AD DS em qualquer controlador de domínio.
- Novos DCs não podem ser instalados em nenhum domínio.

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
