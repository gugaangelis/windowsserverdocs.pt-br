---
title: ftp lcd
description: Artigo de referência para o comando de LCD de FTP, que altera o diretório de trabalho no computador local.
ms.topic: reference
ms.assetid: 60a25808-6abb-408b-8373-0bbdcd0994b4
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 91b2990495c91033c1bc885ec9d19142640f55b5
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89621669"
---
# <a name="ftp-lcd"></a>ftp lcd

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera o diretório de trabalho no computador local. Por padrão, o diretório de trabalho é o diretório no qual o comando de **FTP** foi iniciado.

## <a name="syntax"></a>Sintaxe

```
lcd [<directory>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `[<directory>]` | Especifica o diretório no computador local para o qual alterar. Se o *diretório* não for especificado, o diretório de trabalho atual será alterado para o diretório padrão. |

### <a name="examples"></a>Exemplos

Para alterar o diretório de trabalho no computador local para *c:\Dir1*, digite:

```
lcd c:\dir1
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Diretrizes adicionais de FTP](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
