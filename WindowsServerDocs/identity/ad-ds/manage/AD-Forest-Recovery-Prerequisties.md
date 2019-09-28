---
title: Pré-requisitos para planejar Active Directory recuperação de floresta
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.technology: identity-adds
ms.openlocfilehash: 12c34afc497131bfe6abdc78c636e6d3b784877e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390373"
---
# <a name="active-directory-forest-recovery-prerequisites"></a>Pré-requisitos de recuperação de floresta Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

O documento a seguir discute os pré-requisitos com os quais você deve estar familiarizado antes de planejar um plano de recuperação de floresta ou tentar uma recuperação.

## <a name="assumptions-for-using-this-guide"></a>Suposições sobre o uso deste guia

1. Você trabalhou com um Suporte da Microsoft Professional e:
   - Determine a causa da falha em toda a floresta. Este guia não sugere uma causa da falha ou recomenda quaisquer procedimentos para evitar a falha.
   - Avaliadas todas as possíveis soluções.  
   - Concluído, em consultoria com Suporte da Microsoft, que restaurar a floresta inteira para seu estado antes da falha ocorreu é a melhor maneira de se recuperar da falha. Em muitos casos, a recuperação de floresta deve ser a última opção.

2. Você seguiu as recomendações de práticas recomendadas da Microsoft para usar o Active Directory – sistema de nomes de domínio integrado (DNS). Especificamente, deve haver uma zona DNS integrada Active Directory para cada domínio de Active Directory. 
   - Se esse não for o caso, você ainda poderá usar os princípios básicos deste guia para executar a recuperação de floresta. No entanto, será necessário tomar medidas específicas para a recuperação de DNS com base em seu próprio ambiente. Para obter mais informações sobre como usar o DNS integrado Active Directory, consulte [criando um design de infraestrutura de DNS](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md).

3. Embora este guia seja destinado a um guia genérico para a recuperação de florestas, nem todos os cenários possíveis são cobertos. Por exemplo, a partir do Windows Server 2008, há uma versão do Server Core, que é uma versão completa do Windows Server, mas sem uma GUI completa. Embora seja certamente possível recuperar uma floresta que consiste em apenas DCs que executam o Server Core, este guia não tem instruções detalhadas. No entanto, com base nas diretrizes discutidas aqui você poderá criar as ações de linha de comando necessárias por conta própria.  

> ![!NOTE]
> Embora os objetivos deste guia sejam recuperar a floresta e manter ou restaurar a funcionalidade completa do DNS, a recuperação pode resultar em uma configuração de DNS que é alterada da configuração antes da falha. Depois que a floresta for recuperada, você poderá reverter para a configuração de DNS original. As recomendações neste guia não descrevem como configurar servidores DNS para executar a resolução de nomes de outras partes do namespace corporativo em que há zonas DNS que não estão armazenadas no AD DS.  

## <a name="concepts-for-using-this-guide"></a>Conceitos para usar este guia

Antes de começar a planejar a recuperação de uma floresta Active Directory, você deve estar familiarizado com o seguinte:  
  
- Conceitos fundamentais de Active Directory  
- A importância das funções de mestre de operações (também conhecidas como operações de mestre único flexíveis ou FSMO). Essas funções incluem o seguinte:  
   - Mestre de esquema
   - Mestre de nomeação de domínio
   - Mestre de ID relativa (RID)
   - Mestre de emulador de controlador de domínio primário (PDC)
   - Mestre de infraestrutura

Além disso, você deve ter feito backup e restaurado AD DS e SYSVOL em um ambiente de laboratório regularmente. Para obter mais informações, consulte [fazendo backup dos dados de estado do sistema](AD-Forest-Recovery-Procedures.md) e [executando uma restauração não autoritativa de Active Directory Domain Services](AD-Forest-Recovery-Procedures.md).

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
