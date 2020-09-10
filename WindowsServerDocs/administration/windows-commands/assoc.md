---
title: assoc
description: Artigo de referência para o comando Assoc, que exibe ou modifica associações de extensão de nome de arquivo.
ms.topic: reference
ms.assetid: 237bedda-b24c-4fec-a39c-9b7eacf96417
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 1ce1ee97dd386757ab5802ac2f493e25635ff66f
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633446"
---
# <a name="assoc"></a>assoc

Exibe ou modifica associações de extensão de nome de arquivo. Se usado sem parâmetros, **assoc** exibe uma lista de todas as associações de extensão de nome de arquivo atuais.

> [!NOTE]
> Esse comando só tem suporte no cmd.exe e não está disponível no PowerShell.
> Embora você possa usar `cmd /c assoc` como solução alternativa.

## <a name="syntax"></a>Sintaxe

```
assoc [<.ext>[=[<filetype>]]]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<.ext>` | Especifica a extensão de nome de arquivo. |
| `<filetype>` | Especifica o tipo de arquivo a ser associado à extensão de nome de arquivo especificada. |
| /? | Exibe a ajuda no prompt de comando. |

### <a name="remarks"></a>Comentários

- Para remover a associação de tipo de arquivo para uma extensão de nome de arquivo, adicione um espaço em branco após o sinal de igual pressionando a barra de espaços.

- Para exibir os tipos de arquivo atuais que têm cadeias de comando abertas definidas, use o comando **ftype** .

- Para redirecionar a saída de **assoc** para um arquivo de texto, use o `>` operador de redirecionamento.

## <a name="examples"></a>Exemplos

Para exibir a associação de tipo de arquivo atual para a extensão de nome de arquivo. txt, digite:

```
assoc .txt
```

Para remover a associação de tipo de arquivo para a extensão de nome de arquivo. bak, digite:

```
assoc .bak=
```

> [!NOTE]
> Certifique-se de adicionar um espaço após o sinal de igual.

Para exibir a saída de **assoc** uma tela por vez, digite:

```
assoc | more
```

Para enviar a saída de **assoc** para o arquivo assoc.txt, digite:

```
assoc>assoc.txt
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando ftype](ftype.md)
