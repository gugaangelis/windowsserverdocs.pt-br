---
title: "Pré-requisitos para planejamento de recuperação de floresta do Active Directory"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.technology: identity-adfs
ms.openlocfilehash: 672e1f4d0de9bfb2cbe291c5ed715814c8acacd0
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2017
---
#<a name="active-directory-forest-recovery-prerequisites"></a>Pré-requisitos de recuperação de floresta do Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

O seguinte documento discutir pré-requisitos que você deve estar familiarizado com antes de planejar um plano de recuperação de floresta ou tentando uma recuperação.

## <a name="assumptions-for-using-this-guide"></a>Considerações para usar este guia 

1.  Se tiver trabalhado com um profissional Microsoft Support e:

    - Determine a causa da falha toda a floresta. Este guia não sugerir uma causa da falha ou recomendar qualquer procedimentos para evitar a falha.  
    - Avaliada quaisquer possíveis soluções.  
    - Concluíram, com Microsoft Support, consultoria que restaurar toda a floresta para seu estado antes que a falha ocorreu é a melhor maneira de recuperar da falha. Em muitos casos, a recuperação de floresta deve ser a última opção.  </br></br>

2. Se você tiver seguido as recomendações de práticas recomendadas da Microsoft para usar integrada ao Active Directory sistema DNS (Domain Name). Especificamente, deve haver uma zona DNS integrada ao Active Directory para cada domínio do Active Directory. Se isso não for o caso, você ainda pode usar os princípios básicos deste guia para executar a recuperação de floresta. No entanto, você precisará tomar medidas específicas para a recuperação do DNS com base em seu próprio ambiente. Para obter mais informações sobre como usar o DNS integrada ao Active Directory, consulte [criando um Design de infraestrutura de DNS ](../../ad-ds/plan/Creating-a-DNS-Infrastructure-Design.md).
3. Embora este guia destina-se como um guia genérico para a recuperação da floresta, nem todos os cenários possíveis são abordados. Por exemplo, a partir do Windows Server 2008, há uma versão do núcleo do servidor, que é uma versão completa do Windows Server, mas sem uma GUI completa. Embora ele certamente é possível recuperar uma floresta formada por apenas controladores de domínio que executam o núcleo do servidor, este guia não tem nenhuma instruções detalhadas. No entanto, com base nas diretrizes discutidas aqui será capaz de criar as ações de linha de comando necessárias por conta própria.  
 
>![!NOTE]
> Embora os objetivos deste guia são recuperar floresta e manter ou restaurar a funcionalidade completa do DNS, recuperação pode resultar em uma configuração de DNS é alterada de configuração antes da falha. Depois de floresta é recuperada, você pode reverter para a configuração original do DNS. As recomendações neste guia não descrevem como configurar servidores DNS para executar a resolução de outras partes do namespace corporativo onde há zonas DNS que não são armazenadas no AD DS.  

## <a name="concepts-for-using-this-guide"></a>Conceitos para usar este guia
 Antes de começar o planejamento para a recuperação de uma floresta do Active Directory, você deve estar familiarizado com o seguinte:  
  
-   Conceitos básicos do Active Directory  
  
-   A importância das funções mestre de operações (também conhecida como operações mestre únicas flexíveis ou FSMO). Essas funções incluem o seguinte:  
  
    -   Mestre de esquema  
  
    -   Mestre de nomeação de domínio  
  
    -   Mestre relativo ID (RID)  
  
    -   Mestre emulador do domínio primário (PDC) do controlador  
  
    -   Mestre de infraestrutura  
  
 Além disso, você deve ter feito backup e restaurados AD DS e SYSVOL em um ambiente de laboratório regularmente. Para obter mais informações, consulte [backup dos dados de estado do sistema](AD-Forest-Recovery-Procedures.md) e [executar uma restauração sem autorização dos serviços de domínio do Active Directory ](AD-Forest-Recovery-Procedures.md).

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
