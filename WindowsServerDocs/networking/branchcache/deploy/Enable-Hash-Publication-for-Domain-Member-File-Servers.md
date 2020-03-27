---
title: Habilitar publicação de hash para servidores de arquivos associados ao domínio
description: Este tópico faz parte do guia de implantação do BranchCache para o Windows Server 2016, que demonstra como implantar o BranchCache em modos de cache distribuídos e hospedados para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: a3f1f7c4-d9b2-43e6-8bfa-fac707bbd4d3
ms.author: lizross
author: eross-msft
ms.openlocfilehash: dd39a8d7f08e3ac3e6249017a042c343d9179566
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319282"
---
# <a name="enable-hash-publication-for-domain-member-file-servers"></a>Habilitar publicação de hash para servidores de arquivos associados ao domínio

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Quando estiver usando Active Directory Domain Services (AD DS), você poderá usar o Política de Grupo de domínio para habilitar a publicação de hash do BranchCache para vários servidores de arquivos. Para fazer isso, você deve criar uma UO (unidade organizacional), adicionar servidores de arquivos à UO, criar uma publicação de hash do BranchCache Política de Grupo objeto (GPO) e, em seguida, configurar o GPO.  
  
Consulte os tópicos a seguir para habilitar a publicação de hash para vários servidores de arquivos.  
  
-   [Criar a unidade organizacional de servidores de arquivos do BranchCache](../../branchcache/deploy/Create-the-BranchCache-File-Servers-Organizational-Unit.md)  
  
-   [Mover servidores de arquivos para a unidade organizacional servidores de arquivos do BranchCache](../../branchcache/deploy/Move-File-Servers-to-the-BranchCache-File-Servers-Organizational-Unit.md)  
  
-   [Criar a publicação de hash do BranchCache Política de Grupo objeto](../../branchcache/deploy/Create-the-BranchCache-Hash-Publication-Group-Policy-Object.md)  
  
-   [Configurar a publicação de hash do BranchCache Política de Grupo objeto](../../branchcache/deploy/Configure-the-BranchCache-Hash-Publication-Group-Policy-Object.md)  
  


