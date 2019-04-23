---
title: Recuperação de floresta do AD - procedimentos
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 47a471fb-3b0b-4aa8-8525-1c92d0d51e93
ms.technology: identity-adds
ms.openlocfilehash: da45e3b20c370a2a37b0eab31a78216434dd60be
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827717"
---
# <a name="ad-forest-recovery---procedures"></a>Recuperação de floresta do AD - procedimentos

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Esta seção contém procedimentos relacionados ao processo de recuperação de floresta. Os procedimentos são aplicáveis para o Windows Server 2016, 2012 R2, 2012 e também são aplicáveis ao Windows Server 2008 R2 e o 2008 com algumas exceções secundárias.

Os procedimentos que incluem etapas que variam de acordo para o Windows Server 2003 estão localizados no [recuperação de floresta com controladores de domínio do Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md).  

O exemplo a seguir é uma lista de procedimentos que são usados no backup e restauração de controladores de domínio e o Active Directory.

- [Fazendo backup de um servidor completo](AD-Forest-Recovery-Backing-up-a-Full-Server.md)  
- [Backup de dados de estado do sistema](AD-Forest-Recovery-Backing-up-System-State.md)  
- [Executar uma recuperação de servidor completo](AD-Forest-Recovery-Perform-a-Full-Recovery.md)  
- [Executar uma sincronização autoritativa de DFSR-replicated SYSVOL](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)
- [Executar uma restauração não autoritativa de serviços de domínio do Active Directory](AD-Forest-Recovery-Nonauthoritative-Restore.md)  

Estas etapas explicam como realizar uma restauração autoritativa do SYSVOL, ao mesmo tempo.  

- [Configurando o serviço servidor DNS](AD-Forest-Recovery-Configure-DNS.md)  
- [Remover o catálogo global](AD-Forest-Recovery-Remove-GC.md)  
- [Aumentando o valor de pools RID disponíveis](AD-Forest-Recovery-Raise-RID-Pool.md)  
- [Invalidar o pool RID atual](AD-Forest-Recovery-Invaildate-RID-Pool.md)  
- [Captura de uma função de mestre de operações](AD-Forest-Recovery-Seizing-Operations-Master-Role.md)  
- [Limpeza após uma restauração](AD-Forest-Recovery-Cleanup.md)
- [Limpeza de metadados de controladores de domínio graváveis removidos](AD-Forest-Recovery-Cleaning-Metadata.md)  
- [Redefinindo a senha da conta de computador do controlador de domínio](AD-Forest-Recovery-Reset-Computer-Account-DC.md)  
- [Redefinir a senha de krbtgt](AD-Forest-Recovery-Resetting-the-krbtgt-password.md)  
- [Redefinir uma senha de relação de confiança no um lado da relação de confiança](AD-Forest-Recovery-Reset-Trust.md)  
- [Adicionando o catálogo global](AD-Forest-Recovery-Add-GC.md)  
- [Recursos para verificar a replicação está funcionando](AD-Forest-Recovery-Verify-Replication.md)  

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
