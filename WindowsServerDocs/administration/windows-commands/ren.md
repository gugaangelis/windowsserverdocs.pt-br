---
title: ren
description: Artigo de referência para o comando Ren, que renomeia um arquivo ou diretório.
ms.topic: article
ms.assetid: 60398e12-a05d-4524-a73a-0a925943e21d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: e21b521947ed02307c470932fffcf466016bae8b
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87883803"
---
# <a name="ren"></a>ren

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Renomeia arquivos ou diretórios.

> [!NOTE]
> Esse comando é o mesmo que o [comando renomear](rename.md).

## <a name="syntax"></a>Sintaxe

```
ren [<drive>:][<path>]<filename1> <filename2>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `[<drive>:][<path>]<filename1>` | Especifica o local e o nome do arquivo ou conjunto de arquivos que você deseja renomear. *Arquivo1* pode incluir caracteres curinga (**&#42;** e **?**). |
| `<filename2>` | Especifica o novo nome para o arquivo. Você pode usar caracteres curinga para especificar novos nomes para vários arquivos. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Você não pode especificar uma nova unidade ou caminho ao renomear arquivos. Você também não pode usar esse comando para renomear arquivos em unidades ou para mover arquivos para um diretório diferente.

- Os caracteres representados por caracteres curinga em *arquivo2* serão idênticos aos caracteres correspondentes em *arquivo1*.

- *Arquivo2* deve ser um nome de arquivo exclusivo. Se *arquivo2* corresponder a um nome de arquivo existente, a seguinte mensagem será exibida: `Duplicate file name or file not found` .

### <a name="examples"></a>Exemplos

Para alterar todas as extensões de nome de arquivo. txt no diretório atual para extensões. doc, digite:

```
ren *.txt *.doc
```

Para alterar o nome de um diretório de *Chap10* para *Part10*, digite:

```
ren chap10 part10
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Renomear comando](rename.md)
