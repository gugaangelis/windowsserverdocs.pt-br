---
title: ftp mget
description: Artigo de referência para o comando mget do FTP, que copia arquivos remotos para o computador local usando o tipo de transferência de arquivo atual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6c85ae96-ec51-48a9-a227-7f02c7332c69
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7239f31fa0a9b40d9bba4db0c3e1e29df5c03108
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85925895"
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

- [Diretrizes adicionais de FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
