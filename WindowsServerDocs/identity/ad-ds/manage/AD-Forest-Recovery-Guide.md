---
ms.assetid: 680e05ac-f9be-4b07-a9f4-cd6da5835952
title: Guia de recuperação de floresta do Active Directory
description: Este guia fornece orientações sobre backup, restauração e recuperação de desastres do Active Directory.
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4ad3d3648dddfdc1266afdba07d1c812495f41b8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868187"
---
# <a name="active-directory-forest-recovery-guide"></a>Guia de recuperação de floresta do Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2, Windows Server 2003

Este guia contém as práticas recomendadas para a recuperação de uma floresta Active Directory® se a falha de toda a floresta renderiza todos os controladores de domínio (DCs) na floresta incapazes de funcionando normalmente. As etapas que ele contém servem como um modelo para seu plano de recuperação de floresta, que você pode personalizar para seu ambiente específico. Essas etapas se aplicam aos controladores de domínio que executam o Microsoft® Windows Server 2016, 2012 R2, 2012, 2008 R2, 2008 e 2003 sistemas operacionais.  
  
> [!NOTE]
> Os procedimentos que são exclusivos para controladores de domínio que executam o Windows Server 2003 são consolidados [AD floresta recuperação Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md).  
  
## <a name="steps-outlined-in-this-guide"></a>Etapas descritas neste guia
  
- [Recuperação de floresta do AD - pré-requisitos](AD-Forest-Recovery-Prerequisties.md)  
- [Recuperação de floresta do AD - elaborar um plano de recuperação de floresta personalizado](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Recuperação de floresta do AD - etapas de recuperação](AD-Forest-Recovery-Steps-For-Restoring.md)
- [Recuperação de floresta do AD - identificar o problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Recuperação de floresta do AD - determinar como recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Recuperação de floresta do AD - executar a recuperação inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)  
- [Recuperação de floresta do AD - perguntas frequentes](AD-Forest-Recovery-FAQ.md)  
- [Recuperação de floresta do AD - recuperação de um único domínio dentro de uma floresta Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Recuperação de floresta do AD - virtualização](AD-Forest-Recovery-Virtualization.md)
- [Recuperação de floresta do AD - recuperação de floresta com controladores de domínio do Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)  
