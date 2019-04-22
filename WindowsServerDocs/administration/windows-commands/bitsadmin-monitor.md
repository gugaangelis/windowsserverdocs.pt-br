---
title: bitsadmin monitor
description: Tópico de comandos do Windows para **bitsadmin monitor** -monitora os trabalhos na fila de transferência que o usuário atual possui.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2c4620d5c8e46cb8bfcb6b9c83261d57781abea5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814587"
---
# <a name="bitsadmin-monitor"></a>bitsadmin monitor



Monitora os trabalhos na fila de transferência que o usuário atual possui.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /Monitor [/allusers] [/refresh <Seconds>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|ALLUSERS|Opcional – monitora trabalhos para todos os usuários.|
|Atualizar|Opcional – atualiza os dados em um intervalo especificado por *segundos*. O intervalo de atualização padrão é cinco segundos.|

## <a name="remarks"></a>Comentários

Você deve ter privilégios de administrador para usar o **Allusers** parâmetro.

Use CTRL + C para parar a atualização.

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir monitora a fila de transferência de trabalhos pertencentes ao usuário atual e atualiza as informações a cada 60 segundos.
```
C:\>bitsadmin /Monitor /refesh 60
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)