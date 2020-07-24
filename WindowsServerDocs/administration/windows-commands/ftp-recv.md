---
title: ftp recv
description: Artigo de referência para o comando de recebimento de FTP, que copia um arquivo remoto para o computador local usando o tipo de transferência de arquivo atual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f249ce61-247d-421b-9b93-48bce5108800
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a2b80a0f27bdb4b966ff66d736f05cb82fb2bb81
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86957508"
---
# <a name="ftp-recv"></a>ftp recv

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia um arquivo remoto para o computador local usando o tipo de transferência de arquivo atual.

> [!NOTE]
> Esse comando é o mesmo que o [comando Get do FTP](ftp-get.md).

## <a name="syntax"></a>Sintaxe

```
recv <remotefile> [<localfile>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<remotefile>` | Especifica o arquivo remoto a ser copiado. |
| `[<localfile>]` | Especifica o nome do arquivo a ser usado no computador local. Se *LocalFile* não for especificado, o arquivo receberá o nome do *remotefile*. |

### <a name="examples"></a>Exemplos

Para copiar *test.txt* para o computador local usando a transferência de arquivo atual, digite:

```
recv test.txt
```

Para copiar *test.txt* para o computador local como *test1.txt* usando a transferência de arquivo atual, digite:

```
recv test.txt test1.txt
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Get de FTP](ftp-get.md)

- [comando FTP ASCII](ftp-ascii.md)

- [comando Binary FTP](ftp-binary.md)

- [Diretrizes adicionais de FTP](/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
