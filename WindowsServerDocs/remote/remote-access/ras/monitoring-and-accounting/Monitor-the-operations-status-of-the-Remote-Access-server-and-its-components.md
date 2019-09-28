---
title: Monitorar o status da operação do servidor de acesso Remoto e seus componentes
description: Este tópico faz parte do guia de monitoramento e contabilidade de acesso remoto no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 077a3a64-2fa3-4994-9711-ec1fbdc081ba
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d0ad63ec88a428239a174a0217db94c44ab799bc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404551"
---
# <a name="monitor-the-operations-status-of-the-remote-access-server-and-its-components"></a>Monitorar o status da operação do servidor de acesso Remoto e seus componentes

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

**Observação:** O Windows Server 2012 reúne o DirectAccess e o RRAS (Serviço de Roteamento e Acesso Remoto) em uma única função de Acesso Remoto.  
  
O console de gerenciamento no servidor de acesso remoto pode ser usado para monitorar seu status de operações.  
  
> [!NOTE]  
> Você deve estar conectado como um membro do grupo Admins. do domínio ou um membro do grupo Administradores em cada computador para concluir a tarefa descrita neste tópico. Se você não puder concluir uma tarefa enquanto estiver conectado com uma conta que seja membro do grupo Administradores, tente executar a tarefa enquanto estiver conectado com uma conta que seja membro do grupo Admins. do domínio.  
  
#### <a name="to-monitor-the-remote-access-server-operations-status"></a>Para monitorar o status de operações do servidor de acesso remoto  
  
1.  No **Gerenciador do Servidor**, clique em **Ferramentas** e, em seguida, clique em **Gerenciamento de Acesso Remoto**.  
  
2.  Clique em **painel** para navegar até **relatórios de acesso remoto** no console de gerenciamento de **acesso remoto**.  
  
3.  No painel Monitoramento, observe o bloco **status de operações** no bloco **status do servidor** . Esse bloco lista o status de operações do servidor e o status de todos os componentes do servidor.  
  
4.  Clique em **Atualizar** em **tarefas** no painel direito para recarregar o status das operações. O status de operações é atualizado automaticamente a cada cinco minutos, que é o intervalo de atualização padrão. Para alterar o intervalo de atualização padrão, clique em **Configurar intervalo de atualização**.  
  
0Windows-](../../../media/Monitor-the-operations-status-of-the-Remote-Access-server-and-its-components/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell</em> do PowerShell do @no__t***  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
> [!NOTE]  
> O comando para o status de operações de um cluster é incluído para referência.  
  
```  
PS> Get-RemoteAccessHealth  
PS> Get-RemoteAccessHealth -Cluster  
```  
  


