---
title: Instalar o recurso BranchCache
description: Este tópico faz parte do BranchCache implantação guia para o Windows Server 2016, que demonstra como implantar o BranchCache nos modos de cache hospedado e distribuído para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 4f31dc61-2dbe-4c7e-b3f9-85ae49a45049
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8b4aecd9e9355a6c2d5ac485ac77c76428fe295f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872187"
---
# <a name="install-the-branchcache-feature"></a>Instalar o recurso BranchCache

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este procedimento para instalar o recurso BranchCache e iniciar o serviço BranchCache em um computador executando o Windows Server&reg; 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
A associação a **Administradores** ou equivalente é o requisito mínimo para a execução deste procedimento.  
  
Antes de executar este procedimento, é recomendável que você instale e configure seu servidor de Web ou aplicativo baseado em BITS.  
  
> [!NOTE]  
> Para executar esse procedimento usando o Windows PowerShell, execute o Windows PowerShell como administrador, digite os seguintes comandos no prompt do Windows PowerShell e pressione ENTER.  
>   
> `Install-WindowsFeature BranchCache`  
>   
> `Restart-Computer`  
  
### <a name="to-install-and-enable-the-branchcache-feature"></a>Para instalar e habilitar o recurso BranchCache  
  
1.  No Gerenciador do Servidor, clique em **Gerenciar**e depois em **Adicionar Funções e Recursos**. Abre o assistente Adicionar funções e recursos. Clique em **Avançar**.  
  
2.  Na **Selecionar tipo de instalação**, certifique-se de que **instalação baseada em função ou recurso** está selecionado e, em seguida, clique em **próxima**.  
  
3.  Na **Selecionar servidor de destino**, certifique-se de que o servidor correto está selecionado e, em seguida, clique em **próxima**.  
  
4.  Em **Selecionar funções de servidor**, clique em **Avançar**.  
  
5.  Na **selecionar recursos**, clique em **BranchCache**e, em seguida, clique em **próxima**.  
  
6.  Em **Confirmar seleções de instalação**, clique em **Instalar**. Na **progresso da instalação**, continua a instalação do recurso BranchCache. Quando a instalação for concluída, clique em **fechar**.  
  
Depois de instalar o recurso BranchCache, o serviço BranchCache - também chamado de PeerDistSvc - está habilitado e o tipo de inicialização é automático.  
  


