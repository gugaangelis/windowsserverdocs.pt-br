---
title: attrib
description: Artigo de referência para o comando attrib, que exibe, define ou remove atributos atribuídos a arquivos ou diretórios.
ms.topic: reference
ms.assetid: 5e763ca5-21a2-45d2-b26d-a9c44c99091a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: de5d045421bb71feff70ec608f6b438fbf73942b
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89029224"
---
# <a name="attrib"></a>attrib

Exibe, define ou remove atributos atribuídos a arquivos ou diretórios. Se usado sem parâmetros, **attrib** exibe atributos de todos os arquivos no diretório atual.

## <a name="syntax"></a>Sintaxe

```
attrib [{+|-}r] [{+|-}a] [{+|-}s] [{+|-}h] [{+|-}i] [<drive>:][<path>][<filename>] [/s [/d] [/l]]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `{+|-}r` | Define ( **+** ) ou limpa ( **-** ) o atributo de arquivo somente leitura. |
| `{+\|-}a` | Define ( **+** ) ou limpa ( **-** ) o atributo de arquivo morto. Este conjunto de atributos marca os arquivos que foram alterados desde a última vez em que foram feitos backups. Observe que o comando **xcopy** usa atributos de arquivo morto. |
| `{+\|-}s` | Define ( **+** ) ou limpa ( **-** ) o atributo de arquivo do sistema. Se um arquivo usar esse conjunto de atributos, você deverá limpar o atributo antes de alterar qualquer outro atributo para o arquivo. |
| `{+\|-}h` | Define ( **+** ) ou limpa ( **-** ) o atributo de arquivo oculto. Se um arquivo usar esse conjunto de atributos, você deverá limpar o atributo antes de alterar qualquer outro atributo para o arquivo. |
| `{+\|-}i` | Define ( **+** ) ou limpa ( **-** ) o atributo de arquivo sem conteúdo indexado. |
| `[<drive>:][<path>][<filename>]` | Especifica o local e o nome do diretório, arquivo ou grupo de arquivos para os quais você deseja exibir ou alterar atributos.<p>Você pode usar o **?** e **&#42;** caracteres curinga no parâmetro *filename* para exibir ou alterar os atributos de um grupo de arquivos. |
| /s | Aplica **atributos** e opções de linha de comando a arquivos correspondentes no diretório atual e em todos os seus subdiretórios. |
| /d | Aplica **Atrib** e qualquer opção de linha de comando a diretórios. |
| /l | Aplica **Atrib** e quaisquer opções de linha de comando ao link simbólico, em vez do destino do link simbólico. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para exibir os atributos de um arquivo chamado News86 que está localizado no diretório atual, digite:

```
attrib news86
```

Para atribuir o atributo somente leitura ao arquivo chamado report.txt, digite:

```
attrib +r report.txt
```

Para remover o atributo somente leitura dos arquivos no diretório público e seus subdiretórios em um disco na unidade b:, digite:

```
attrib -r b:\public\*.* /s
```

Para definir o atributo de arquivo morto para todos os arquivos na unidade a: e, em seguida, limpe o atributo de arquivo para arquivos com a extensão. bak, digite:

```
attrib +a a:*.* & attrib -a a:*.bak
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando xcopy](xcopy.md)
