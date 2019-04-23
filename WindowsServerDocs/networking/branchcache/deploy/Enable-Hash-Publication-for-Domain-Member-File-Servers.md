---
title: Habilitar publicação de hash para servidores de arquivos associados ao domínio
description: Este tópico faz parte do BranchCache implantação guia para o Windows Server 2016, que demonstra como implantar o BranchCache nos modos de cache hospedado e distribuído para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: a3f1f7c4-d9b2-43e6-8bfa-fac707bbd4d3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 174e83c950d2aff8afba4f05641a74861b9a7938
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865457"
---
# <a name="enable-hash-publication-for-domain-member-file-servers"></a>Habilitar publicação de hash para servidores de arquivos associados ao domínio

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Quando você estiver usando os serviços de domínio do Active Directory (AD DS), você pode usar a diretiva de grupo do domínio para habilitar a publicação de hash do BranchCache para vários servidores de arquivos. Para fazer isso, você deve criar uma unidade organizacional (UO), adicione os servidores de arquivos à UO, crie uma publicação de hash do BranchCache objeto de diretiva de grupo (GPO) e, em seguida, configure o GPO.  
  
Consulte os tópicos a seguir para habilitar a publicação de hash para vários servidores de arquivos.  
  
-   [Criar unidade organizacional de servidores de arquivos BranchCache](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md)  
  
-   [Mover servidores de arquivos para a unidade organizacional de servidores de arquivos BranchCache](../../branchcache/deploy/Move-File-Servers-to-the-BranchCache-File-Servers-Organizational-Unit.md)  
  
-   [Criar o objeto de diretiva de grupo de publicação de Hash do BranchCache](../../branchcache/deploy/Create-the-BranchCache-Hash-Publication-Group-Policy-Object.md)  
  
-   [Configurar o objeto de diretiva de grupo de publicação de Hash do BranchCache](../../branchcache/deploy/Configure-the-BranchCache-Hash-Publication-Group-Policy-Object.md)  
  


