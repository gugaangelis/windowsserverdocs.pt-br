---
title: AD floresta recuperação - identificar o problema
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 5a291f65-794e-4fc3-996e-094c5845a383
ms.technology: identity-adds
ms.openlocfilehash: cb3cf45f778ff305c8fe5aad5e23f0257650d6f5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865517"
---
# <a name="identify-the-problem"></a>Identificar o problema

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2
  
Quando os sintomas de uma falha de toda a floresta são exibidos, como em logs de eventos ou outras soluções de monitoramento, trabalhar com o Microsoft Support para determinar a causa da falha e avaliar qualquer as possíveis soluções.  

## <a name="examples-of-forest-wide-failures"></a>Exemplos de falhas de toda a floresta

- Todos os controladores de domínio foram logicamente corrompidas ou danificadas fisicamente para um ponto que a continuidade dos negócios é impossível; Por exemplo, todos os aplicativos de negócios que dependem do AD DS não estão funcionais.  
- Um administrador invasor comprometeu o ambiente do Active Directory.  
- Um invasor intencionalmente — ou um administrador acidentalmente — executa um script que se espalha a corrupção de dados em toda a floresta.  
- Um invasor intencionalmente — ou um administrador acidentalmente — estende o esquema do Active Directory com alterações conflitantes ou mal-intencionado.  
- Um invasor conseguiu instalar o software mal-intencionado em controladores de domínio, e você tiver sido informado pela Microsoft Support para recuperar a floresta de backup.  
  
   > [!IMPORTANT]
   >  Este documento não aborda as recomendações de segurança sobre como recuperar uma floresta que foi invadido por hackers ou comprometida. Em geral, recomenda-se às técnicas de mitigação siga Pass-the-Hash para proteger o ambiente. Para obter mais informações, consulte [ataques Mitigating Pass-the-Hash (PtH) e outras técnicas de roubo de credencial](https://www.microsoft.com/download/details.aspx?id=36036).
  
- Nenhum dos controladores de domínio pode replicar com seus parceiros de replicação.  
- Não é possível fazer alterações para o AD DS em qualquer controlador de domínio.  
- Novos controladores de domínio não podem ser instalados em qualquer domínio.  
  
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
