---
title: Recuperação de floresta do AD - as etapas para restaurar a floresta
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: 1712d3a636160f82495539afdd42ab2ee85ffae2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861467"
---
# <a name="ad-forest-recovery---steps-for-restoring-the-forest"></a>Recuperação de floresta do AD - as etapas para restaurar a floresta

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Esta seção fornece uma visão geral do que o caminho recomendado para a recuperação de uma floresta. As etapas de recuperação de floresta são descritas em detalhes mais tarde.  
  
A lista a seguir resume as etapas de recuperação em um alto nível:  
  
1. [Identificar o problema](AD-Forest-Recovery-Identify-the-Problem.md)  

   Trabalhar com IT e Microsoft Support para determinar o escopo do problema e as possíveis causas e avaliar as possíveis soluções com todas as partes interessadas no negócio. Em muitos casos, recuperação de floresta total deve ser a última opção.  
  
2. [Decidir como se recuperar da floresta](AD-Forest-Recovery-Determine-how-to-Recover.md)  

   Depois de determinar que a recuperação de floresta é necessária, preliminar completa as etapas para se preparar para ele: determinar a estrutura atual da floresta, identificar as funções que cada DC executa, decidir qual controlador de domínio para restaurar para cada domínio e certifique-se de que todos os controladores de domínio graváveis estejam offline.  

3. [Executar recuperação inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  

   Isoladamente, recuperar um controlador de domínio para cada domínio, limpá-los e reconectar-se os domínios. Redefinir contas privilegiadas e corrija problemas causados por violações de segurança nesta fase.  
  
4. [Reimplantar o restante de controladores de domínio](AD-Forest-Recovery-Restore-Additional-DCs.md)  

   Reimplante a floresta para retorná-lo ao seu estado antes da falha. Essa etapa precisará ser adaptada aos seus requisitos e design específico. Clonagem do controlador de domínio virtualizado pode ajudar a acelerar esse processo.  

5. [Limpeza](AD-Forest-Recovery-Cleanup.md)  

   Depois de funcionalidade tiver sido restaurada, reconfigure a resolução de nome conforme a necessidade e obter aplicativos de LOB trabalhando.  

As etapas neste guia são projetadas para minimizar a possibilidade de reintroduzir dados perigosos na floresta recuperada. Talvez você precise modificar essas etapas para considerar fatores como:  
  
- Escalabilidade  
- Capacidade de gerenciamento remota  
- Velocidade de recuperação  
