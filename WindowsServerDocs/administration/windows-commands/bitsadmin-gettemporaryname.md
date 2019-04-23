---
title: Bitsadmin gettemporaryname
description: Tópico de comandos do Windows para **gettemporaryname bitsadmin** -relata o nome de arquivo temporário de determinado arquivo dentro do trabalho.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 68925edc-a801-4292-a812-7471c4f60fdd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 762a2a5943202b38e94a245b74745e6631e0792d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59876707"
---
# <a name="bitsadmin-gettemporaryname"></a>Bitsadmin gettemporaryname



Relata o nome de arquivo temporário de determinado arquivo dentro do trabalho.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetTemporaryName <Job> <file index> 
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|
|Índice de arquivo|Começa em 0|

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir relata o nome de arquivo temporário do arquivo 2 do trabalho nomeado *myJob*.
```
C:\>bitsadmin /GetTemporaryName myJob 1 
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)