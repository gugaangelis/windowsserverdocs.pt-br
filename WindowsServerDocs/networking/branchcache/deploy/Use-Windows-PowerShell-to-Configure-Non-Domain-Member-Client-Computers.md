---
title: Usar o Windows PowerShell para configurar os computadores cliente não membro de domínio
description: Este tópico faz parte do BranchCache implantação guia para o Windows Server 2016, que demonstra como implantar o BranchCache nos modos de cache hospedado e distribuído para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 1b511e1a-686d-441f-a1c7-d4d029e1a061
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9abb77e573d7b3f144ab831c655c81370a4a6af1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839947"
---
# <a name="use-windows-powershell-to-configure-non-domain-member-client-computers"></a>Usar o Windows PowerShell para configurar os computadores cliente não membro de domínio

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este procedimento para configurar manualmente um computador de cliente do BranchCache para o modo de cache distribuído ou modo de cache hospedado.  
  
> [!NOTE]  
> Se você configurou computadores cliente BranchCache usando a Diretiva de Grupo, as configurações da Diretiva de Grupo substituirão qualquer configuração manual de computadores cliente aos quais as diretivas foram aplicadas.  
  
A associação a **Administradores** ou equivalente é o requisito mínimo para a execução deste procedimento.  
  
### <a name="to-enable-branchcache-distributed-or-hosted-cache-mode"></a>Para habilitar o modo de cache distribuído ou hospedado do BranchCache  
  
1.  Sobre o computador cliente BranchCache que você deseja configurar, executar o Windows PowerShell como administrador e, em seguida, faça o seguinte.  
  
    -   Para configurar o computador cliente para o modo de cache distribuído do BranchCache, digite o seguinte comando e pressione ENTER.  
  
        `Enable-BCDistributed`  
  
    -   Para configurar o computador cliente para o modo de cache hospedado do BranchCache, digite o seguinte comando e pressione ENTER.  
  
        `Enable-BCHostedClient`  
  
        > [!TIP]  
        > Se você quiser especificar servidores de cache hospedado disponíveis, use o `-ServerNames` lista de seus servidores de cache hospedado, como o valor do parâmetro de separados de parâmetro com uma vírgula. Por exemplo, se você tiver dois servidores de cache hospedado chamados HCS1 e HCS2, configure o computador cliente para o modo de cache hospedado com o comando a seguir.  
        >   
        > `Enable-BCHostedClient -ServerNames HCS1,HCS2`  
  


