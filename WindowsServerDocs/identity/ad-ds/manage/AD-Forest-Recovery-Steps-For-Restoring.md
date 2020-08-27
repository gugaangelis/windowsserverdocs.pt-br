---
title: Recuperação de floresta do AD – etapas para restaurar a floresta
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.openlocfilehash: b6dbd22eb34578013f90dda937ec1c26fe8fbf4e
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88938856"
---
# <a name="ad-forest-recovery---steps-for-restoring-the-forest"></a>Recuperação de floresta do AD – etapas para restaurar a floresta

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Esta seção fornece uma visão geral do caminho recomendado para recuperar uma floresta. As etapas de recuperação de floresta são descritas em detalhes posteriormente.

A lista a seguir resume as etapas de recuperação em um alto nível:

1. [Identificar o problema](AD-Forest-Recovery-Identify-the-Problem.md)

   Trabalhe com ele e Suporte da Microsoft para determinar o escopo do problema e as possíveis causas, além de avaliar possíveis soluções com todos os participantes comerciais. Em muitos casos, a recuperação de floresta total deve ser a última opção.

2. [Decidir como recuperar a floresta](AD-Forest-Recovery-Determine-how-to-Recover.md)

   Depois de determinar que a recuperação de floresta é necessária, conclua as etapas preliminares para se preparar para ela: Determine a estrutura atual da floresta, identifique as funções que cada DC executa, decida qual DC será restaurado para cada domínio e verifique se todos os DCs graváveis são colocados offline.

3. [Executar recuperação inicial](AD-Forest-Recovery-Perform-initial-recovery.md)

   Em isolamento, recupere um DC para cada domínio, limpe-os e reconecte os domínios. Redefina contas com privilégios e corrija os problemas causados por violações de segurança nesta fase.

4. [Reimplantar os DCs restantes](AD-Forest-Recovery-Restore-Additional-DCs.md)

   Reimplante a floresta para retorná-la ao estado anterior à falha. Esta etapa precisará ser adaptada ao design e aos requisitos específicos. A clonagem do controlador de domínio virtualizado pode ajudar a agilizar esse processo.

5. [Limpeza](AD-Forest-Recovery-Cleanup.md)

   Após a restauração da funcionalidade, reconfigure a resolução de nomes conforme necessário e obtenha os aplicativos LOB funcionando.

As etapas neste guia foram projetadas para minimizar a possibilidade de reintroduzir dados perigosos na floresta recuperada. Talvez seja necessário modificar essas etapas para considerar tais fatores como:

- Escalabilidade
- Capacidade de gerenciamento remoto
- Velocidade de recuperação
