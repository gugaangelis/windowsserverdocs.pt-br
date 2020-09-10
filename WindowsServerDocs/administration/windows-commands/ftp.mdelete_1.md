---
title: ftp mdelete
description: Artigo de referência para o comando mdelete do FTP, que exclui arquivos no computador remoto.
ms.topic: reference
ms.assetid: 8a80a8f5-e880-40a8-abc9-29a41836844f
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4b9ca1843fd405aef2a1c28c8b7f94bbc650f2d4
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89627622"
---
# <a name="ftp-mdelete"></a>ftp mdelete

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exclui arquivos no computador remoto.

## <a name="syntax"></a>Sintaxe
```
mdelete <remotefile>[...]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<remotefile>` | Especifica o arquivo remoto a ser excluído. |

### <a name="examples"></a>Exemplos

Para excluir arquivos remotos *a.exe* e *b.exe*, digite:

```
mdelete a.exe b.exe
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Diretrizes adicionais de FTP](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
