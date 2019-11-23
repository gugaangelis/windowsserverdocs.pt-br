---
title: exec
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 514503e4920e16ba6778185af32f925541805223
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377426"
---
# <a name="exec"></a>exec



executa um arquivo no computador local. O arquivo pode ser um script **cmd** .

## <a name="syntax"></a>Sintaxe

```
exec <ScriptFile.cmd>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<scriptfile. cmd >|Especifica o arquivo de script a ser executado.|

## <a name="remarks"></a>Comentários

-   Esse comando é usado para duplicar ou restaurar dados como parte de uma sequência de backup ou restauração.
-   Se o script falhar, um erro será retornado e o DiskShadow sairá.

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)