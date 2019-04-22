---
title: informações e cache bitsadmin
description: Tópico de comandos do Windows para **bitsadmin cache e informações** -Despeja uma entrada de cache específica.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 15975cbf-dba6-49ca-a725-d15ce1952de5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 61ff57b33e575921f2032d4e13a2d9b74accae60
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813317"
---
# <a name="bitsadmin-cache-and-info"></a>informações e cache bitsadmin



Despeja uma entrada de cache específica.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /Cache /Info RecordID [/Verbose] 
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|RecordID|O GUID associado com a entrada de cache.|

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir descarta a entrada de cache com o RecordID da {6511FB02-E195-40A2-B595-E8E2F8F47702}.
```
C:\>bitsadmin /Cache /Info {6511FB02-E195-40A2-B595-E8E2F8F47702} 
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)