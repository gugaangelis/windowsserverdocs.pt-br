---
title: Pré-requisitos para o planejamento para recuperação de floresta do Active Directory
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.technology: identity-adds
ms.openlocfilehash: c8945dd5ccccb27826dd96413b56a070a7452789
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842167"
---
# <a name="active-directory-forest-recovery-prerequisites"></a>Pré-requisitos de recuperação de floresta do Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

O documento a seguir discutem os pré-requisitos que você deve estar familiarizado antes de elaborar um plano de recuperação de floresta ou a tentativa de uma recuperação.

## <a name="assumptions-for-using-this-guide"></a>Considerações para usar este guia

1. Você trabalhou com um profissional da Microsoft Support e:
   - Determine a causa da falha de toda a floresta. Este guia não sugerir uma causa da falha ou recomendar todos os procedimentos descritos para evitar a falha.
   - Avaliado qualquer as possíveis soluções.  
   - Concluído, em conjunto com o Microsoft Support, que a restauração de toda a floresta para seu estado antes que a falha ocorreu é a melhor maneira de se recuperar da falha. Em muitos casos, a recuperação de floresta deve ser a última opção.

2. Se você tiver seguido as recomendações de práticas recomendadas da Microsoft para usar o sistema de nome de domínio (DNS) integrada ao Active Directory. Especificamente, deve haver uma zona DNS integrada ao Active Directory para cada domínio do Active Directory. 
   - Se isso não for o caso, você ainda pode usar os princípios básicos deste guia para executar a recuperação de floresta. No entanto, você precisará tomar medidas específicas para a recuperação DNS com base em seu próprio ambiente. Para obter mais informações sobre como usar o DNS integrado ao Active Directory, consulte [criar um Design de infraestrutura de DNS](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md).

3. Embora este guia destina-se como um guia genérico para recuperação de floresta, nem todos os possíveis cenários são abordados. Por exemplo, a partir do Windows Server 2008, há uma versão do Server Core, que é uma versão completa do Windows Server, mas sem uma GUI completa. Embora seja certamente possível recuperar uma floresta consiste apenas controladores de domínio que executam o Server Core, este guia tem não há instruções detalhadas. No entanto, com base nas diretrizes discutidas aqui, você poderá criar as ações de linha de comando necessárias por conta própria.  

> ![!NOTE]
> Embora sejam os objetivos deste guia recuperar a floresta e manter ou restaurar a funcionalidade completa do DNS, a recuperação pode resultar em uma configuração de DNS é alterada da configuração antes da falha. Depois que a floresta é recuperada, você poderá reverter para a configuração de DNS original. As recomendações neste guia descreve como configurar servidores DNS para executar a resolução de nome de outras partes do namespace corporativo onde há zonas DNS que não são armazenadas no AD DS.  

## <a name="concepts-for-using-this-guide"></a>Conceitos para usar este guia

Antes de começar o planejamento para recuperação de uma floresta do Active Directory, você deve estar familiarizado com o seguinte:  
  
- Conceitos fundamentais do Active Directory  
- A importância de funções de mestre de operações (também conhecido como operações de mestre únicas flexíveis ou FSMO). Essas funções incluem o seguinte:  
   - Mestre de esquema
   - Mestre de nomeação de domínio
   - Mestre ID relativo (RID)
   - Mestre do emulador PDC (controlador) de domínio primário
   - Mestre de infraestrutura

Além disso, você deve ter backup e restaurados AD DS e SYSVOL em um ambiente de laboratório com regularidade. Para obter mais informações, consulte [backup de dados de estado do sistema](AD-Forest-Recovery-Procedures.md) e [executar uma restauração não autoritativa de serviços de domínio do Active Directory](AD-Forest-Recovery-Procedures.md).

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
