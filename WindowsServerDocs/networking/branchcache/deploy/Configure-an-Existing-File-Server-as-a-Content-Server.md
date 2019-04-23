---
title: Configurar um servidor de arquivos existente como um servidor de conteúdo
description: Este tópico faz parte do BranchCache implantação guia para o Windows Server 2016, que demonstra como implantar o BranchCache nos modos de cache hospedado e distribuído para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-bc
ms.topic: get-started-article
ms.assetid: bdac7d2a-25b4-4f61-bed1-b290700c18f3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d031e8aa853849c322692552ca9107838cebb5e5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866907"
---
# <a name="configure-an-existing-file-server-as-a-content-server"></a>Configurar um servidor de arquivos existente como um servidor de conteúdo

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este procedimento para instalar o **BranchCache para arquivos de rede** serviço de função da função de servidor Serviços de arquivo em um computador executando o Windows Server 2016.  
  
> [!IMPORTANT]  
> Se a função de servidor de Serviços de Arquivo ainda não estiver instalada, não execute este procedimento. Em vez disso, consulte [instalar um novo servidor de arquivos como um servidor de conteúdo](../../branchcache/deploy/Install-a-New-File-Server-as-a-Content-Server.md).  
  
A associação a **Administradores** ou equivalente é o requisito mínimo para a execução deste procedimento.  
  
> [!NOTE]  
> Para executar esse procedimento usando o Windows PowerShell, execute o Windows PowerShell como administrador, digite os seguintes comandos no prompt do Windows PowerShell e pressione ENTER.  
>   
> `Install-WindowsFeature FS-BranchCache -IncludeManagementTools`  
>   
> Para instalar o serviço de função de eliminação de duplicação de dados, digite o seguinte comando e pressione ENTER.  
>   
> `Install-WindowsFeature FS-Data-Deduplication -IncludeManagementTools`  
  
### <a name="to-install-the-branchcache-for-network-files-role-service"></a>Para instalar o BranchCache para arquivos de rede  
  
1.  No Gerenciador do Servidor, clique em **Gerenciar**e depois em **Adicionar Funções e Recursos**. Abre o assistente Adicionar funções e recursos. Clique em **Avançar**.  
  
2.  Na **Selecionar tipo de instalação**, certifique-se de que **instalação baseada em função ou recurso** está selecionado e, em seguida, clique em **próxima**.  
  
3.  Na **Selecionar servidor de destino**, certifique-se de que o servidor correto está selecionado e, em seguida, clique em **próxima**.  
  
4.  Na **selecionar funções de servidor**, na **funções**, observe que o **serviços de arquivo e armazenamento** função já está instalada e clique na seta à esquerda do nome da função para expandir o seleção de serviços de função e, em seguida, clique na seta à esquerda do **arquivo e iSCSI serviços**.  
  
5.  Marque a caixa de seleção **BranchCache para arquivos de rede**.  
  
    > [!TIP]  
    > Se você ainda não fez isso, é recomendável que você também pode selecionar a caixa de seleção **eliminação de duplicação de dados**.  
  
    Clique em **Avançar**.  
  
6.  Na **selecionar recursos**, clique em **próxima**.  
  
7.  Na **confirmar seleções de instalação**, examine suas seleções e, em seguida, clique em **instalar**. O **progresso da instalação** painel é exibido durante a instalação. Quando a instalação for concluída, clique em **fechar**.  
  


