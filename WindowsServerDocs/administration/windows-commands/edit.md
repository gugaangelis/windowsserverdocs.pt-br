---
title: editar
description: Tópico de referência para o comando Editar, que inicia o editor do MS-DOS, para que você possa criar e alterar arquivos de texto ASCII.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4e0ff2e8-3518-47c1-8c69-5e93f895fa0e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a9f6c78889f466015d60149c27a87dcefe840133
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436911"
---
# <a name="edit"></a>editar

Inicia o editor do MS-DOS, que cria e altera arquivos de texto ASCII.

## <a name="syntax"></a>Sintaxe

```
edit [/b] [/h] [/r] [/s] [/<nnn>] [[<drive>:][<path>]<filename> [<filename2> [...]]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `[<drive>:][<path>]<filename> [<filename2> [...]]` | Especifica o local e o nome de um ou mais arquivos de texto ASCII. Se o arquivo não existir, o editor do MS-DOS o criará. Se o arquivo existir, o editor do MS-DOS o abrirá e exibirá seu conteúdo na tela. A opção *filename* pode conter caracteres curinga (**&#42;** e **?**). Separe vários nomes de arquivo com espaços. |
| /b | Força o modo monocromático, para que o editor do MS-DOS seja exibido em preto e branco. |
| /h | Exibe o número máximo de linhas possíveis para o monitor atual. |
| /r | Carrega arquivo (s) no modo somente leitura. |
| /s | Força o uso de nomes de FileName curtos. |
| `<nnn>` | Carrega arquivo (s) binários, encapsulando linhas em *nnn* caracteres de largura. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Para obter ajuda adicional, abra o editor do MS-DOS e pressione a tecla F1.

- Alguns monitores não dão suporte à exibição de teclas de atalho por padrão. Se o monitor não exibir as teclas de atalho, use **/b**.

### <a name="examples"></a>Exemplos

Para abrir o editor do MS-DOS, digite:

```
edit
```

Para criar e editar um arquivo chamado *newtextfile. txt* no diretório atual, digite:

```
edit newtextfile.txt
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
