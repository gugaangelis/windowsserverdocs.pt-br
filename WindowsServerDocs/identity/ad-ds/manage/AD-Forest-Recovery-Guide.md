---
ms.assetid: 680e05ac-f9be-4b07-a9f4-cd6da5835952
title: Guia de recuperação de floresta Active Directory
description: Este guia fornece orientação sobre como fazer backup, restauração e recuperação de desastres do Active Directory.
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.openlocfilehash: 94f6235c06e89d506f24b71d7f086c8ca007d7b3
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88939676"
---
# <a name="active-directory-forest-recovery-guide"></a>Guia de recuperação de floresta Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2, Windows Server 2003

Este guia contém recomendações de práticas recomendadas para a recuperação de um Active Directory® floresta se a falha em toda a floresta renderiza todos os controladores de domínio (DCs) na floresta incapaz de funcionar normalmente. As etapas que ele contém funcionam como um modelo para o plano de recuperação da floresta, que você pode personalizar para seu ambiente específico. Essas etapas se aplicam a DCs que executam os sistemas operacionais Microsoft® Windows Server 2016, 2012 R2, 2012, 2008 R2, 2008 e 2003.

> [!NOTE]
> Os procedimentos exclusivos para DCs que executam o Windows Server 2003 são consolidados na [recuperação de floresta do AD no Windows server 2003](AD-Forest-Recovery-Windows-Server-2003.md).

## <a name="steps-outlined-in-this-guide"></a>Etapas descritas neste guia

- [Recuperação de floresta do AD – Pré-requisitos](AD-Forest-Recovery-Prerequisties.md)
- [Recuperação de floresta do AD-planejar um plano de recuperação de floresta personalizado](AD-Forest-Recovery-Devising-a-Plan.md)
- [Recuperação de floresta do AD – Etapas para recuperação](AD-Forest-Recovery-Steps-For-Restoring.md)
- [Recuperação de floresta do AD – identificar o problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Recuperação de floresta do AD-determine como recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Recuperação de floresta do AD-executar recuperação inicial](AD-Forest-Recovery-Perform-initial-recovery.md)
- [Recuperação de floresta do AD – Procedimentos](AD-Forest-Recovery-Procedures.md)
- [Recuperação de floresta do AD-perguntas frequentes](AD-Forest-Recovery-FAQ.md)
- [Recuperação de floresta do AD-recuperando um único domínio em uma floresta de multidomínio](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)
- [Recuperação de floresta do AD – Virtualização](AD-Forest-Recovery-Virtualization.md)
- [Recuperação de floresta do AD-recuperação de floresta com controladores de domínio do Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)
