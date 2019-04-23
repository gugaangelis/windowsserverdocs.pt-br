---
title: Recuperação de floresta do AD - recuperação de um único domínio em uma floresta de vários domínios
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 267541be-2ea7-4af6-ab34-8b5a3fedee2d
ms.technology: identity-adds
ms.openlocfilehash: fae2cc40af0b43dd38d72c2622720a6bb17b0a66
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863467"
---
# <a name="ad-forest-recovery---recovering-a-single-domain-in-a-multidomain-forest"></a>Recuperação de floresta do AD - recuperação de um único domínio em uma floresta de vários domínios

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Pode haver ocasiões em que é necessário recuperar um único domínio em uma floresta que tenha vários domínios, em vez de uma recuperação de floresta completa. Este tópico aborda considerações sobre a recuperação de um único domínio e possíveis estratégias para recuperação.  
  
Recuperação de um único domínio apresenta um desafio único para a recriação de servidores de catálogo global (GC). Por exemplo, se o primeiro controlador de domínio (DC) para o domínio for restaurado de um backup que foi criado anteriormente uma semana, em seguida, todos os outros GCs na floresta terá mais dados atualizados para esse domínio que o controlador de domínio restaurado. Para restabelecer a consistência dos dados de GC, há algumas opções:  
  
- Unhost e, em seguida, hospede novamente a partição de domínios recuperados de todos os GCs na floresta, exceto aqueles no domínio recuperado, ao mesmo tempo.  
- Siga o processo de recuperação de floresta para recuperar o domínio e, em seguida, remover objetos remanescentes de GCs em outros domínios.  
  
As seções a seguir fornecem considerações gerais para cada opção. O conjunto completo de etapas que precisam ser feitas para a recuperação variam para diferentes ambientes do Active Directory.  
  
## <a name="rehost-all-gcs"></a>Hospede novamente todos os GCs  

> [!WARNING]
> A senha da conta de administrador de domínio para todos os domínios deve esteja pronta para uso no caso de um problema impede o acesso a um GC para logon.  

Hospedando novamente todos os GCs pode ser feito usando repadmin / unhost e repadmin /rehost comandos (parte do repadmin /experthelp). Você executaria comandos repadmin em cada GC em cada domínio que não esteja recuperado. Ele precisa ser garantida, que todos os GCs não manter uma cópia do domínio recuperado mais. Para fazer isso, unhost a partição de domínio pela primeira vez de todos os controladores de domínio em todos os domínios recuperado none da floresta pela primeira vez. Depois que todos os GCs não contêm mais a partição, você pode hospedar novamente-lo. Ao rehosting, considere a estrutura site e a replicação da sua floresta, por exemplo, o novo host de um controlador de domínio por site antes da nova hospedagem os outros controladores de domínio do site de conclusão.  
  
Essa opção pode ser vantajosa para uma organização pequena que tem apenas alguns controladores de domínio para cada domínio. Todos os GCs poderia ser recompilados em uma noite de sexta-feira e, se necessário, concluir a replicação de domínio somente leitura todas as partições antes da manhã de segunda-feira. Mas, se você precisar recuperar um grande domínio que abrange os sites em todo o mundo, rehosting a partição de domínio somente leitura em todos os GCs para outros domínios podem significativamente as operações de impacto e potencialmente exigir tempo de inatividade.  
  
## <a name="remove-lingering-objects"></a>Remover objetos remanescentes

Semelhante ao processo de recuperação de floresta, você restaurar um controlador de domínio de backup no domínio que você precisa recuperar, execute a limpeza de metadados de controladores de domínio restantes e, em seguida, reinstalar o AD DS para criar o domínio. No GCs de todos os outros domínios na floresta, é possível remover os objetos remanescentes para a partição de somente leitura do domínio recuperado.  

A fonte para a limpeza remanescentes do objeto deve ser um controlador de domínio no domínio recuperado. Para ter certeza de que o DC de origem não tem todos os objetos remanescentes para as partições de domínio, você pode remover o catálogo global se fosse um GC.  

Remover objetos remanescentes é vantajoso para organizações maiores que não podem se arriscar o tempo de inatividade associado com as outras opções.  

Para obter mais informações, consulte [Use Repadmin para remover objetos remanescentes](https://technet.microsoft.com/library/cc785298.aspx).

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
