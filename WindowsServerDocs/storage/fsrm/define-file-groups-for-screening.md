---
title: Definir grupos de arquivos para triagem
description: Este artigo descreve como definir grupos de arquivos para criar um namespace para triagem de arquivo, exceção de triagem de arquivo ou arquivos por relatórios de armazenamento do grupo de arquivos
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: b56d7b0439e3dc6f1a2e0a1c96f761dbb77cb0a0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394393"
---
# <a name="define-file-groups-for-screening"></a>Definir grupos de arquivos para triagem

> Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Um *grupo de arquivo* é usado para definir um namespace de uma triagem de arquivo, exceção de triagem de arquivo ou relatório de armazenamento de **Arquivos por grupo de arquivo**. Ele consiste em um conjunto de padrões de nome de arquivo, que são agrupados pelo seguinte:

-   **Arquivos a serem incluídos**: arquivos que pertencem ao grupo
-   **Arquivos a serem excluídos**: arquivos que não pertencem ao grupo

> [!Note]
> Por conveniência, você pode criar e editar grupos de arquivos enquanto edita as propriedades de triagens de arquivo, exceções de triagem de arquivo, modelos de triagem de arquivo e relatórios de **Arquivos por grupo de arquivos**. As alterações de grupo de arquivo que você cria a partir dessas folhas de propriedades não estão limitadas ao item atual onde você está trabalhando.

## <a name="to-create-a-file-group"></a>Para criar um Grupo de arquivos

1.  Em **Gerenciamento de triagem de arquivo**, clique no nó **Grupos de arquivo**.

2.  No painel **Ações**, clique em **Criar grupo de arquivos**. A caixa de diálogo **Criar propriedades de grupo de arquivos** é aberta.

    (De forma alternativa, enquanto você edita as propriedades de uma triagem de arquivo, exceção de triagem de arquivo, modelo de triagem de arquivo ou o relatório **Arquivos por grupo de arquivo**, em **Manter grupos de arquivo**, clique em **Criar**.)

3.  Na caixa de diálogo **Criar propriedades de grupo de arquivos**, digite um nome para o grupo de arquivos.

4.  Adicione arquivos para incluir e arquivos para excluir:

    -   Para cada conjunto de arquivos que você deseja incluir no grupo de arquivos, na caixa **Arquivos a serem incluídos**, insira um padrão de nome de arquivo e clique em **Adicionar**.
    -   Para cada conjunto de arquivos que você deseja excluir do grupo de arquivos, na caixa **Arquivos a serem excluídos**, insira um padrão de nome de arquivo e clique em **Adicionar**.
        Observe que as regras de curinga padrão se aplicam, por exemplo, **@no__t o-1. exe** seleciona todos os arquivos executáveis.

5.  Clique em **OK**.

## <a name="see-also"></a>Consulte também

-   [Gerenciamento de triagem de arquivos](file-screening-management.md)
-   [Criar uma triagem de arquivo](create-file-screen.md)
-   [Criar uma exceção de triagem de arquivo](create-file-screen-exception.md)
-   [Criar um modelo de triagem de arquivo](create-file-screen-template.md)
-   [Gerenciamento de relatórios de armazenamento](storage-reports-management.md)


