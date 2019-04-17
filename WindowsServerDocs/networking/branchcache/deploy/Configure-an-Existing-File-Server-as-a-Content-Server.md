---
title: Configurar um servidor de arquivos existente como um servidor de conteúdo
description: Este tópico faz parte do BranchCache implantação guia para Windows Server 2016, que demonstra como implantar BranchCache nos modos de cache hospedado e distribuídos para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: bdac7d2a-25b4-4f61-bed1-b290700c18f3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: da4c38b6209dc10704aee8c79344ee2da98da272
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-an-existing-file-server-as-a-content-server"></a>Configurar um servidor de arquivos existente como um servidor de conteúdo

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este procedimento para instalar o **BranchCache para arquivos de rede** serviço de função da função de servidor Serviços de arquivo em um computador executando o Windows Server 2016.  
  
> [!IMPORTANT]  
> Se a função de servidor de serviços de arquivos já não estiver instalada, não siga este procedimento. Em vez disso, consulte [instalar um novo servidor de arquivos como um servidor de conteúdo](../../branchcache/deploy/Install-a-New-File-Server-as-a-Content-Server.md).  
  
A associação ao grupo **administradores**, ou equivalente é o requisito mínimo para executar este procedimento.  
  
> [!NOTE]  
> Para executar este procedimento usando o Windows PowerShell, execute o Windows PowerShell como administrador, digite os seguintes comandos no prompt do Windows PowerShell e pressione ENTER.  
>   
> `Install-WindowsFeature FS-BranchCache -IncludeManagementTools`  
>   
> Para instalar o serviço de função de duplicação de dados, digite o seguinte comando e pressione ENTER.  
>   
> `Install-WindowsFeature FS-Data-Deduplication -IncludeManagementTools`  
  
### <a name="to-install-the-branchcache-for-network-files-role-service"></a>Para instalar o BranchCache para o serviço de função de arquivos de rede  
  
1.  No Gerenciador do servidor, clique em **gerenciar**e clique em **adicionar funções e recursos**. Abre o Assistente de adição de funções e recursos. Clique em **próxima**.  
  
2.  Em **selecionar o tipo de instalação**, certifique-se de que **instalação baseada em função ou recurso baseado** está selecionado e clique em **próxima**.  
  
3.  Em **servidor de destino Select**, certifique-se de que o servidor correto está selecionado e clique em **próxima**.  
  
4.  Em **selecionar funções de servidor**, na **funções**, observe que o **serviços de arquivo e armazenamento** função já está instalada; Clique na seta para a esquerda do nome da função para expandir a seleção dos serviços de função e, em seguida, clique na seta à esquerda do **arquivo e iSCSI Services**.  
  
5.  Marque a caixa de seleção para **BranchCache para arquivos de rede**.  
  
    > [!TIP]  
    > Se você não tiver feito isso, é recomendável que você também pode selecionar a caixa de seleção para **duplicação de dados**.  
  
    Clique em **próxima**.  
  
6.  Em **Selecione recursos**, clique em **próxima**.  
  
7.  Em **confirmar seleções de instalação**, revise suas seleções e clique em **instalar**. O **progresso da instalação** painel é exibido durante a instalação. Quando a instalação for concluída, clique em **fechar**.  
  


