---
title: ftp ls
description: Artigo de referência do comando FTP ls, que exibe uma lista abreviada de arquivos e subdiretórios do computador remoto.
ms.topic: article
ms.assetid: 5e03c7db-1e2b-419c-acb2-8a68f3db9615
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 59e19d7e48b902ccc0704c22e150b3494fb2ad2b
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87889348"
---
# <a name="ftp-ls"></a>ftp ls

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

Para obter uma listagem de diretório abreviada de *dir1* no computador remoto e salvá-lo em um arquivo local chamado *dirlist.txt*, digite:

```
ls dir1 dirlist.txt
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Diretrizes adicionais de FTP](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
