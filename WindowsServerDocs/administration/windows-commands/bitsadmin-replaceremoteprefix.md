---
title: bitsadmin replaceremoteprefix
description: Artigo de referência para o comando Bitsadmin replaceremoteprefix, que altera a URL remota para todos os arquivos no trabalho de *oldprefix* para *newprefix*, conforme necessário.
ms.topic: reference
ms.assetid: d0e0abb1-bdb4-4c74-abbc-16c809f5fd81
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 83c517a9126a3b78dfc919af5663e939aceee186
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631127"
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
