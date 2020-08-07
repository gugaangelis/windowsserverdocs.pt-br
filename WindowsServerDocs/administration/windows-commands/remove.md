---
title: remove
description: Artigo de referência do comando Remove, que remove uma letra de unidade ou um ponto de montagem de um volume.
ms.topic: article
ms.assetid: b0886140-da8b-4231-8cb2-f280874d99c0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 469b3ac1783dfff5228778d11532448bee49346c
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87883834"
---
# <a name="remove"></a>remove

Remove uma letra de unidade ou ponto de montagem do volume com foco. Se o parâmetro All for usado, todas as letras de unidade e pontos de montagem atuais serão removidos. Se nenhuma letra da unidade ou ponto de montagem for especificado, o DiskPart removerá a primeira letra da unidade ou o ponto de montagem que encontrar.

O comando remover também pode ser usado para alterar a letra da unidade associada a uma unidade removível. Não é possível remover as letras da unidade em volumes do sistema, de inicialização ou de paginação. Além disso, não é possível remover a letra da unidade de uma partição OEM, qualquer partição GPT com um GUID não reconhecido ou qualquer uma das partições GPT especiais, não de dados, como a partição do sistema EFI.

> [!NOTE]
> Um volume deve ser selecionado para que o comando **remover** seja executado com sucesso. Use o comando [selecionar volume](select-volume.md) para selecionar um disco e deslocar o foco para ele.

## <a name="syntax"></a>Sintaxe

```
remove [{letter=<drive> | mount=<path> [all]}] [noerr]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| letra =`<drive>` | A letra da unidade a ser removida. |
| Mount =`<path>` | O caminho do ponto de montagem a ser removido. |
| all | Remove todas as letras de unidade e pontos de montagem atuais. |
| NOERR | Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |

### <a name="examples"></a>Exemplos

Para remover o d:\ unidade, digite:

```
remove letter=d
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
