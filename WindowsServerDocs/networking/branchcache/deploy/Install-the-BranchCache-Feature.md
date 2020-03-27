---
title: Instalar o recurso BranchCache
description: Este tópico faz parte do guia de implantação do BranchCache para o Windows Server 2016, que demonstra como implantar o BranchCache em modos de cache distribuídos e hospedados para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: 4f31dc61-2dbe-4c7e-b3f9-85ae49a45049
ms.author: lizross
author: eross-msft
ms.openlocfilehash: a895e65686a6ccfb1453bc7cc7ddfcab5720a206
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80319210"
---
# <a name="install-the-branchcache-feature"></a>Instalar o recurso BranchCache

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este procedimento para instalar o recurso do BranchCache e iniciar o serviço do BranchCache em um computador que executa o Windows Server&reg; 2016, Windows Server 2012 R2 ou Windows Server 2012.  
  
A associação a **Administradores** ou equivalente é o requisito mínimo para a execução deste procedimento.  
  
Antes de executar esse procedimento, é recomendável que você instale e configure seu aplicativo baseado em BITS ou servidor Web.  
  
> [!NOTE]  
> Para executar esse procedimento usando o Windows PowerShell, execute o Windows PowerShell como administrador, digite os seguintes comandos no prompt do Windows PowerShell e pressione ENTER.  
>   
> `Install-WindowsFeature BranchCache`  
>   
> `Restart-Computer`  
  
### <a name="to-install-and-enable-the-branchcache-feature"></a>Para instalar e habilitar o recurso BranchCache  
  
1.  No Gerenciador do Servidor, clique em **Gerenciar**e depois em **Adicionar Funções e Recursos**. O assistente Adicionar funções e recursos é aberto. Clique em **Avançar**.  
  
2.  Em **Selecionar tipo de instalação**, verifique se a instalação baseada em **função ou em recurso** está selecionada e clique em **Avançar**.  
  
3.  Em **selecionar servidor de destino**, verifique se o servidor correto está selecionado e clique em **Avançar**.  
  
4.  Em **Selecionar funções de servidor**, clique em **Avançar**.  
  
5.  Em **selecionar recursos**, clique em **BranchCache**e em **Avançar**.  
  
6.  Em **Confirmar seleções de instalação**, clique em **Instalar**. Em **andamento da instalação**, a instalação do recurso do BranchCache continua. Quando a instalação for concluída, clique em **fechar**.  
  
Depois de instalar o recurso do BranchCache, o serviço do BranchCache também chamado de PeerDistSvc-está habilitado e o tipo de inicialização é automático.  
  


