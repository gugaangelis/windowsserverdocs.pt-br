---
title: Recuperação de floresta do AD-recuperando um único domínio em uma floresta de multidomínio
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: 267541be-2ea7-4af6-ab34-8b5a3fedee2d
ms.technology: identity-adds
ms.openlocfilehash: 2582ffacb169b59692c0510469131b58be09d5f3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71368926"
---
# <a name="ad-forest-recovery---recovering-a-single-domain-in-a-multidomain-forest"></a>Recuperação de floresta do AD-recuperando um único domínio em uma floresta de multidomínio

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Pode haver ocasiões em que é necessário recuperar apenas um único domínio dentro de uma floresta que tenha vários domínios, em vez de uma recuperação de floresta completa. Este tópico aborda as considerações para recuperar um único domínio e as possíveis estratégias de recuperação.  
  
Uma única recuperação de domínio apresenta um desafio exclusivo para a recriação de servidores de catálogo global (GC). Por exemplo, se o primeiro controlador de domínio (DC) para o domínio for restaurado de um backup criado uma semana antes, todos os outros GCs na floresta terão dados mais atualizados para esse domínio do que o DC restaurado. Para restabelecer a consistência de dados de GC, há duas opções:  
  
- Deshospede e, em seguida, hospede novamente a partição de domínios recuperados de todos os GCs na floresta, exceto aqueles no domínio recuperado, ao mesmo tempo.  
- Siga o processo de recuperação de floresta para recuperar o domínio e, em seguida, remova os objetos remanescentes de GCs em outros domínios.  
  
As seções a seguir fornecem considerações gerais para cada opção. O conjunto completo de etapas que precisam ser feitas para a recuperação irá variar para diferentes ambientes de Active Directory.  
  
## <a name="rehost-all-gcs"></a>Rehospedar todos os GCs  

> [!WARNING]
> A senha da conta de administrador de domínio para todos os domínios deve estar pronta para uso caso um problema impeça o acesso a um GC para logon.  

A rehospedagem de todos os GCs pode ser feita usando os comandos repadmin/Unhost e repadmin/Rehost (parte do repadmin/experthelp). Você executaria os comandos repadmin em cada GC em cada domínio que não é recuperado. Ele precisa ser garantido, que todos os GCs não mantêm mais uma cópia do domínio recuperado. Para fazer isso, deshospede a partição de domínio primeiro de todos os controladores de domínio em todos os domínios recuperados em nenhum da floresta primeiro. Depois que todos os GCs não contiverem mais a partição, você poderá hospedá-lo novamente. Ao hospedar novamente, considere a estrutura de replicação e site de sua floresta, por exemplo, conclua o novo host de um DC por site antes de hospedar novamente os outros controladores de domínio desse site.  
  
Essa opção pode ser vantajosa para uma organização pequena que tenha apenas alguns controladores de domínio para cada domínio. Todos os GCs poderiam ser recriados em uma noite de sexta-feira e, se necessário, a replicação completa para todas as partições de domínio somente leitura antes da segunda-feira de manhã. Mas se você precisar recuperar um domínio grande que cubra sites em todo o mundo, hospedar novamente a partição de domínio somente leitura em todos os GCs para outros domínios pode afetar significativamente as operações e possivelmente exigir tempo de inatividade.  
  
## <a name="remove-lingering-objects"></a>Remover objetos remanescentes

Semelhante ao processo de recuperação de floresta, você restaura um DC do backup no domínio que precisa ser recuperado, executa a limpeza de metadados dos DCs restantes e, em seguida, reinstala o AD DS para criar o domínio. Nos GCs de todos os outros domínios na floresta, você remove os objetos remanescentes para a partição somente leitura do domínio recuperado.  

A origem para a limpeza do objeto remanescente deve ser um DC no domínio recuperado. Para ter certeza de que o DC de origem não tem nenhum objeto remanescente para nenhuma partição de domínio, você poderá remover o catálogo global se ele fosse um GC.  

A remoção de objetos remanescentes é vantajosa para organizações maiores que não podem arriscar o tempo de inatividade associado às outras opções.  

Para obter mais informações, consulte [usar repadmin para remover objetos remanescentes](https://technet.microsoft.com/library/cc785298.aspx).

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
