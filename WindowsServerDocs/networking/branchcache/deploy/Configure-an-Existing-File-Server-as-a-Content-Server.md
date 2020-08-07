---
title: Configurar um servidor de arquivos existente como um servidor de conteúdo
description: Este tópico faz parte do guia de implantação do BranchCache para o Windows Server 2016, que demonstra como implantar o BranchCache em modos de cache distribuídos e hospedados para otimizar o uso de largura de banda WAN em filiais
manager: brianlic
ms.topic: get-started-article
ms.assetid: bdac7d2a-25b4-4f61-bed1-b290700c18f3
ms.author: lizross
author: eross-msft
ms.openlocfilehash: ddb6a9d8ad14146d6f4aca27171649fde3b227eb
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87971913"
---
# <a name="configure-an-existing-file-server-as-a-content-server"></a>Configurar um servidor de arquivos existente como um servidor de conteúdo

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este procedimento para instalar o **BranchCache para** o serviço de função de arquivos de rede da função de servidor de serviços de arquivo em um computador que executa o Windows Server 2016.

> [!IMPORTANT]
> Se a função de servidor de Serviços de Arquivo ainda não estiver instalada, não execute este procedimento. Em vez disso, consulte [instalar um novo servidor de arquivos como um servidor de conteúdo](../../branchcache/deploy/Install-a-New-File-Server-as-a-Content-Server.md).

A associação a **Administradores** ou equivalente é o requisito mínimo para a execução deste procedimento.

> [!NOTE]
> Para executar esse procedimento usando o Windows PowerShell, execute o Windows PowerShell como administrador, digite os seguintes comandos no prompt do Windows PowerShell e pressione ENTER.
>
> `Install-WindowsFeature FS-BranchCache -IncludeManagementTools`
>
> Para instalar o serviço de função eliminação de duplicação de dados, digite o comando a seguir e pressione ENTER.
>
> `Install-WindowsFeature FS-Data-Deduplication -IncludeManagementTools`

### <a name="to-install-the-branchcache-for-network-files-role-service"></a>Para instalar o BranchCache para o serviço de função de arquivos de rede

1.  No Gerenciador do Servidor, clique em **Gerenciar** e depois em **Adicionar Funções e Recursos**. O assistente Adicionar funções e recursos é aberto. Clique em **Próximo**.

2.  Em **Selecionar tipo de instalação**, verifique se a instalação baseada em **função ou em recurso** está selecionada e clique em **Avançar**.

3.  Em **selecionar servidor de destino**, verifique se o servidor correto está selecionado e clique em **Avançar**.

4.  Em **selecionar funções de servidor**, em **funções**, observe que a função **serviços de arquivo e armazenamento** já está instalada; Clique na seta à esquerda do nome da função para expandir a seleção de serviços de função e, em seguida, clique na seta à esquerda de **serviços de arquivo e iSCSI**.

5.  Marque a caixa de seleção do **BranchCache para arquivos de rede**.

    > [!TIP]
    > Se você ainda não tiver feito isso, é recomendável marcar também a caixa de seleção para **eliminação de duplicação de dados**.

    Clique em **Próximo**.

6.  Em **selecionar recursos**, clique em **Avançar**.

7.  Em **confirmar seleções de instalação**, examine suas seleções e clique em **instalar**. O painel **progresso da instalação** é exibido durante a instalação. Quando a instalação for concluída, clique em **fechar**.



