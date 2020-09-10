---
title: ftp mput
description: Artigo de referência para o comando mput do FTP, que copia arquivos locais para o computador remoto usando o tipo de transferência de arquivo atual.
ms.topic: reference
ms.assetid: 980f15e7-7cf1-4813-9946-a8cc4edfb198
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: cd799e713b71f23c7ce22a9ee81dc19cd78f8c31
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635313"
---
# <a name="ftp-mput"></a>ftp mput

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia arquivos locais para o computador remoto usando o tipo de transferência de arquivo atual.

## <a name="syntax"></a>Sintaxe

```
mput <localfile>[ ]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<localfile>` | Especifica o arquivo local a ser copiado para o computador remoto. |

### <a name="examples"></a>Exemplos

Para copiar *Program1.exe* e *Program2.exe* para o computador remoto usando o tipo de transferência de arquivo atual, digite:

```
mput Program1.exe Program2.exe
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando FTP ASCII](ftp-ascii.md)

- [comando Binary FTP](ftp-binary.md)

- [Diretrizes adicionais de FTP](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
