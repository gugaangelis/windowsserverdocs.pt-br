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
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="edit-file-screen-template-properties"></a>Editar propriedades de modelo de triagem de arquivo

> Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Quando você faz alterações a um modelo de triagem de arquivo, você tem a opção de ampliar essas alterações para triagens de arquivo que foram criadas com o modelo de triagem de arquivo original. Você pode optar por modificar somente as triagens de arquivo que correspondem ao modelo original ou todas as triagens de arquivo que derivadas do modelo original, independentemente de quaisquer modificações que você fez nas triagens de arquivo desde que elas foram criadas. Esse recurso simplifica o processo de atualização das propriedades de suas triagens de arquivo fornecendo um ponto central onde você pode realizar todas as alterações.

> [!Note]
> Se optar por aplicar as alterações em todos as triagens de arquivo que derivam do modelo original, você irá sobrescrever quaisquer propriedades de triagem de arquivo personalizadas que você criou.

## <a name="to-edit-file-screen-template-properties"></a>Para editar propriedades de modelo de triagem de arquivo

1.  Em **Modelos de triagem de arquivo**, selecione o modelo que você deseja modificar.

2.  Clique com o botão direito no modelo de triagem de arquivo e clique em **Editar propriedades de modelo** (ou no painel **Ações**, em **Modelos de triagem de arquivo selecionados**, selecione **Editar propriedades de modelo**.) Isso abre a caixa de diálogo **Propriedades de modelo de triagem de arquivo**.

3.  Se você quiser copiar as propriedades de outro modelo como base para seu modelo, selecione um modelo na lista suspensa **Copiar propriedades do modelo de cota**. Em seguida, clique em **Copiar**.

4.  Execute todas as alterações necessárias. As configurações e opções de notificação são idênticas às que estão disponíveis quando você cria um modelo de triagem de arquivo.

5.  Quando tiver terminado de editar as propriedades de modelo, clique em **OK**. Isso abrirá a caixa de diálogo **Atualizar triagens de arquivo derivadas de modelo**.

6.  Selecione o tipo de atualização que deseja aplicar:

    -   Se você tiver triagens de arquivo que foram modificadas desde que elas foram criadas usando o modelo original e não desejar alterá-las, clique em **Aplicar modelo somente às triagens de arquivo derivadas que correspondem ao modelo original**. Essa opção atualizará somente as triagens de arquivo que não foram editadas desde que elas foram criados com as propriedades do modelo original.
    -   Se você quiser modificar todas as suas triagens de arquivo que foram criadas usando o modelo original, clique em **Aplicar modelo em todas as triagens de arquivo derivadas**.
    -   Se quiser manter as triagens de arquivo existentes inalteradas, clique em **Não aplicar modelo a triagens de arquivo derivadas**.

7.  Clique em **OK**.

## <a name="see-also"></a>Veja também

-   [Gerenciamento de triagem de arquivos](file-screening-management.md)
-   [Criar um modelo de triagem de arquivo](create-file-screen-template.md)


