---
title: reg export
description: Artigo de referência do comando reg export, que copia as subchaves, entradas e valores especificados do computador local em um arquivo para transferência para outros servidores.
ms.topic: reference
ms.assetid: 0ad9526f-1e29-4fa5-9d2d-feaa92f12d7c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a7bf8abe5dd97463202a024da90523a52020a986
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89627055"
---
# <a name="reg-export"></a>reg export

Copia as subchaves, entradas e valores especificados do computador local em um arquivo para transferência para outros servidores.

## <a name="syntax"></a>Sintaxe

```
reg export <keyname> <filename> [/y]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<keyname>` | Especifica o caminho completo da subchave. A operação de exportação funciona apenas com o computador local. O *KeyName* deve incluir uma chave de raiz válida. As chaves de raiz válidas para o computador local são: **HKLM**, **HKCU**, **HKCR**, **HKU**e **HKCC**. Se o nome da chave do registro contiver um espaço, coloque o nome da chave entre aspas. |
| `<filename>` | Especifica o nome e o caminho do arquivo a ser criado durante a operação. O arquivo deve ter uma extensão. reg. |
| /y | Substitui qualquer arquivo existente pelo *nome nome do arquivo sem solicitar* confirmação. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Os valores de retorno para a operação de **exportação de reg** são:

    | Valor | Descrição |
    |--|--|
    | 0 | Êxito |
    | 1 | Falha |

### <a name="examples"></a>Exemplos

Para exportar o conteúdo de todas as subchaves e valores da chave MyApp para o arquivo AppBkUp. reg, digite:

```
reg export HKLM\Software\MyCo\MyApp AppBkUp.reg
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
