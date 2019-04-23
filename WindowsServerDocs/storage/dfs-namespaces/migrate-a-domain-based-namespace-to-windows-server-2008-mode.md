---
title: Migre um namespace baseado em domínio para modo Windows 2008 Server
description: Este artigo descreve como migrar um namespace baseado em domínio para o modo do Windows Server 2008
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e2624e53d306dd198525b0abe8adf96fe6060bc1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847857"
---
# <a name="migrate-a-domain-based-namespace-to-windows-server-2008-mode"></a>Migrar um namespace baseado em domínio para o modo Windows Server 2008

> Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

O modo de namespaces baseado para domínio do Windows Server 2008 inclui suporte para enumeração baseada em acesso e escalabilidade.

## <a name="to-migrate-a-domain-based-namespace-to-windows-server-2008-mode"></a>Para migra um namespace baseado em domínio para modo Windows 2008 Server

Para migrar um namespace baseado em domínio do modo Windows 2000 Server para o modo do Windows Server 2008, você deve exportar o namespace para um arquivo, excluir o namespace, recriá-la no modo do Windows Server 2008 e, em seguida, importe as configurações de namespace. Para fazer isso, use o procedimento a seguir:

1.  Abra uma janela de Prompt de comando e digite o seguinte comando para exportar o namespace para um arquivo, onde \\ \\ *domínio*\\*namespace* é o nome das domínio apropriado e o namespace e *caminho\\filename* é o nome de arquivo e caminho do arquivo de exportação:
     ```
     Dfsutil root export \\domain\namespace path\filename.xml 
     ```
2.  Anote o caminho ( \\ \\ *server* \\ *compartilhar* ) para cada servidor de namespace. Você deve adicionar manualmente os servidores de namespace ao namespace recriado porque Dfsutil não pode importar servidores de namespace.
3.  No Gerenciamento DFS, clique com botão direito do namespace e, em seguida, clique em **excluir**, ou digite o seguinte comando em um prompt de comando, <br /> em que \\ \\ *domínio*\\*namespace* é o nome do domínio apropriado e do namespace:
     ```
     Dfsutil root remove \\domain\namespace
     ```
4.  Em Gerenciamento DFS, recrie o namespace com o mesmo nome, mas use o modo Windows Server 2008 ou digite o seguinte comando em um prompt de comando, onde <br /> \\\\*servidor*\\*namespace* é o nome do servidor apropriado e do compartilhamento para a raiz do namespace:
     ```
     Dfsutil root adddom \\server\namespace v2
     ```
5.  Para importar o namespace do arquivo de exportação, digite o seguinte comando em um prompt de comando, onde <br /> \\\\*domínio*\\*namespace* é o nome do domínio apropriado e do namespace e *caminho\\filename* é o nome de arquivo e caminho do arquivo a importar:
     ```
     Dfsutil root import merge path\filename.xml \\domain\namespace
     ```

    > [!NOTE]
    > Para minimizar o tempo necessário para importar um namespace grande, execute o **Dfsutil** comando import localmente em um servidor de namespace de raiz.
6.  Adicione qualquer servidor de namespace restante ao namespace recriado clicando namespace no Gerenciamento DFS e clique em **Adicionar servidor de Namespace**, ou digitando o seguinte comando em um prompt de comando, onde <br /> \\\\*servidor*\\*compartilhar* é o nome do servidor apropriado e do compartilhamento para a raiz do namespace:
     ```
     Dfsutil target add \\server\share 
     ```

    > [!NOTE]
    > Você pode adicionar servidores de namespace antes de importar o namespace, mas fazendo isso faz com que os servidores de namespace de forma adicional baixar os metadados para o namespace em vez de baixar imediatamente o namespace inteiro após ser adicionado como um servidor de namespace.

## <a name="see-also"></a>Consulte também
-   [Implantando os Namespaces do DFS](deploying-dfs-namespaces.md)
-   [Escolha um tipo de Namespace](choose-a-namespace-type.md)