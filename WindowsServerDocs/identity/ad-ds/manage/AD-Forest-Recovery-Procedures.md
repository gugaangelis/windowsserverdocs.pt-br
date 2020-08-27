---
title: Recuperação de floresta do AD - procedimentos
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.date: 08/09/2018
ms.topic: article
ms.assetid: 47a471fb-3b0b-4aa8-8525-1c92d0d51e93
ms.openlocfilehash: 2bd333d2ae6d27facf5e3c5b31a113b13a1cd11c
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88939475"
---
# <a name="ad-forest-recovery---procedures"></a>Recuperação de floresta do AD - procedimentos

>Aplica-se a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2

Esta seção contém procedimentos relacionados ao processo de recuperação da floresta. Os procedimentos são aplicáveis para o Windows Server 2016, 2012 R2, 2012 e também são aplicáveis ao Windows Server 2008 R2 e ao 2008 com algumas exceções secundárias.

Os procedimentos que incluem etapas que variam para o Windows Server 2003 são encontrados em [recuperação de floresta com controladores de domínio do Windows server 2003](AD-Forest-Recovery-Windows-Server-2003.md).

Veja a seguir uma lista de procedimentos que são usados para fazer backup e restaurar controladores de domínio e Active Directory.

- [Fazendo backup de um servidor completo](AD-Forest-Recovery-Backing-up-a-Full-Server.md)
- [Fazendo backup dos dados de estado do sistema](AD-Forest-Recovery-Backing-up-System-State.md)
- [Executando uma recuperação completa do servidor](AD-Forest-Recovery-Perform-a-Full-Recovery.md)
- [Executando uma sincronização autoritativa de SYSVOL replicado pelo DFSR](AD-Forest-Recovery-Authoritative-Recovery-SYSVOL.md)
- [Executando uma restauração não autoritativa de Active Directory Domain Services](AD-Forest-Recovery-Nonauthoritative-Restore.md)

Estas etapas explicam como executar uma restauração autoritativa do SYSVOL ao mesmo tempo.

- [Configurando o serviço do servidor DNS](AD-Forest-Recovery-Configure-DNS.md)
- [Removendo o catálogo global](AD-Forest-Recovery-Remove-GC.md)
- [Elevando o valor dos pools RID disponíveis](AD-Forest-Recovery-Raise-RID-Pool.md)
- [Invalidando o pool RID atual](AD-Forest-Recovery-Invaildate-RID-Pool.md)
- [Capturando uma função de mestre de operações](AD-Forest-Recovery-Seizing-Operations-Master-Role.md)
- [Limpando após uma restauração](AD-Forest-Recovery-Cleanup.md)
- [Limpando metadados de controladores de domínio graváveis removidos](AD-Forest-Recovery-Cleaning-Metadata.md)
- [Redefinindo a senha da conta de computador do controlador de domínio](AD-Forest-Recovery-Reset-Computer-Account-DC.md)
- [Redefinindo a senha krbtgt](AD-Forest-Recovery-Resetting-the-krbtgt-password.md)
- [Redefinindo uma senha de confiança em um lado da relação de confiança](AD-Forest-Recovery-Reset-Trust.md)
- [Adicionando o catálogo global](AD-Forest-Recovery-Add-GC.md)
- [Recursos para verificar se a replicação está funcionando](AD-Forest-Recovery-Verify-Replication.md)

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
