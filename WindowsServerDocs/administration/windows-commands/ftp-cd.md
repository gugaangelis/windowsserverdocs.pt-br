---
title: CD do FTP
description: Tópico de referência para o comando CD do FTP, que altera o diretório de trabalho no computador remoto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a574855a-31b4-45c6-bce2-581c7231c99b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dbeff9c2fca654120347f0f616325b700f5c9be3
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2020
ms.locfileid: "83819996"
---
# <a name="ftp-cd"></a>CD do FTP

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera o diretório de trabalho no computador remoto.

## <a name="syntax"></a>Sintaxe

```
cd <remotedirectory>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| <remotedirectory> | Especifica o diretório no computador remoto para o qual você deseja alterar. |

### <a name="examples"></a>Exemplos

Para alterar o diretório no computador remoto para *documentos*, digite:

```
cd Docs
```

Para alterar o diretório no computador remoto para *vídeos de maio*, digite:

```
cd  May Videos
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Diretrizes adicionais de FTP](https://docs.microsoft.com/previous-versions/orphan-topics/ws.10/cc756013(v=ws.10))
