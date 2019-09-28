---
title: bitsadmin monitor
description: Tópico de comandos do Windows para **Monitor de Bitsadmin** -monitora trabalhos na fila de transferência que o usuário atual possui.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2c424d27-e011-49c2-b579-a2c235467c39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fe4963349c7e17fc77500b5adfceafc48a20ac5f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381224"
---
# <a name="bitsadmin-monitor"></a>bitsadmin monitor



Monitora trabalhos na fila de transferência que o usuário atual possui.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /Monitor [/allusers] [/refresh <Seconds>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|AllUsers|Opcional — monitora trabalhos para todos os usuários.|
|Atualizar|Opcional – atualiza os dados em um intervalo especificado por *segundos*. O intervalo de atualização padrão é cinco segundos.|

## <a name="remarks"></a>Comentários

Você deve ter privilégios de administrador para usar o parâmetro **AllUsers** .

Use CTRL + C para interromper a atualização.

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir monitora a fila de transferência de trabalhos pertencentes ao usuário atual e atualiza as informações a cada 60 segundos.
```
C:\>bitsadmin /Monitor /refesh 60
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)