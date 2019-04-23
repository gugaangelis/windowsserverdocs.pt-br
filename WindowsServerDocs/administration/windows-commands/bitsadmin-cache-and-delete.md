---
title: excluir e cache bitsadmin
description: Tópico de comandos do Windows para **bitsadmin armazenar em cache e excluir** -exclui uma entrada de cache específica.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 22540273-55a5-46ea-869b-6df2aa6808a1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 63b82cbbadebf2c4e36f2c76076b329787d7b1b5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852507"
---
# <a name="bitsadmin-cache-and-delete"></a>excluir e cache bitsadmin



Exclui uma entrada de cache específica.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /Cache /Delete RecordID 
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|RecordID|O GUID associado com a entrada de cache.|

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir exclui a entrada de cache com o RecordID da {6511FB02-E195-40A2-B595-E8E2F8F47702}.
```
C:\>bitsadmin /Cache /Delete {6511FB02-E195-40A2-B595-E8E2F8F47702} 
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)