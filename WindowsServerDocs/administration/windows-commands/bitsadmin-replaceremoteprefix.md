---
title: bitsadmin replaceremoteprefix
description: Artigo de referência para o comando Bitsadmin replaceremoteprefix, que altera a URL remota para todos os arquivos no trabalho de *oldprefix* para *newprefix*, conforme necessário.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d0e0abb1-bdb4-4c74-abbc-16c809f5fd81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e2453ac4c223baa049980578c81d9bc6539baac7
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927976"
---
# <a name="bitsadmin-replaceremoteprefix"></a>bitsadmin replaceremoteprefix

Altera a URL remota para todos os arquivos no trabalho de *oldprefix* para *newprefix*, conforme necessário.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /replaceremoteprefix <job> <oldprefix> <newprefix>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| oldprefix | Prefixo de URL existente. |
| newprefix | Novo prefixo de URL. |

## <a name="examples"></a>Exemplos

Para alterar a URL remota de todos os arquivos no trabalho denominado *myDownloadJob*, de *http://stageserver* para *http://prodserver* .

```
bitsadmin /replaceremoteprefix myDownloadJob http://stageserver http://prodserver
```

## <a name="additional-information"></a>Informações adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
