---
title: Get de FTP
description: Tópico de referência para o comando Get do FTP, que copia um arquivo remoto para o computador local usando o tipo de transferência de arquivo atual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d70355c4-58ef-43e0-916b-c7ecf77e6ee4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b7254cac15afc446695f22ee1a63f2f4573d3565
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2020
ms.locfileid: "83819706"
---
# <a name="ftp-get"></a>Get de FTP

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia um arquivo remoto para o computador local usando o tipo de transferência de arquivo atual.

> [!NOTE]
> Esse comando é o mesmo que o [comando de recebimento de FTP](ftp-recv.md).

## <a name="syntax"></a>Sintaxe

```
get <remotefile> [<localfile>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<remotefile>` | Especifica o arquivo remoto a ser copiado. |
| `[<localfile>]` | Especifica o nome do arquivo a ser usado no computador local. Se *LocalFile* não for especificado, o arquivo receberá o nome do *remotefile*. |

### <a name="examples"></a>Exemplos

Para copiar *Test. txt* para o computador local usando a transferência de arquivo atual, digite:

```
get test.txt
```

Para copiar *Test. txt* para o computador local como *Test1. txt* usando a transferência de arquivo atual, digite:

```
get test.txt test1.txt
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando de recebimento de FTP](ftp-recv.md)

- [comando FTP ASCII](ftp-ascii.md)

- [comando Binary FTP](ftp-binary.md)

- [Diretrizes adicionais de FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
