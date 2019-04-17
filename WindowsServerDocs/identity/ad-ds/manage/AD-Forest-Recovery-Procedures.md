---
title: "Recuperação de floresta do AD - procedimentos"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: 47a471fb-3b0b-4aa8-8525-1c92d0d51e93
ms.technology: identity-adfs
ms.openlocfilehash: 91e3954c05fe3cd12d35b5db91afd29fb3a31e00
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/13/2017
---
# <a name="ad-forest-recovery---procedures"></a>Recuperação de floresta do AD - procedimentos


>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Esta seção contém procedimentos relacionados ao processo de recuperação de floresta. Os procedimentos são aplicáveis para o Windows Server 2016, 2012 R2, 2012 e também são aplicáveis ao Windows Server 2008 R2 e 2008 com algumas poucas exceções. 

Os procedimentos que incluem etapas variam de acordo para o Windows Server 2003 são encontrados em [recuperação da floresta com controladores de domínio do Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md).  

A seguir está uma lista de procedimentos que são usados em backup e restauração controladores de domínio e o Active Directory.
  
-   [Fazer backup de um servidor completo](AD-Forest-Recovery-Backing-up-a-Full-Server.md)  
-   [Backup dos dados de estado do sistema](AD-Forest-Recovery-Backing-up-System-State.md)  
-   [Realizar uma recuperação de servidor completo](AD-Forest-Recovery-Perform-a-Full-Recovery.md)  
-   [Executando uma sincronização autoritativa replicados DFSR SYSVOL](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)
-   [Executar uma restauração sem autorização dos serviços de domínio do Active Directory](AD-Forest-Recovery-Nonauthoritative-Restore.md)  
  
     Estas etapas explicam como executar uma restauração autorizada do SYSVOL ao mesmo tempo.  
-   [Configurar o serviço de servidor DNS](AD-Forest-Recovery-Configure-DNS.md)  
-   [Remover o catálogo global](AD-Forest-Recovery-Remove-GC.md)  
-   [Aumentando o valor de pools RID disponíveis](AD-Forest-Recovery-Raise-RID-Pool.md)  
-   [Invalidar o pool RID atual](AD-Forest-Recovery-Invaildate-RID-Pool.md)  
-   [Capturar uma função mestre de operações](AD-Forest-Recovery-Seizing-Operations-Master-Role.md)  
-   [Limpando após uma restauração](AD-Forest-Recovery-Cleanup.md)
-   [Limpeza de metadados de controladores de domínio gravável removido](AD-Forest-Recovery-Cleaning-Metadata.md)  
-   [Redefinir a senha da conta de computador do controlador de domínio](AD-Forest-Recovery-Reset-Computer-Account-DC.md)  
-   [Redefinir a senha krbtgt](AD-Forest-Recovery-Resetting-the-krbtgt-password.md)  
-   [Redefinir uma senha de confiança em um lado da relação de confiança](AD-Forest-Recovery-Reset-Trust.md)  
-   [Adicionando o catálogo global](AD-Forest-Recovery-Add-GC.md)  
-   [Recursos para verificar replicação está funcionando](AD-Forest-Recovery-Verify-Replication.md)  
  
  
## <a name="next-steps"></a>Próximas etapas
-   [Recuperação de floresta do AD - pré-requisitos](AD-Forest-Recovery-Prerequisties.md)  
-   [Recuperação de floresta do AD - descobrindo um plano de recuperação personalizada floresta](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Recuperação de floresta do AD - identificar o problema](AD-Forest-Recovery-Identify-the-Problem.md)
-   [Recuperação de floresta do AD - determinar como recuperar](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [Recuperação de floresta do AD - realizar uma recuperação inicial](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [Recuperação de floresta do AD - procedimentos](AD-Forest-Recovery-Procedures.md)  
-   [Recuperação de floresta do AD - perguntas frequentes](AD-Forest-Recovery-FAQ.md)  
-   [Recuperação de floresta do AD - recuperando um único domínio em uma floresta Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [AD floresta recuperação - floresta com controladores de domínio do Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md) 
