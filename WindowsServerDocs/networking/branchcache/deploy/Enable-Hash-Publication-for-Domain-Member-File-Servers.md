---
title: Habilitar publicação de hash para servidores de arquivos associados ao domínio
description: Este tópico faz parte do guia de implantação do BranchCache para o Windows Server 2016, que demonstra como implantar o BranchCache em modos de cache distribuídos e hospedados para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: a3f1f7c4-d9b2-43e6-8bfa-fac707bbd4d3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1e450b9a2282cb4820b8802aa6d36e822f56ca12
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356585"
---
# <a name="enable-hash-publication-for-domain-member-file-servers"></a>Habilitar publicação de hash para servidores de arquivos associados ao domínio

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Quando estiver usando Active Directory Domain Services (AD DS), você poderá usar o Política de Grupo de domínio para habilitar a publicação de hash do BranchCache para vários servidores de arquivos. Para fazer isso, você deve criar uma UO (unidade organizacional), adicionar servidores de arquivos à UO, criar uma publicação de hash do BranchCache Política de Grupo objeto (GPO) e, em seguida, configurar o GPO.  
  
Consulte os tópicos a seguir para habilitar a publicação de hash para vários servidores de arquivos.  
  
-   [Criar a unidade organizacional de servidores de arquivos do BranchCache](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md)  
  
-   [Mover servidores de arquivos para a unidade organizacional servidores de arquivos do BranchCache](../../branchcache/deploy/Move-File-Servers-to-the-BranchCache-File-Servers-Organizational-Unit.md)  
  
-   [Criar a publicação de hash do BranchCache Política de Grupo objeto](../../branchcache/deploy/Create-the-BranchCache-Hash-Publication-Group-Policy-Object.md)  
  
-   [Configurar a publicação de hash do BranchCache Política de Grupo objeto](../../branchcache/deploy/Configure-the-BranchCache-Hash-Publication-Group-Policy-Object.md)  
  


