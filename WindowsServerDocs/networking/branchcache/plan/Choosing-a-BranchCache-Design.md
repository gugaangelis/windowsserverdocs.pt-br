---
title: Escolher um design para o BranchCache
description: Este tópico faz parte do guia de implantação do BranchCache para o Windows Server 2016, que demonstra como implantar o BranchCache em modos de cache distribuídos e hospedados para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 86c1ccad-2aa4-40fe-84c1-f77c49eb1216
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2a1b4615dfda3989f0321725fd27da066fcb8a5e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406318"
---
# <a name="choosing-a-branchcache-design"></a>Escolher um design para o BranchCache

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para saber mais sobre os modos de BranchCache e para selecionar os melhores modos para sua implantação.  
  
Você pode usar este guia para implantar o BranchCache nos modos e combinações de modo a seguir.  
  
-   Todas as filiais são configuradas para o modo de cache distribuído.  
  
-   Todas as filiais são configuradas para o modo de cache hospedado e têm um servidor de cache hospedado no site.  
  
-   Algumas filiais estão configuradas para o modo de cache distribuído e algumas filiais têm um servidor de cache hospedado no site e são configuradas para o modo de cache hospedado.  
  
A ilustração a seguir descreve uma instalação de modo duplo, com uma filial configurada para o modo de cache distribuído e uma filial configurada para o modo de cache hospedado.  
  
![Escolher um design do BranchCache](../../media/Choosing-a-BranchCache-Design/bc_new_modes.jpg)  
  
Antes de implantar o BranchCache, selecione o modo que você prefere para cada filial em sua organização.  
  


