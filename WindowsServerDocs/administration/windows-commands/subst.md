---
title: subst
description: Artigo de referência para o comando subst, que associa um caminho a uma letra da unidade.
ms.topic: reference
ms.assetid: 3e69234c-2312-4343-868b-afc1017c622a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 52d45c9139c70c4f513a972f7789f872145c8b63
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718223"
---
# <a name="subst"></a>subst

Associa um caminho a uma letra da unidade. Se usado sem parâmetros, **subst** exibirá os nomes das unidades virtuais em vigor.

## <a name="syntax"></a>Sintaxe

```
subst [<drive1>: [<drive2>:]<path>]
subst <drive1>: /d
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<drive1>:` | Especifica a unidade virtual à qual você deseja atribuir um caminho. |
| `[<drive2>:]<path>` | Especifica a unidade física e o caminho que você deseja atribuir a uma unidade virtual. |
| /d | Exclui uma unidade substituída (virtual). |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="remarks"></a>Comentários

- Os comandos a seguir não funcionam e não devem ser usados em unidades especificadas no comando **subst** :

  - [comando chkdsk](chkdsk.md)

    [comando diskcomp](diskcomp.md)

    [comando diskcopy](diskcopy.md)

    [comando Format](format.md)

    [comando de rótulo](label.md)

    [comando de recuperação](recover.md)

- O `<drive1>` parâmetro deve estar dentro do intervalo especificado pelo comando **LastDrive** . Caso contrário, **subst** exibirá a seguinte mensagem de erro: `Invalid parameter - drive1:`

## <a name="examples"></a>Exemplos

Para criar uma unidade virtual z para o caminho b:\user\betty\forms, digite:

```
subst z: b:\user\betty\forms
```

Em vez de digitar o caminho completo, você pode acessar esse diretório digitando a letra da unidade virtual seguida por dois-pontos, da seguinte maneira:

```
z:
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)