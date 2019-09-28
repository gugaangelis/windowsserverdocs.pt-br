---
title: Migre um namespace baseado em domínio para modo Windows 2008 Server
description: Este artigo descreve como migrar um namespace baseado em domínio para o modo do Windows Server 2008
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 75e366d6b72e9f08ece77558cff253239474777c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386212"
---
# <a name="migrate-a-domain-based-namespace-to-windows-server-2008-mode"></a>Migrar um namespace baseado em domínio para o modo Windows Server 2008

> Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

O modo de namespaces baseado para domínio do Windows Server 2008 inclui suporte para enumeração baseada em acesso e escalabilidade.

## <a name="to-migrate-a-domain-based-namespace-to-windows-server-2008-mode"></a>Para migra um namespace baseado em domínio para modo Windows 2008 Server

Para migrar um namespace baseado em domínio do modo Windows 2000 Server para o modo do Windows Server 2008, você deve exportar o namespace para um arquivo, excluir o namespace, recriá-la no modo do Windows Server 2008 e, em seguida, importe as configurações de namespace. Para fazer isso, use o procedimento a seguir:

1.  Abra uma janela de prompt de comando e digite o seguinte comando para exportar o namespace para um arquivo, em que \\ @ no__t-1*domínio*\\*namespace* é o nome do domínio apropriado, e namespace e *caminho @ no__t-6filename* é o caminho e o nome de arquivo do arquivo para exportação:
     ```
     Dfsutil root export \\domain\namespace path\filename.xml 
     ```
2.  Anote o caminho (\\ @ no__t-1*server* \\*share* ) para cada servidor de namespace. Você deve adicionar manualmente os servidores de namespace ao namespace recriado porque Dfsutil não pode importar servidores de namespace.
3.  No Gerenciamento DFS, clique com botão direito do namespace e, em seguida, clique em **excluir**, ou digite o seguinte comando em um prompt de comando, <br /> em que \\ @ no__t-1*domain*\\*namespace* é o nome do domínio e do namespace apropriados:
     ```
     Dfsutil root remove \\domain\namespace
     ```
4.  Em Gerenciamento DFS, recrie o namespace com o mesmo nome, mas use o modo Windows Server 2008 ou digite o seguinte comando em um prompt de comando, onde <br /> \\ @ no__t-1*server*\\*namespace* é o nome do servidor e do compartilhamento apropriados para a raiz do namespace:
     ```
     Dfsutil root adddom \\server\namespace v2
     ```
5.  Para importar o namespace do arquivo de exportação, digite o seguinte comando em um prompt de comando, onde <br /> \\ @ no__t-1*domínio*\\*namespace* é o nome do domínio e namespace apropriados e o *caminho @ no__t-6filename* é o caminho e o nome do arquivo a ser importado:
     ```
     Dfsutil root import merge path\filename.xml \\domain\namespace
     ```

    > [!NOTE]
    > Para minimizar o tempo necessário para importar um namespace grande, execute o **Dfsutil** comando import localmente em um servidor de namespace de raiz.
6.  Adicione qualquer servidor de namespace restante ao namespace recriado clicando namespace no Gerenciamento DFS e clique em **Adicionar servidor de Namespace**, ou digitando o seguinte comando em um prompt de comando, onde <br /> \\ @ no__t-1*server*\\*share* é o nome do servidor e do compartilhamento apropriados para a raiz do namespace:
     ```
     Dfsutil target add \\server\share 
     ```

    > [!NOTE]
    > Você pode adicionar servidores de namespace antes de importar o namespace, mas fazendo isso faz com que os servidores de namespace de forma adicional baixar os metadados para o namespace em vez de baixar imediatamente o namespace inteiro após ser adicionado como um servidor de namespace.

## <a name="see-also"></a>Consulte também
-   [Implantar namespaces do DFS](deploying-dfs-namespaces.md)
-   [Escolher um tipo de namespace](choose-a-namespace-type.md)