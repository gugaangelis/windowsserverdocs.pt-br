---
title: Instalar o recurso BranchCache
description: Este tópico faz parte do BranchCache implantação guia para Windows Server 2016, que demonstra como implantar BranchCache nos modos de cache hospedado e distribuídos para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 4f31dc61-2dbe-4c7e-b3f9-85ae49a45049
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a69848536b56521da9b5ef07689aba7f8690e888
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="install-the-branchcache-feature"></a>Instalar o recurso BranchCache

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este procedimento para instalar o recurso BranchCache e iniciar o serviço BranchCache em um computador executando o Windows Server&reg; 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
A associação ao grupo **administradores** ou equivalente é o requisito mínimo para executar este procedimento.  
  
Antes de executar este procedimento, é recomendável que você instale e configure seu aplicativo com base em BITS ou um servidor Web.  
  
> [!NOTE]  
> Para executar este procedimento usando o Windows PowerShell, execute o Windows PowerShell como administrador, digite os seguintes comandos no prompt do Windows PowerShell e pressione ENTER.  
>   
> `Install-WindowsFeature BranchCache`  
>   
> `Restart-Computer`  
  
### <a name="to-install-and-enable-the-branchcache-feature"></a>Para instalar e habilitar o recurso BranchCache  
  
1.  No Gerenciador do servidor, clique em **gerenciar**e clique em **adicionar funções e recursos**. Abre o Assistente de adição de funções e recursos. Clique em **próxima**.  
  
2.  Em **selecionar o tipo de instalação**, certifique-se de que **instalação baseada em função ou recurso baseado** está selecionado e clique em **próxima**.  
  
3.  Em **servidor de destino Select**, certifique-se de que o servidor correto está selecionado e clique em **próxima**.  
  
4.  Em **selecionar funções de servidor**, clique em **próxima**.  
  
5.  Em **Selecione recursos**, clique em **BranchCache**e clique em **próxima**.  
  
6.  Em **confirmar seleções de instalação**, clique em **instalar**. Em **progresso da instalação**, a instalação do recurso BranchCache receita. Quando a instalação for concluída, clique em **fechar**.  
  
Depois de instalar o recurso BranchCache, o serviço BranchCache - também chamado de PeerDistSvc - estiver ativado, e o tipo de tela inicial é automático.  
  


