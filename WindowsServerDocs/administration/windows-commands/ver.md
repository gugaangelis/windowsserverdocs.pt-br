---
title: ver
description: Tópico de comandos do Windows para ver, que exibe o número de versão do sistema operacional.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5a9c6cd4-b67d-4b30-8c56-5f9798eafd2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d0d0676dcfa6546e4bbf74c4c58a24f51744d00f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830189"
---
# <a name="ver"></a>ver



Exibe o número de versão do sistema operacional.

Esse comando tem suporte no prompt de comando do Windows (cmd. exe), mas não no PowerShell.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
ver
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para obter o número de versão do sistema operacional do Shell de comando (cmd. exe), digite:

```
ver
```

O comando ver não funciona no PowerShell. Para obter a versão do sistema operacional do PowerShell, digite:

```powershell
$PSVersionTable.BuildVersion
````


## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
