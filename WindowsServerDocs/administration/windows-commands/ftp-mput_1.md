---
title: mput FTP
description: Tópico de referência para o comando mput do FTP, que copia arquivos locais para o computador remoto usando o tipo de transferência de arquivo atual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 980f15e7-7cf1-4813-9946-a8cc4edfb198
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5006c1ba19f0e017dea377b47bd0d89a68266382
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820406"
---
# <a name="ftp-mput"></a>mput FTP

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

Para copiar *Program1. exe* e *Program2. exe* para o computador remoto usando o tipo de transferência de arquivo atual, digite:

```
mput Program1.exe Program2.exe
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando FTP ASCII](ftp-ascii.md)

- [comando Binary FTP](ftp-binary.md)

- [Diretrizes adicionais de FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
