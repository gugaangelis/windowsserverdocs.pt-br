---
title: bitsadmin replaceremoteprefix
description: Artigo de referência para o comando Bitsadmin replaceremoteprefix, que altera a URL remota para todos os arquivos no trabalho de *oldprefix* para *newprefix*, conforme necessário.
ms.topic: reference
ms.assetid: d0e0abb1-bdb4-4c74-abbc-16c809f5fd81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 21c1ede5b05a80ba80b1bd470065e914fb353c3c
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89026324"
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
