---
title: EXEC
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 364e8baf-576f-401b-a431-7d3c06621614
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ecdfd05b8abefb35946b783daaa3220a6713a38d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882917"
---
# <a name="exec"></a>EXEC



Executa um arquivo no computador local. O arquivo pode ser um **cmd** script.

## <a name="syntax"></a>Sintaxe

```
exec <ScriptFile.cmd>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<ScriptFile.cmd>|Especifica o arquivo de script para executar.|

## <a name="remarks"></a>Comentários

-   Esse comando é usado para duplicar ou restaurar os dados como parte de um backup ou a sequência de restauração.
-   Se o script falhar, um erro será retornado e fecha o DiskShadow.

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)