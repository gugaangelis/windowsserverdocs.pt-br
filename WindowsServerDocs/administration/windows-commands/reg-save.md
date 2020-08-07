---
title: reg save
description: Artigo de referência do comando reg Save, que salva uma cópia de subchaves, entradas e valores especificados do registro em um arquivo especificado.
ms.topic: article
ms.assetid: b326482b-c8af-467d-a20c-0481eeda3d5c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 187811b277ca109ac3f3e1517aeb169bd8baca15
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87884015"
---
# <a name="reg-save"></a>reg save

Salva uma cópia de subchaves, entradas e valores especificados do registro em um arquivo especificado.

## <a name="syntax"></a>Sintaxe

```
reg save <keyname> <filename> [/y]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<keyname>` | Especifica o caminho completo da subchave. Para especificar um computador remoto, inclua o nome do computador (no formato `\\<computername>\` ) como parte do *KeyName*. Omitir `\\<computername>\` faz com que a operação seja padronizada para o computador local. O *KeyName* deve incluir uma chave de raiz válida. As chaves de raiz válidas para o computador local são: **HKLM**, **HKCU**, **HKCR**, **HKU**e **HKCC**. Se um computador remoto for especificado, as chaves de raiz válidas serão: **HKLM** e **HKU**. Se o nome da chave do registro contiver um espaço, coloque o nome da chave entre aspas. |
| `<filename>` | Especifica o nome e o caminho do arquivo criado. Se nenhum caminho for especificado, o caminho atual será usado. |
| /y | Substitui um arquivo existente pelo *nome nome do arquivo sem solicitar* confirmação. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Antes de editar as entradas do registro, você deve salvar a subchave pai usando o comando **reg Save** . Se a edição falhar, você poderá restaurar a subchave original usando a operação **reg Restore** .

- Os valores de retorno para a operação **reg Save** são:

    | Valor | Descrição |
    |--|--|
    | 0 | Êxito |
    | 1 | Falha |

### <a name="examples"></a>Exemplos

Para salvar o hive MyApp na pasta atual como um arquivo chamado AppBkUp. HIV, digite:

```
reg save HKLM\Software\MyCo\MyApp AppBkUp.hiv
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando reg Restore](reg-restore.md)