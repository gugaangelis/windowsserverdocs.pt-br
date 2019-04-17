---
title: Recuperação de floresta do AD - recuperando um único domínio em uma floresta de vários domínios
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 267541be-2ea7-4af6-ab34-8b5a3fedee2d
ms.technology: identity-adfs
ms.openlocfilehash: 3a1c9d0671a732eee83aa707e061afdbbf106fa5
ms.sourcegitcommit: f26d2668f57624a3865ca4ffd36a698eea7b503e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/12/2018
---
# <a name="ad-forest-recovery---recovering-a-single-domain-in-a-multidomain-forest"></a>Recuperação de floresta do AD - recuperando um único domínio em uma floresta de vários domínios

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Pode haver momentos em que é necessário para recuperar somente um único domínio em uma floresta com vários domínios, em vez de uma recuperação total floresta. Este tópico aborda considerações para recuperar um único domínio e estratégias possíveis para a recuperação.  
  
 Recuperação de um único domínio apresenta um desafio exclusivo para recriar os servidores de catálogo global (GC). Por exemplo, se o primeiro controlador de domínio (DC) para o domínio for restaurado de um backup que foi criado anteriormente uma semana, em seguida, todos os outros GC na floresta terá mais dados atualizados para esse domínio que o controlador de domínio restaurado. Para restabelecer consistência de dados GC, há algumas opções:  
  
-   Unhost e, em seguida, rehost a partição recuperados de domínios de todos os GCs da floresta, exceto aqueles no domínio recuperado, ao mesmo tempo.  
  
-   Siga o processo de recuperação de floresta para recuperar o domínio e, em seguida, remova objetos remanescentes GC em outros domínios.  
  
 As seguintes seções fornecem considerações gerais para cada opção. O conjunto completo de etapas que precisa ser feito para a recuperação irá variar para diferentes ambientes do Active Directory.  
  
## <a name="rehost-all-gcs"></a>Rehost GC todos  

> [!WARNING]
>  A senha da conta do administrador do domínio para todos os domínios deve estar pronta para uso no caso de um problema impede o acesso a um GC para logon.  

 Rehosting GC todos pode ser feito usando repadmin//unhost e repadmin /rehost comandos (parte do repadmin /experthelp). Você pode executar os comandos repadmin em cada GC em cada domínio que não forem recuperado. Ele precisa ser garantida, que todos os GC não contêm uma cópia do domínio recuperado mais. Para conseguir isso, unhost a partição do domínio primeiro de todos os controladores de domínio em todos os domínios recuperadas none da floresta pela primeira vez. Depois que todos os GC não contêm a partição mais, você pode rehost-lo. Quando rehosting, considere a estrutura site e replicação da floresta, por exemplo, concluir a rehost de um controlador de domínio por site antes rehosting as outros controladores de domínio do site.  
  
 Essa opção pode ser vantajosa para uma empresa de pequeno que tem apenas alguns controladores de domínio para cada domínio. Todos os GC poderiam ser reconstruídos em uma noite de sexta-feira e, se necessário, conclua a replicação de domínio somente leitura todas as partições antes da manhã de segunda-feira. Mas se você precisar recuperar um grande domínio que abrange sites em todo o mundo, rehosting a partição de domínio somente leitura em todos os GC de outros domínios podem significativamente operações de impacto e potencialmente exigem tempo de inatividade.  
  
## <a name="remove-lingering-objects"></a>Remover objetos remanescentes  
 Assim como o processo de recuperação de floresta, você restaurar um controlador de domínio de backup no domínio que você precisa recuperar, executar a limpeza de metadados de controladores de domínio restantes e reinstale o AD DS para criar o domínio. No GC de todos os outros domínios na floresta, você deve remover os objetos persistentes para a partição de somente leitura do domínio recuperado.  
  
 A origem para a limpeza do objeto remanescente deve ser um controlador de domínio no domínio recuperado. Para ter certeza de que o controlador de domínio de origem não tem qualquer objeto remanescente para todas as partições de domínio, você pode remover o catálogo global se ela tiver sido GC.  
  
 Remover objetos remanescentes é vantajoso para grandes organizações que não correr o risco do tempo de inatividade associado com as outras opções.  
  
 Para obter mais informações, consulte [Repadmin de uso para remover objetos remanescentes](https://technet.microsoft.com/library/cc785298.aspx).

## <a name="next-steps"></a>Próximas etapas
-   [Recuperação de floresta do AD - pré-requisitos](AD-Forest-Recovery-Prerequisties.md)  
-   [Recuperação de floresta do AD - descobrindo um plano de recuperação personalizada floresta](AD-Forest-Recovery-Devising-a-Plan.md)  
-   [Recuperação de floresta do AD - identificar o problema](AD-Forest-Recovery-Identify-the-Problem.md)
-   [Recuperação de floresta do AD - determinar como recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [Recuperação de floresta do AD - realizar uma recuperação inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)  
-   [Recuperação de floresta do AD - perguntas frequentes](AD-Forest-Recovery-FAQ.md)  
-   [Recuperação de floresta do AD - recuperando um único domínio em uma floresta Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [AD floresta recuperação - floresta com controladores de domínio do Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)  
