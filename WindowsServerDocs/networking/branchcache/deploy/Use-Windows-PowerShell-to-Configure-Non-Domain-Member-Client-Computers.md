---
title: Usar o Windows PowerShell para configurar computadores cliente que não são membros do domínio
description: Este tópico faz parte do guia de implantação do BranchCache para o Windows Server 2016, que demonstra como implantar o BranchCache em modos de cache distribuídos e hospedados para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 1b511e1a-686d-441f-a1c7-d4d029e1a061
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9743d93fe7bc21a971ff886a7e255eed3b775c97
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406431"
---
# <a name="use-windows-powershell-to-configure-non-domain-member-client-computers"></a>Usar o Windows PowerShell para configurar computadores cliente que não são membros do domínio

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este procedimento para configurar manualmente um computador cliente do BranchCache para o modo de cache distribuído ou o modo de cache hospedado.  
  
> [!NOTE]  
> Se você configurou computadores cliente BranchCache usando a Diretiva de Grupo, as configurações da Diretiva de Grupo substituirão qualquer configuração manual de computadores cliente aos quais as diretivas foram aplicadas.  
  
A associação a **Administradores** ou equivalente é o requisito mínimo para a execução deste procedimento.  
  
### <a name="to-enable-branchcache-distributed-or-hosted-cache-mode"></a>Para habilitar o modo de cache distribuído ou hospedado do BranchCache  
  
1.  No computador cliente do BranchCache que você deseja configurar, execute o Windows PowerShell como administrador e, em seguida, siga um destes procedimentos.  
  
    -   Para configurar o computador cliente para o modo de cache distribuído do BranchCache, digite o comando a seguir e pressione ENTER.  
  
        `Enable-BCDistributed`  
  
    -   Para configurar o computador cliente para o modo de cache hospedado do BranchCache, digite o comando a seguir e pressione ENTER.  
  
        `Enable-BCHostedClient`  
  
        > [!TIP]  
        > Se você quiser especificar os servidores de cache hospedados disponíveis, use o parâmetro `-ServerNames` com uma lista separada por vírgulas de seus servidores de cache hospedados como o valor do parâmetro. Por exemplo, se você tiver dois servidores de cache hospedados chamados HCS1 e HCS2, configure o computador cliente para o modo de cache hospedado com o comando a seguir.  
        >   
        > `Enable-BCHostedClient -ServerNames HCS1,HCS2`  
  


