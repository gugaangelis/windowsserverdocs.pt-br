---
title: Editar propriedades de modelo de triagem de arquivo
description: Este artigo descreve como editar propriedades de modelo de triagem de arquivo
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 31ca46707a32d23a5dd9606c57bcaec5d6e53a80
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59846887"
---
# <a name="edit-file-screen-template-properties"></a>Editar propriedades de modelo de triagem de arquivo

> Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Quando você faz alterações a um modelo de triagem de arquivo, você tem a opção de ampliar essas alterações para triagens de arquivo que foram criadas com o modelo de triagem de arquivo original. Você pode optar por modificar somente as triagens de arquivo que correspondem ao modelo original ou todas as triagens de arquivo que derivadas do modelo original, independentemente de quaisquer modificações que você fez nas triagens de arquivo desde que elas foram criadas. Esse recurso simplifica o processo de atualização das propriedades de suas triagens de arquivo fornecendo um ponto central onde você pode realizar todas as alterações.

> [!Note]
> Se optar por aplicar as alterações em todos as triagens de arquivo que derivam do modelo original, você irá sobrescrever quaisquer propriedades de triagem de arquivo personalizadas que você criou.

## <a name="to-edit-file-screen-template-properties"></a>Para editar propriedades de modelo de triagem de arquivo

1.  Em **Modelos de triagem de arquivo**, selecione o modelo que você deseja modificar.

2.  O modelo de triagem de arquivo com o botão direito e clique em **editar propriedades do modelo** (ou o **ações** painel, em **modelos de tela de arquivo selecionado**, selecione  **Editar propriedades de modelo**.) Isso abre o **propriedades do modelo de triagem de arquivo** caixa de diálogo.

3.  Se você quiser copiar as propriedades de outro modelo como base para seu modelo, selecione um modelo na lista suspensa **Copiar propriedades do modelo de cota**. Em seguida, clique em **Copiar**.

4.  Execute todas as alterações necessárias. As configurações e opções de notificação são idênticas às que estão disponíveis quando você cria um modelo de triagem de arquivo.

5.  Quando tiver terminado de editar as propriedades de modelo, clique em **OK**. Isso abrirá a caixa de diálogo **Atualizar triagens de arquivo derivadas de modelo**.

6.  Selecione o tipo de atualização que deseja aplicar:

    -   Se você tiver triagens de arquivo que foram modificadas desde que elas foram criadas usando o modelo original e não desejar alterá-las, clique em **Aplicar modelo somente às triagens de arquivo derivadas que correspondem ao modelo original**. Essa opção atualizará somente as triagens de arquivo que não foram editadas desde que elas foram criados com as propriedades do modelo original.
    -   Se você quiser modificar todas as suas triagens de arquivo que foram criadas usando o modelo original, clique em **Aplicar modelo em todas as triagens de arquivo derivadas**.
    -   Se quiser manter as triagens de arquivo existentes inalteradas, clique em **Não aplicar modelo a triagens de arquivo derivadas**.

7.  Clique em **OK**.

## <a name="see-also"></a>Consulte também

-   [Gerenciamento de triagem de arquivo](file-screening-management.md)
-   [Criar um modelo de tela de arquivo](create-file-screen-template.md)


