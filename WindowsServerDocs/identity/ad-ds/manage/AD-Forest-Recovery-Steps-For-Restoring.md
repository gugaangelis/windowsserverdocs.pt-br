---
title: "Recuperação de floresta do AD - etapas para restaurar a floresta"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adfs
ms.openlocfilehash: 29988c262897649173e039cc052bb64f38a1a0ca
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2017
---
#<a name="ad-forest-recovery---steps-for-restoring-the-forest"></a>Recuperação de floresta do AD - etapas para restaurar a floresta 

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Esta seção fornece uma visão geral do caminho recomendado para recuperar uma floresta. As etapas de recuperação de floresta são descritas em detalhes mais tarde.  
  
 A lista a seguir resume as etapas de recuperação em um alto nível:  
  
1.  [Identificar o problema](AD-Forest-Recovery-Identify-the-Problem.md)  
  
     Trabalhar com IT e Microsoft Support para determinar o escopo do problema e possíveis causas e avaliar as possíveis soluções com todas as partes interessadas no negócio. Em muitos casos recuperação total floresta deve ser a última opção.  
  
2.  [Decida como recuperar da floresta](AD-Forest-Recovery-Determine-how-to-Recover.md)  
  
     Depois de determinar que a recuperação de floresta é necessária, as etapas para preparar para ele preliminar completo: determinar a estrutura da floresta atual, identificar as funções que cada DC executa, decida quais DC para restaurar para cada domínio e garantir que todos os controladores de domínio graváveis são colocados offline.  
  
3.  [Executar a recuperação inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
  
     Forma isolada, recuperar um controlador de domínio para cada domínio, limpá-los e reconecte os domínios. Redefinir contas privilegiadas e corrigir problemas causados por violações de segurança nessa fase.  
  
4.  [Reimplantar restante controladores de domínio](AD-Forest-Recovery-Restore-Additional-DCs.md)  
  
     Reimplantá da floresta para retorná-lo ao seu estado antes da falha. Esta etapa precisarão ser adaptado para suas necessidades e específicos de design. Clonagem de controlador de domínio virtualizado pode ajudar a acelerar esse processo.  
  
5.  [Limpeza](AD-Forest-Recovery-Cleanup.md)  
  
     Depois que a funcionalidade tiver sido restaurada, reconfigurar a resolução de nomes conforme necessário e obter aplicativos LOB trabalhando.  

  
 As etapas neste guia foram projetadas para minimizar a possibilidade de reintrodução dados perigosos na floresta recuperada. Talvez você precise modificar estas etapas para levar em conta fatores como:  
  
-   Escalabilidade  
-   Gerenciamento remoto  
-   Velocidade da recuperação  

