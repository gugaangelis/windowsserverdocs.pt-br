---
title: reg restore
description: Artigo de referência para o comando reg Restore, que grava subchaves e entradas salvas de volta no registro.
ms.topic: reference
ms.assetid: a51f1c0c-969b-4b76-930a-c8bb14dea26e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d5c0e51ed3b16c2655647919f360a56b2aee6344
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639856"
---
# <a name="reg-restore"></a>reg restore

Grava subchaves e entradas salvas de volta no registro.

## <a name="syntax"></a>Sintaxe

```
reg restore <keyname> <filename>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<keyname>` | Especifica o caminho completo da subchave a ser restaurada. A operação de restauração funciona apenas com o computador local. O *KeyName* deve incluir uma chave de raiz válida. As chaves de raiz válidas para o computador local são: **HKLM**, **HKCU**, **HKCR**, **HKU**e **HKCC**. Se o nome da chave do registro contiver um espaço, coloque o nome da chave entre aspas. |
| `<filename>` | Especifica o nome e o caminho do arquivo com o conteúdo a ser gravado no registro. Esse arquivo deve ser criado com antecedência usando o comando **reg Save** e deve ter uma extensão. HIV. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Antes de editar as entradas do registro, você deve salvar a subchave pai usando o comando **reg Save** . Se a edição falhar, você poderá restaurar a subchave original usando a operação **reg Restore** .

- Os valores de retorno para a operação **reg Restore** são:

    | Valor | Descrição |
    |--|--|
    | 0 | Êxito |
    | 1 | Falha |

### <a name="examples"></a>Exemplos

Para restaurar o arquivo chamado NTRKBkUp. HIV na chave HKLM\Software\Microsoft\ResKit e substituir o conteúdo existente da chave, digite:

```
reg restore HKLM\Software\Microsoft\ResKit NTRKBkUp.hiv
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando reg Save](reg-save.md)
