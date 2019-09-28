---
title: Criar uma propriedade de classificação
description: Este artigo descreve as propriedades de classificação, usadas para atribuir valores a arquivos em uma pasta especificada ou volume.
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: d330f896c71cced8e97701af2c1008b3531e065d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394216"
---
# <a name="create-a-classification-property"></a>Criar uma propriedade de classificação

> Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Propriedades de classificação são usadas para atribuir valores a arquivos em uma pasta especificada ou volume. Existem muitos tipos de propriedade que você pode escolher, dependendo de suas necessidades. A tabela a seguir define os tipos de propriedade disponíveis.

|Propriedade | Descrição |
| --- | --- |
| Sim/Não | Uma propriedade booliana que pode ser **Sim** ou **Não**. Ao combinar vários valores durante a classificação ou no conteúdo do arquivo, um valor **Não** será substituído por um valor **Sim**. |
| Data e hora | Uma propriedade simples de data/hora. Ao combinar vários valores durante a classificação ou no conteúdo do arquivo, os valores conflitantes evitarão nova classificação. |
| Number | Uma propriedade de número simples. Ao combinar vários valores durante a classificação ou no conteúdo do arquivo, os valores conflitantes evitarão nova classificação. |
| Lista ordenada | Uma lista de valores fixos. Apenas um valor pode ser atribuído a uma propriedade por vez. Ao combinar vários valores durante a classificação ou no conteúdo do arquivo, o maior valor na lista será usado. |
| String | Uma propriedade de cadeia simples. Ao combinar vários valores durante a classificação ou no conteúdo do arquivo, os valores conflitantes evitarão nova classificação. |
| Múltipla escolha | Uma lista de valores que podem ser atribuídos a uma propriedade. Mais que um valor pode ser atribuído a uma propriedade por vez. Ao combinar vários valores durante a classificação ou no conteúdo do arquivo, cada valor na lista será usado. |
| Várias cadeias | Uma lista de cadeias que podem ser atribuídas a uma propriedade. Mais que um valor pode ser atribuído a uma propriedade por vez. Ao combinar vários valores durante a classificação ou no conteúdo do arquivo, cada valor na lista será usado. |

<br />

O procedimento a seguir orienta você pelo processo de criação de uma propriedade de classificação.

## <a name="to-create-a-classification-property"></a>Para criar uma propriedade de classificação

1.  Em **Gerenciamento de Classificação**, clique no nó **Propriedades de Classificação**.

2.  Clique com botão direito em **Propriedades de classificação** e clique em **Criar propriedade** (ou em **Criar propriedade** no painel **Ações**). Isso abre a caixa de diálogo **Definições de propriedade de classificação**.

3.  Na caixa de texto **Nome da propriedade**, digite um nome para a propriedade.

4.  Na caixa de texto **Descrição**, adicione uma descrição opcional para a propriedade.

5.  No menu suspenso **Tipo de Propriedade**, selecione um tipo de propriedade na lista.

6.  Clique em **OK**.

## <a name="see-also"></a>Consulte também

-   [Criar uma regra de classificação automática](create-automatic-classification-rule.md)
-   [Gerenciamento de classificação](classification-management.md)