---
title: Virtualização de recuperação de floresta do AD
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: c49b40b2-598d-49aa-85b4-766bce960e0d
ms.technology: identity-adds
ms.openlocfilehash: 23317f55fdce18e78ac3e7e1490f6fc4937fd062
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858447"
---
# <a name="active-directory-forest-recovery-virtualization"></a>Virtualização de recuperação de floresta do Active Directory

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Este tópico descreve o recurso de clonagem do controlador de domínio virtualizados no Windows Server 2016, 2012 R2 e 2012.  

## <a name="using-virtualized-domain-controller-cloning-to-expedite-forest-recovery"></a>Usando a clonagem do controlador de domínio virtualizado para agilizar a recuperação de floresta

Clonagem de DC (controlador) de domínio virtualizado simplifica e agiliza o processo de instalação de controladores de domínio virtualizados adicionais em um domínio, especialmente em locais centralizados como data centers onde vários controladores de domínio executam em hipervisores. Depois de restaurar um controlador de domínio virtual em cada domínio de backup, os controladores de domínio adicionais em cada domínio podem rapidamente ficar online usando o processo de clonagem de DC virtualizado. Você pode preparar o controlador de domínio virtualizado primeiro que você recupera, desligá-lo e, em seguida, copie esse disco rígido virtual como quantas vezes forem necessárias para criar clonado os DCs virtualizados para criar o domínio.  
  
Os requisitos para clonagem de controlador de domínio virtualizado são:  
  
- O hipervisor deve dar suporte a VM-GenerationID. Hyper-V no Windows Server 2016, 2012 e Windows 8 é um exemplo de um hipervisor que dá suporte a VM-GenerationID. Verifique com o fornecedor do hipervisor se há suporte para a VM-GenerationID.  
- O controlador de domínio virtualizado que é usado como uma fonte para clonagem deve executar o Windows Server 2016 ou 2012 e ser um membro do grupo de controladores de domínio Clonáveis. 
- O emulador do PDC deve executar o Windows Server 2016 ou 2012. Você pode clonar o emulador PDC se ele é virtualizado.  
  
Para obter instruções passo a passo sobre como executar virtualizado clonagem do controlador de domínio, consulte [Introdução à virtualização de serviços de domínio Active Directory (AD DS) (nível 100)](../Introduction-to-Active-Directory-Domain-Services-AD-DS-Virtualization-Level-100.md). Para obter detalhes sobre como virtualizado funciona a clonagem do controlador de domínio, consulte [virtualizados domínio controlador de referência técnica (nível 300)](../deploy/virtual-dc/virtualized-domain-controller-technical-reference--level-300-.md). 

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
