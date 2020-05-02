---
title: bitsadmin replaceremoteprefix
description: Tópico de referência para o comando Bitsadmin replaceremoteprefix, que altera a URL remota para todos os arquivos no trabalho de *oldprefix* para *newprefix*, conforme necessário.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d0e0abb1-bdb4-4c74-abbc-16c809f5fd81
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 745d026513413db799e86df3422d5ee19c89274f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717034"
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

Para alterar a URL remota de todos os arquivos no trabalho denominado *myDownloadJob*, *http://stageserver* de *http://prodserver*para.

```
bitsadmin /replaceremoteprefix myDownloadJob http://stageserver http://prodserver
```

## <a name="additional-information"></a>Informações adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
