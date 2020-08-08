---
title: Instalar o recurso BranchCache
description: Este tópico faz parte do guia de implantação do BranchCache para o Windows Server 2016, que demonstra como implantar o BranchCache em modos de cache distribuídos e hospedados para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.topic: get-started-article
ms.assetid: 4f31dc61-2dbe-4c7e-b3f9-85ae49a45049
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 64fa6dea8d93d0e152b0cf795251aec0d9f52bc5
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87952618"
---
# <a name="install-the-branchcache-feature"></a>Instalar o recurso BranchCache

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este procedimento para instalar o recurso do BranchCache e iniciar o serviço do BranchCache em um computador que executa o Windows Server &reg; 2016, o Windows server 2012 R2 ou o Windows server 2012.

A associação a **Administradores** ou equivalente é o requisito mínimo para a execução deste procedimento.

Antes de executar esse procedimento, é recomendável que você instale e configure seu aplicativo baseado em BITS ou servidor Web.

> [!NOTE]
> Para executar esse procedimento usando o Windows PowerShell, execute o Windows PowerShell como administrador, digite os seguintes comandos no prompt do Windows PowerShell e pressione ENTER.
>
> `Install-WindowsFeature BranchCache`
>
> `Restart-Computer`

### <a name="to-install-and-enable-the-branchcache-feature"></a>Para instalar e habilitar o recurso BranchCache

1.  No Gerenciador do Servidor, clique em **Gerenciar** e depois em **Adicionar Funções e Recursos**. O assistente Adicionar funções e recursos é aberto. Clique em **Próximo**.

2.  Em **Selecionar tipo de instalação**, verifique se a instalação baseada em **função ou em recurso** está selecionada e clique em **Avançar**.

3.  Em **selecionar servidor de destino**, verifique se o servidor correto está selecionado e clique em **Avançar**.

4.  Em **Selecionar funções de servidor**, clique em **Avançar**.

5.  Em **selecionar recursos**, clique em **BranchCache**e em **Avançar**.

6.  Em **Confirmar seleções de instalação**, clique em **Instalar**. Em **andamento da instalação**, a instalação do recurso do BranchCache continua. Quando a instalação for concluída, clique em **fechar**.

Depois de instalar o recurso do BranchCache, o serviço do BranchCache também chamado de PeerDistSvc-está habilitado e o tipo de inicialização é automático.



