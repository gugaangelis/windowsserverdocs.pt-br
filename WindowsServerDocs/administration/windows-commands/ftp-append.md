---
title: ftp append
description: Artigo de referência do comando FTP Append, que acrescenta um arquivo local a um arquivo no computador remoto usando a configuração de tipo de arquivo atual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7c1a133c-31dc-41a4-9eb9-258efd79804d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f1d1cf36139f00f3d61e400cb38960d2c8551532
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85925162"
---
# <a name="ftp-append"></a>ftp append

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

Para acrescentar *file1.txt* ao *file2.txt* no computador remoto, digite:

```
append file1.txt file2.txt
```

Para acrescentar o *file1.txt* local a um arquivo chamado *file1.txt* no computador remoto.

```
append file1.txt
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Diretrizes adicionais de FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
