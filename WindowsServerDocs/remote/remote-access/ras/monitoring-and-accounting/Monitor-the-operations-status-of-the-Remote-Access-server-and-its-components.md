---
title: Monitorar o status da operação do servidor de acesso Remoto e seus componentes
description: Este tópico faz parte do guia de monitoramento de acesso remoto e contabilização no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 077a3a64-2fa3-4994-9711-ec1fbdc081ba
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7f4c7e1418e541e1f913c8a20cbda3456c1c3802
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446604"
---
# <a name="monitor-the-operations-status-of-the-remote-access-server-and-its-components"></a>Monitorar o status da operação do servidor de acesso Remoto e seus componentes

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

**Observação:** O Windows Server 2012 reúne o DirectAccess e o RRAS (Serviço de Roteamento e Acesso Remoto) em uma única função de Acesso Remoto.  
  
O console de gerenciamento no servidor de acesso remoto pode ser usado para monitorar seu status de operações.  
  
> [!NOTE]  
> Você deve estar conectado como um membro do grupo Admins. do domínio ou um membro do grupo Administradores em cada computador para concluir a tarefa descrita neste tópico. Se você não puder concluir uma tarefa enquanto estiver conectado com uma conta que seja membro do grupo Administradores, tente realizar a tarefa enquanto você está conectado com uma conta que seja um membro do grupo Admins. do domínio.  
  
#### <a name="to-monitor-the-remote-access-server-operations-status"></a>Para monitorar o status de operações do servidor de acesso remoto  
  
1.  No **Gerenciador do Servidor**, clique em **Ferramentas** e, em seguida, clique em **Gerenciamento de Acesso Remoto**.  
  
2.  Clique em **DASHBOARD** para navegar até **relatórios de acesso remoto** no **Console de gerenciamento de acesso remoto**.  
  
3.  No painel de monitoramento, observe o **Status de operações** lado a lado dentro de **Status do servidor** lado a lado. Esse bloco lista o status de operações do servidor e o status de todos os componentes do servidor.  
  
4.  Clique em **Refresh** sob **tarefas** no painel direito para recarregar o status de operações. O status de operações é atualizado automaticamente a cada cinco minutos, que é o intervalo de atualização padrão. Para alterar o intervalo de atualização padrão, clique em **configurar o intervalo de atualização**.  
  
![Windows PowerShell](../../../media/Monitor-the-operations-status-of-the-Remote-Access-server-and-its-components/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows PowerShell</em>***  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
> [!NOTE]  
> O comando para o status de operações de um cluster é incluído para referência.  
  
```  
PS> Get-RemoteAccessHealth  
PS> Get-RemoteAccessHealth -Cluster  
```  
  


