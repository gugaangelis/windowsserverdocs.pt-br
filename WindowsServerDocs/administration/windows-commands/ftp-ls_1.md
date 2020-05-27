---
title: FTP ls
description: Tópico de referência para o comando FTP ls, que exibe uma lista abreviada de arquivos e subdiretórios do computador remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5e03c7db-1e2b-419c-acb2-8a68f3db9615
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9ae913f001c3ddffce9ff81c9c5c5fd32f436da5
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820166"
---
# <a name="ftp-ls"></a>FTP ls

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe uma lista abreviada de arquivos e subdiretórios do computador remoto.

## <a name="syntax"></a>Sintaxe

```
ls [<remotedirectory>] [<localfile>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- |------------ |
| `[<remotedirectory>]` | Especifica o diretório para o qual você deseja ver uma listagem. Se nenhum diretório for especificado, o diretório de trabalho atual no computador remoto será usado. |
| `[<localfile>]` | Especifica um arquivo local no qual armazenar a listagem. Se um arquivo local não for especificado, os resultados serão exibidos na tela. |

### <a name="examples"></a>Exemplos

Para exibir uma lista abreviada de arquivos e subdiretórios do computador remoto, digite:

```
ls
```

Para obter uma listagem de diretório abreviada de *dir1* no computador remoto e salvá-lo em um arquivo local chamado *DirList. txt*, digite:

```
ls dir1 dirlist.txt
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Diretrizes adicionais de FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
