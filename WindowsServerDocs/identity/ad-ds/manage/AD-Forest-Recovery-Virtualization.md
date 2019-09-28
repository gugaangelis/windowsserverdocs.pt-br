---
title: Virtualização de recuperação de floresta do AD
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.technology: identity-adds
ms.openlocfilehash: c055445c2d3aecd8c6d92e94799f556962c977bf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71390126"
---
# <a name="active-directory-forest-recovery-virtualization"></a>Virtualização de recuperação de floresta Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Este tópico descreve o recurso de clonagem do controlador de domínio virtualizado no Windows Server 2016, 2012 R2 e 2012.  

## <a name="using-virtualized-domain-controller-cloning-to-expedite-forest-recovery"></a>Usando a clonagem do controlador de domínio virtualizado para agilizar a recuperação da floresta

A clonagem do DC (controlador de domínio virtualizado) simplifica e agiliza o processo de instalação de DCs virtualizados adicionais em um domínio, especialmente em locais centralizados, como data centers em que vários DCs são executados em hipervisores. Depois de restaurar um DC virtual em cada domínio do backup, os DCs adicionais em cada domínio podem ser rapidamente colocados online usando o processo de clonagem de DC virtualizado. Você pode preparar o primeiro controlador de domínio virtualizado recuperado, desligá-lo e, em seguida, copiar esse disco rígido virtual quantas vezes forem necessárias para criar DCs virtualizados clonados para compilar o domínio.  
  
Os requisitos para a clonagem de DC virtualizado são:  
  
- O hipervisor deve dar suporte a VM-Generationid. O Hyper-V no Windows Server 2016, 2012 e Windows 8 é um exemplo de um hipervisor que dá suporte a VM-Generationid. Verifique com seu fornecedor de hipervisor se há suporte para VM-Generationid.  
- O DC virtualizado que é usado como uma fonte para clonagem deve executar o Windows Server 2016 ou 2012 e ser um membro do grupo de controladores de domínio clonáveis. 
- O emulador de PDC deve executar o Windows Server 2016 ou 2012. Você pode clonar o emulador PDC se ele for virtualizado.  
  
Para obter instruções detalhadas sobre como executar a clonagem de DC virtualizado, consulte [introdução à virtualização de Active Directory Domain Services (AD DS) (nível 100)](../Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md). Para obter detalhes sobre como funciona a clonagem de DC virtualizado, consulte [referência técnica do controlador de domínio virtualizado (nível 300)](../deploy/virtual-dc/virtualized-domain-controller-technical-reference--level-300-.md). 

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
