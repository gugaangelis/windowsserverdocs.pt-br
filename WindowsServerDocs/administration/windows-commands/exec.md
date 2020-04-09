---
title: exec
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 364e8baf-576f-401b-a431-7d3c06621614
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d39fbf948050dd00f329e461c34c2365030cb05d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844989"
---
# <a name="exec"></a>exec



executa um arquivo no computador local. O arquivo pode ser um script **cmd** .

## <a name="syntax"></a>Sintaxe

```
exec <ScriptFile.cmd>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<scriptfile. cmd >|Especifica o arquivo de script a ser executado.|

## <a name="remarks"></a>Comentários

-   Esse comando é usado para duplicar ou restaurar dados como parte de uma sequência de backup ou restauração.
-   Se o script falhar, um erro será retornado e o DiskShadow sairá.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)