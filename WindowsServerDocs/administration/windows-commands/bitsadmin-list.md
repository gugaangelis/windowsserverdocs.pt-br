---
title: bitsadmin list
description: Tópico de comandos do Windows para **lista de Bitsadmin** – lista os trabalhos de transferência de Propriedade do usuário atual.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: bd4787f51dc2a7843ff6cf5c4f786658e530ad8f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381101"
---
# <a name="bitsadmin-list"></a>bitsadmin list



Lista os trabalhos de transferência de Propriedade do usuário atual.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /List [/allusers][/verbose]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/Allusers|Opcional — lista os trabalhos para todos os usuários|
|/Verbose|Opcional — fornece informações detalhadas para cada trabalho.|

## <a name="remarks"></a>Comentários

Você deve ter privilégios de administrador para usar o parâmetro/AllUsers

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir recupera informações sobre trabalhos pertencentes ao usuário atual.
```
C:\>bitsadmin /List 
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)