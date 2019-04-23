---
title: bitsadmin list
description: Tópico de comandos do Windows para **bitsadmin lista** -lista os trabalhos de transferência pertencentes ao usuário atual.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1416965e-e0e6-49cf-b1d4-b286d3cf8716
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f0b88001b9c4ae01b57006ffeef66dec0348ca77
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873857"
---
# <a name="bitsadmin-list"></a>bitsadmin list



Lista os trabalhos de transferência pertencentes ao usuário atual.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /List [/allusers][/verbose]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/Allusers|Opcional — lista os trabalhos para todos os usuários|
|/Verbose|Opcional – fornece informações detalhadas para cada trabalho.|

## <a name="remarks"></a>Comentários

Você deve ter privilégios de administrador para usar o parâmetro /allusers

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir recupera informações sobre trabalhos pertencentes ao usuário atual.
```
C:\>bitsadmin /List 
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)