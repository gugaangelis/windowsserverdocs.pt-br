---
title: attrib
description: O tópico de comandos do Windows para **attrib**, que exibe, define ou remove atributos atribuídos a arquivos ou diretórios.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5e763ca5-21a2-45d2-b26d-a9c44c99091a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94a6f307450b06dff81180b70f864bd9e1ed5885
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851249"
---
# <a name="attrib"></a>attrib

Exibe, define ou remove atributos atribuídos a arquivos ou diretórios. Se usado sem parâmetros, **attrib** exibe atributos de todos os arquivos no diretório atual.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
attrib [{+|-}r] [{+|-}a] [{+|-}s] [{+|-}h] [{+|-}i] [<Drive>:][<Path>][<FileName>] [/s [/d] [/l]]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `{+|-}r` | Define ( **+** ) ou limpa ( **-** ) o atributo de arquivo somente leitura. |
| `{+\|-}a` | Define ( **+** ) ou limpa ( **-** ) o atributo arquivo morto. |
| `{+\|-}s` | Define ( **+** ) ou limpa ( **-** ) o atributo de arquivo do sistema. |
| `{+\|-}h` | Define ( **+** ) ou limpa ( **-** ) o atributo de arquivo oculto. |
| `{+\|-}i` | Define ( **+** ) ou limpa ( **-** ) o atributo de arquivo não indexado de conteúdo. |
| `[<Drive>:][<Path>][<FileName>]` | Especifica o local e o nome do diretório, arquivo ou grupo de arquivos para os quais você deseja exibir ou alterar atributos. Você pode usar o **?** e **&#42;** caracteres curinga no parâmetro *filename* para exibir ou alterar os atributos de um grupo de arquivos. |
| /s | Aplica **atributos** e opções de linha de comando a arquivos correspondentes no diretório atual e em todos os seus subdiretórios. |
| /d | Aplica **Atrib** e qualquer opção de linha de comando a diretórios. |
| /l | Aplica **Atrib** e quaisquer opções de linha de comando ao link simbólico, em vez do destino do link simbólico. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="remarks"></a>Comentários

- Você pode usar caracteres curinga ( **?** e **&#42;** ) com o parâmetro *filename* para exibir ou alterar os atributos de um grupo de arquivos.

- Se um arquivo tiver o (s) sistema (**es**) ou conjunto de atributos oculto (**h**), você deverá limpar o atributo antes de poder alterar qualquer outro atributo para esse arquivo.

- O atributo de arquivo (**a**) marca os arquivos que foram alterados desde a última vez em que foi feito o backup. Observe que o comando **xcopy** usa atributos de arquivo morto.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para exibir os atributos de um arquivo chamado News86 que está localizado no diretório atual, digite:

```
attrib news86 
```

Para atribuir o atributo somente leitura ao arquivo chamado Report. txt, digite:

```
attrib +r report.txt 
```

Para remover o atributo somente leitura dos arquivos no diretório público e de seus subdiretórios em um disco na unidade B, digite:

```
attrib -r b:\public\*.* /s 
```

Para definir o atributo arquivo morto para todos os arquivos na unidade A e, em seguida, limpe o atributo arquivo morto para arquivos com a extensão. bak, digite:

```
attrib +a a:*.* & attrib -a a:*.bak 
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)