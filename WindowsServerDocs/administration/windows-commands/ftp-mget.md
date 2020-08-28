---
title: ftp mget
description: Artigo de referência para o comando mget do FTP, que copia arquivos remotos para o computador local usando o tipo de transferência de arquivo atual.
ms.topic: reference
ms.assetid: 6c85ae96-ec51-48a9-a227-7f02c7332c69
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e72d253fec35f366e2ab80a491c256e0de6c948f
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025740"
---
# <a name="ftp-mget"></a>ftp mget

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia arquivos remotos para o computador local usando o tipo de transferência de arquivo atual.

## <a name="syntax"></a>Sintaxe

```
mget <remotefile>[ ]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<remotefile>` | Especifica os arquivos remotos a serem copiados para o computador local. |

### <a name="examples"></a>Exemplos

Para copiar arquivos remotos *a.exe* e *b.exe* no computador local usando o tipo de transferência de arquivo atual, digite:

```
mget a.exe b.exe
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando FTP ASCII](ftp-ascii.md)

- [comando Binary FTP](ftp-binary.md)

- [Diretrizes adicionais de FTP](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
