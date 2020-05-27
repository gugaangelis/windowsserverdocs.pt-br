---
title: exclusão de FTP
description: Tópico de referência para o comando FTP Delete, que exclui arquivos em computadores remotos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 067c45f3-e4e8-4450-b8b6-836994f6adfe
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 43ea2ef90f29970a42b0196717ce4d2d3552a32c
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2020
ms.locfileid: "83819947"
---
# <a name="ftp-delete"></a>exclusão de FTP

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exclui arquivos em computadores remotos.

## <a name="syntax"></a>Sintaxe

```
delete <remotefile>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<remotefile>` | Especifica o arquivo a ser excluído. |

### <a name="examples"></a>Exemplos

Para excluir o arquivo *Test. txt* no computador remoto, digite:

```
delete test.txt
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Diretrizes adicionais de FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
