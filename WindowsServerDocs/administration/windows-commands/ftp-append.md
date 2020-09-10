---
title: ftp append
description: Artigo de referência do comando FTP Append, que acrescenta um arquivo local a um arquivo no computador remoto usando a configuração de tipo de arquivo atual.
ms.topic: reference
ms.assetid: 7c1a133c-31dc-41a4-9eb9-258efd79804d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 42270b8f3633158e12d472a234fcf1904cee86de
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89638578"
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

- [Diretrizes adicionais de FTP](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
