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
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 64471ba81842fb91a7f6ef765e171949294102fa
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314188"
---
# <a name="monitor-the-operations-status-of-the-remote-access-server-and-its-components"></a>Monitorar o status da operação do servidor de acesso Remoto e seus componentes

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

**Observação:** o Windows Server 2012 reúne o DirectAccess e o RRAS (Serviço de Roteamento e Acesso Remoto) em uma única função de Acesso Remoto.  
  
O console de gerenciamento no servidor de acesso remoto pode ser usado para monitorar seu status de operações.  
  
> [!NOTE]  
> Você deve estar conectado como um membro do grupo Admins. do domínio ou um membro do grupo Administradores em cada computador para concluir a tarefa descrita neste tópico. Se você não puder concluir uma tarefa enquanto estiver conectado com uma conta que seja membro do grupo Administradores, tente executar a tarefa enquanto estiver conectado com uma conta que seja membro do grupo Admins. do domínio.  
  
#### <a name="to-monitor-the-remote-access-server-operations-status"></a>Para monitorar o status de operações do servidor de acesso remoto  
  
1.  No **Gerenciador do Servidor**, clique em **Ferramentas** e, em seguida, clique em **Gerenciamento de Acesso Remoto**.  
  
2.  Clique em **painel** para navegar até **relatórios de acesso remoto** no console de gerenciamento de **acesso remoto**.  
  
3.  No painel Monitoramento, observe o bloco **status de operações** no bloco **status do servidor** . Esse bloco lista o status de operações do servidor e o status de todos os componentes do servidor.  
  
4.  Clique em **Atualizar** em **tarefas** no painel direito para recarregar o status das operações. O status de operações é atualizado automaticamente a cada cinco minutos, que é o intervalo de atualização padrão. Para alterar o intervalo de atualização padrão, clique em **Configurar intervalo de atualização**.  
  
![](../../../media/Monitor-the-operations-status-of-the-Remote-Access-server-and-its-components/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows</em> PowerShell***  
  
O cmdlet ou cmdlets do Windows PowerShell a seguir executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, embora eles apareçam com quebra de linha em várias linhas aqui devido a restrições de formatação.  
  
> [!NOTE]  
> O comando para o status de operações de um cluster é incluído para referência.  
  
```  
PS> Get-RemoteAccessHealth  
PS> Get-RemoteAccessHealth -Cluster  
```  
  


