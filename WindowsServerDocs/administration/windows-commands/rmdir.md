---
title: rmdir
description: Artigo de referência do comando rmdir, que exclui um diretório.
ms.topic: reference
ms.assetid: e4d7162b-7132-413c-a416-6eb3dc032a26
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d67c0bb16ad47457cfb6eb927ba9dc5cf167b20c
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89036234"
---
# <a name="rmdir"></a>rmdir

Exclui um diretório.

O comando **rmdir** também pode ser executado no console de recuperação do Windows, usando parâmetros diferentes. Para obter mais informações, consulte [ambiente de recuperação do Windows (WinRE)](/windows-hardware/manufacture/desktop/windows-recovery-environment--windows-re--technical-reference).

> [!NOTE]
> Esse comando é o mesmo que o [comando RD](rd.md).

## <a name="syntax"></a>Sintaxe

```
rmdir [<drive>:]<path> [/s [/q]]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `[<drive>:]<path>` | Especifica o local e o nome do diretório que você deseja excluir. O *caminho* é necessário. Se você incluir uma barra invertida ( \) no início do *caminho*especificado, o *caminho* será iniciado no diretório raiz (independentemente do diretório atual). |
| /s | Exclui uma árvore de diretórios (o diretório especificado e todos os seus subdiretórios, incluindo todos os arquivos). |
| /q | Especifica o modo silencioso. Não solicita confirmação ao excluir uma árvore de diretórios. O parâmetro **/q** só funcionará se **/s** também for especificado.<p>**Cuidado:** Quando você executa no modo silencioso, toda a árvore de diretórios é excluída sem confirmação. Certifique-se de que arquivos importantes sejam movidos ou submetidos a backup antes de usar a opção de linha de comando **/q** . |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Você não pode excluir um diretório que contém arquivos, incluindo arquivos ocultos ou do sistema. Se você tentar fazer isso, a seguinte mensagem será exibida:

    `The directory is not empty`

    Use o comando **dir/a** para listar todos os arquivos (incluindo arquivos ocultos e do sistema). Em seguida, use o comando **attrib** com **-h** para remover atributos de arquivo oculto, **-s** para remover atributos de arquivo do sistema ou **-h-s** para remover os atributos de arquivo ocultos e do sistema. Depois que os atributos ocultos e de arquivo forem removidos, você poderá excluir os arquivos.

- Você não pode usar o comando **rmdir** para excluir o diretório atual. Se você tentar excluir o diretório atual, a seguinte mensagem de erro será exibida:

    `The process can't access the file because it is being used by another process.`

    Se você receber essa mensagem de erro, deverá alterar para um diretório diferente (não um subdiretório do diretório atual) e, em seguida, tentar novamente.

## <a name="examples"></a>Exemplos

Para alterar para o diretório pai para que você possa remover com segurança o diretório desejado, digite:

```
cd ..
```

Para remover um diretório chamado *Test* (e todos os seus arquivos e subdiretórios) do diretório atual, digite:

```
rmdir /s test
```

Para executar o exemplo anterior no modo silencioso, digite:

```
rmdir /s /q test
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
