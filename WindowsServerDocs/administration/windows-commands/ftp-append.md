---
title: acréscimo de FTP
description: Tópico de referência para o comando FTP Append, que acrescenta um arquivo local a um arquivo no computador remoto usando a configuração de tipo de arquivo atual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7c1a133c-31dc-41a4-9eb9-258efd79804d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7d1b6ab4a6ae0c1654d4335d24f135b2893bdcb7
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2020
ms.locfileid: "83819136"
---
# <a name="ftp-append"></a>acréscimo de FTP

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Anexa um arquivo local a um arquivo no computador remoto usando a configuração de tipo de arquivo atual.

## <a name="syntax"></a>Sintaxe

```
append <localfile> [remotefile]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<localfile>` | Especifica o arquivo local a ser adicionado. |
| [remotofile] | Especifica o arquivo no computador remoto ao qual <localfile> é adicionado. Se você não usar esse parâmetro, o `<localfile>` nome será usado no lugar do nome do arquivo remoto. |

### <a name="examples"></a>Exemplos

Para acrescentar *file1. txt* a *file2. txt* no computador remoto, digite:

```
append file1.txt file2.txt
```

Para acrescentar o *arquivo1. txt* local a um arquivo chamado *arquivo1. txt* no computador remoto.

```
append file1.txt
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Diretrizes adicionais de FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
