---
title: "Virtualização de recuperação de floresta do AD"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.technology: identity-adfs
ms.openlocfilehash: 08b0c4a4c5ed230f91241fcfb67db1749935c5e2
ms.sourcegitcommit: 84a2bdcb92ba6af45781fab9727617e50fa5e911
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/07/2017
---
# <a name="active-directory-forest-recovery-virtualization"></a>Virtualização de recuperação de floresta do Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Este tópico descreve o recurso de clonagem de controlador de domínio virtualizado no 2012, 2012 R2 e Windows Server 2016.  
 
## <a name="using-virtualized-domain-controller-cloning-to-expedite-forest-recovery"></a>Usando clonagem de controlador de domínio virtualizado para agilizar a recuperação de floresta  
 Clonagem de (DC) do controlador de domínio virtualizado simplifica e agiliza o processo de instalação adicionais virtualizados controladores de domínio em um domínio, especialmente em locais centralizados, como data centers onde vários controladores de domínio é executado em hipervisores. Depois de restaurar um controlador de domínio virtual em cada domínio de backup, controladores de domínio adicionais em cada domínio podem ser rapidamente trazidos online usando o processo de clonagem virtualizada do controlador de domínio. Você pode preparar o controlador de domínio virtualizado primeiro que você recuperar, desligá-lo e, em seguida, copie esse disco rígido virtual como quantas vezes for necessário para criar clonados virtualizados controladores de domínio para criar o domínio.  
  
 Os requisitos para virtualizado DC clonagem são:  
  
-   O hipervisor deve dar suporte a VM-GenerationID. Hyper-V no Windows Server 2016, 2012 e Windows 8 é um exemplo de um hipervisor que dá suporte a VM-GenerationID. Verifique com o fornecedor de hipervisor se VM-GenerationID é suportado.  
  
-   O controlador de domínio virtualizado é usado como uma fonte para clonagem deve executar o Windows Server 2016 ou 2012 e ser um membro do grupo Cloneable controladores de domínio.  
  
-   O emulador do PDC deve executar o Windows Server 2016 ou 2012. Você pode clonar emulador do PDC se ele é virtualizado.  
  
 Para instruções passo a passo sobre como executar virtualizados DC clonagem, consulte [Introdução aos serviços de domínio do Active Directory (AD DS) virtualização (nível 100)](../Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md). Para obter detalhes sobre como virtualizados DC clonagem funciona, consulte [virtualizados domínio controlador referência técnica (nível 300)](../deploy/virtual-dc/virtualized-domain-controller-technical-reference--level-300-.md).  

-   [Recuperação de floresta do AD - pré-requisitos](AD-Forest-Recovery-Prerequisties.md)  
-   [Recuperação de floresta do AD - descobrindo um plano de recuperação personalizada floresta](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Recuperação de floresta do AD - identificar o problema](AD-Forest-Recovery-Identify-the-Problem.md)
-   [Recuperação de floresta do AD - determinar como recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [Recuperação de floresta do AD - realizar uma recuperação inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)  
-   [Recuperação de floresta do AD - perguntas frequentes](AD-Forest-Recovery-FAQ.md)  
-   [Recuperação de floresta do AD - recuperando um único domínio em uma floresta Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [AD floresta recuperação - floresta com controladores de domínio do Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md) 

  
