---
title: setlimit e cache bitsadmin
description: Tópico de comandos do Windows para **bitsadmin cache e setlimit** -define o limite de tamanho do cache.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 46578835-d5ce-423b-be4d-62ddb9e1908d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7d0b72c5ec6c779fa4ce3fa038352836cd9456ac
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852587"
---
# <a name="bitsadmin-cache-and-setlimit"></a>setlimit e cache bitsadmin



Define o limite de tamanho do cache.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /Cache /SetLimit Percent
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Porcentagem|O limite de cache definido como uma porcentagem do espaço total do disco rígido...|

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir limita o tamanho do cache como 50%.
```
C:\>bitsadmin /Cache /SetLimit 50 
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)