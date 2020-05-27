---
title: Dir. FTP
description: Tópico de referência para o comando FTP dir, que exibe uma lista de arquivos de diretório e subdiretórios em um computador remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a29a92a5-7b79-4e6e-95cf-2ccb38bb6fb2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6a120691964b7303cf3241ffef2f11d81573ba4d
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2020
ms.locfileid: "83819856"
---
# <a name="ftp-dir"></a>Dir. FTP

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe uma lista de arquivos e subdiretórios do diretório em um computador remoto.

## <a name="syntax"></a>Sintaxe

```
dir [<remotedirectory>] [<localfile>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| ------- | -------- |
| `[<remotedirectory>]` | Especifica o diretório para o qual você deseja ver uma listagem. Se nenhum diretório for especificado, o diretório de trabalho atual no computador remoto será usado. |
| `[<localfile>]` | Especifica um arquivo local no qual armazenar a listagem de diretório. Se um arquivo local não for especificado, os resultados serão exibidos na tela. |

### <a name="examples"></a>Exemplos

Para exibir uma listagem de diretório para *dir1* no computador remoto, digite:

```
dir dir1
```

Para salvar uma lista do diretório atual no computador remoto no arquivo local *DirList. txt*, digite:

```
dir . dirlist.txt
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Diretrizes adicionais de FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
