---
title: exec
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 364e8baf-576f-401b-a431-7d3c06621614
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f10e28a8da96bc7228af4561fb36824899f2d7a4
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725734"
---
# <a name="exec"></a>exec



Executa um arquivo no computador local. O arquivo pode ser um script **cmd** .

## <a name="syntax"></a>Sintaxe

```
exec <ScriptFile.cmd>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<> scriptfile. cmd|Especifica o arquivo de script a ser executado.|

## <a name="remarks"></a>Comentários

-   Esse comando é usado para duplicar ou restaurar dados como parte de uma sequência de backup ou restauração.
-   Se o script falhar, um erro será retornado e o DiskShadow sairá.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)