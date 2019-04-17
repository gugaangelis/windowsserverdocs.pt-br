---
title: Usar o Windows PowerShell para configurar computadores cliente de não membro do domínio
description: Este tópico faz parte do BranchCache implantação guia para Windows Server 2016, que demonstra como implantar BranchCache nos modos de cache hospedado e distribuídos para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 1b511e1a-686d-441f-a1c7-d4d029e1a061
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a5415d9fa5a4af806f23a1af9c907b1f02e9627b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="use-windows-powershell-to-configure-non-domain-member-client-computers"></a>Usar o Windows PowerShell para configurar computadores cliente de não membro do domínio

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este procedimento para configurar manualmente um computador do cliente BranchCache para o modo de cache distribuído ou hospedado modo de cache.  
  
> [!NOTE]  
> Se você tiver configurado BranchCache os computadores cliente usando a política de grupo, as configurações de política de grupo substituem qualquer configuração manual de computadores cliente para que as políticas são aplicadas.  
  
A associação ao grupo **administradores**, ou equivalente é o requisito mínimo para executar este procedimento.  
  
### <a name="to-enable-branchcache-distributed-or-hosted-cache-mode"></a>Para habilitar o modo de cache hospedado ou distribuído BranchCache  
  
1.  No computador cliente em BranchCache que você deseja configurar, execute o Windows PowerShell como administrador e siga um destes procedimentos.  
  
    -   Para configurar o computador cliente para o modo de cache distribuído BranchCache, digite o seguinte comando e pressione ENTER.  
  
        `Enable-BCDistributed`  
  
    -   Para configurar o computador cliente para o modo de cache BranchCache hospedado, digite o seguinte comando e pressione ENTER.  
  
        `Enable-BCHostedClient`  
  
        > [!TIP]  
        > Se você quiser especificar os servidores de cache hospedado disponíveis, use o `-ServerNames` parâmetro com uma vírgula separados lista de seus servidores de cache hospedado como o valor do parâmetro. Por exemplo, se você tiver dois servidores de cache hospedado denominados HCS1 e HCS2, configure o computador cliente para o modo de cache hospedado com o comando a seguir.  
        >   
        > `Enable-BCHostedClient -ServerNames HCS1,HCS2`  
  


