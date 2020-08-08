---
title: Criar uma exceção de triagem de arquivo
description: Este artigo descreve como criar uma exceção de triagem de arquivo
ms.date: 7/7/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 9d8f0e4a8bc89312b846421c64b14518a9246aaa
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87942135"
---
# <a name="create-a-file-screen-exception"></a>Criar uma exceção de triagem de arquivo

> Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Ocasionalmente, você precisará permitir exceções à triagem de arquivo. Por exemplo, você talvez queira bloquear arquivos de vídeo de um servidor de arquivos, mas precisa permitir que seu grupo de treinamento salve os arquivos de vídeo para o treinamento baseado no computador. Para permitir arquivos que outros arquivos estão bloqueando, crie uma *exceção de triagem de arquivo*.

Uma exceção de triagem de arquivo é um tipo especial de triagem de arquivo que substitui qualquer triagem de arquivo que se aplique a uma pasta e todas as suas subpastas em um caminho de exceção designado. Ou seja, ele cria uma exceção para quaisquer regras derivadas de uma pasta pai.

> [!Note]
> Você não pode criar uma exceção de triagem de arquivo em uma pasta pai onde uma triagem de arquivo já estiver definida. Você deve atribuir a exceção a uma subpasta ou fazer alterações na triagem de arquivo existente.

<br />
Atribuir grupos de arquivos para determinar quais tipos de arquivo serão permitidos na exceção da triagem de arquivo.

## <a name="to-create-a-file-screen-exception"></a>Para criar uma exceção de triagem de arquivo

1.  Em **Gerenciamento de triagem de arquivo**, clique no nó **Triagens de arquivo**.

2.  Clique com botão direito em **Triagens de arquivo**e clique em **Criar exceção da triagem de arquivo** (ou selecione **Criar exceção da triagem de arquivo** no painel **Ações**). A caixa de diálogo **Criar Exceção da Triagem de Arquivo** é aberta.

3.  Na caixa de texto **Caminho da exceção**, digite ou selecione o caminho onde a exceção será aplicada. A exceção será aplicada para a pasta selecionada e todas as suas subpastas.

4.  Para especificar quais arquivos devem ser excluídos da triagem de arquivo:

    -   Em **Grupos de arquivos**, selecione cada grupo de arquivos que você deseja excluir da triagem de arquivo. (Para marcar a caixa de seleção do grupo de arquivos, clique duas vezes no rótulo do grupo de arquivos.)
    -   Se você quiser exibir os tipos de arquivo que um grupo de arquivos inclui e exclui, clique no rótulo do grupo de arquivos e clique em **Editar**.
    -   Para criar um novo grupo de arquivos, clique em **Criar**.

5.  Clique em **OK**.

## <a name="additional-references"></a>Referências adicionais

-   [Gerenciamento de triagem de arquivos](file-screening-management.md)
-   [Definir grupos de arquivos para triagem](define-file-groups-for-screening.md)


