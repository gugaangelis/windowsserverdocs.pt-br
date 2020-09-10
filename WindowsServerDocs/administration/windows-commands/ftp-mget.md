---
title: ftp mget
description: Artigo de referência para o comando mget do FTP, que copia arquivos remotos para o computador local usando o tipo de transferência de arquivo atual.
ms.topic: reference
ms.assetid: 6c85ae96-ec51-48a9-a227-7f02c7332c69
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ca56e43df4a03fd52f8028d390cb2da62901ca06
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89621619"
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
