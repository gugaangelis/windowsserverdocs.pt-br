---
title: ktmutil
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 53bc56df-f0e5-443b-ab20-bbf8b11d4a9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 47b447165ee54e6839bb6338801c6703d818caa8
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724519"
---
# <a name="ktmutil"></a>ktmutil



Inicia o utilitário Gerenciador de transações do kernel. Se usado sem parâmetros, **KTMUTIL** exibirá os subcomandos disponíveis.



## <a name="syntax"></a>Sintaxe

```
ktmutil list tms 
ktmutil list transactions [{TmGuid}]
ktmutil resolve complete {TmGuid} {RmGuid} {EnGuid}
ktmutil resolve commit {TxGuid}
ktmutil resolve rollback {TxGuid}
ktmutil force commit {??Guid}
ktmutil force rollback {??Guid}
ktmutil forget
```

### <a name="parameters"></a>Parâmetros

## <a name="remarks"></a>Comentários

## <a name="examples"></a>Exemplos

Para forçar uma transação incerta com GUID 311a9209-03f4-11dc-918f-00188b8f707b a ser confirmada, digite:
```
ktmutil force commit {311a9209-03f4-11dc-918f-00188b8f707b}
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)