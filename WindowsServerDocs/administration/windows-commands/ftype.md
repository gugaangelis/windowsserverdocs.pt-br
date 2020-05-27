---
title: ftype
description: Tópico de referência para o comando ftype, que exibe ou modifica o tipo de arquivo usado em associações de extensão de nome de arquivo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6fb53cee-9bed-44dd-af5d-bc7cec1dd114
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a1387a9f8cb607d3563a381c757ea237104e6032
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820206"
---
# <a name="ftype"></a>ftype

Exibe ou modifica os tipos de arquivo que são usados em associações de extensão de nome de arquivo. Se usado sem um operador de atribuição (=), esse comando exibirá a cadeia de caracteres do comando Open atual para o tipo de arquivo especificado. Se usado sem parâmetros, esse comando exibe os tipos de arquivo que têm cadeias de caracteres de comando abertas definidas.

> [!NOTE]
> Só há suporte para esse comando no cmd. exe e ele não está disponível no PowerShell.
> Embora você possa usar `cmd /c ftype` como solução alternativa.

## <a name="syntax"></a>Sintaxe

```
ftype [<filetype>[=[<opencommandstring>]]]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<filetype>` | Especifica o tipo de arquivo a ser exibido ou alterado. |
| `<opencommandstring>` | Especifica a cadeia de caracteres do comando Open a ser usada ao abrir arquivos do tipo de arquivo especificado.|
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

A tabela a seguir descreve como o **ftype** substitui variáveis dentro de uma cadeia de caracteres de comando Open:

| Variável | Valor de substituição |
| -------- | ----------------- |
| `%0` ou `%1` | É substituído pelo nome do arquivo que está sendo iniciado por meio da associação. |
| `%*` | Obtém todos os parâmetros. |
| `%2`, `%3`, ... | Obtém o primeiro parâmetro ( `%2` ), o segundo parâmetro ( `%3` ) e assim por diante. |
| `%~<n>` | Obtém todos os parâmetros restantes começando com o *n*-ésimo parâmetro, em que *n* pode ser qualquer número de 2 a 9. |

### <a name="examples"></a>Exemplos

Para exibir os tipos de arquivo atuais que têm cadeias de comando abertas definidas, digite:

```
ftype
```

Para exibir a cadeia de comando Open atual para o tipo de arquivo *txtfile* , digite:

```
ftype txtfile
```

Esse comando gera uma saída semelhante à seguinte:

`txtfile=%SystemRoot%\system32\NOTEPAD.EXE %1`

Para excluir a cadeia de comando Open para um tipo de arquivo chamado *example*, digite:

```
ftype example=
```

Para associar a extensão de nome de arquivo. pl ao tipo de arquivo PerlScript e habilitar o tipo de arquivo PerlScript para executar o PERL. EXE, digite os seguintes comandos:

```
assoc .pl=PerlScript
ftype PerlScript=perl.exe %1 %*
```

Para eliminar a necessidade de digitar a extensão de nome de arquivo. pl ao invocar um script Perl, digite:

```
set PATHEXT=.pl;%PATHEXT%
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
