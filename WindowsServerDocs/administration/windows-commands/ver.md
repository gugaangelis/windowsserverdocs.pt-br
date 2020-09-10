---
title: ver
description: Artigo de referência para ver, que exibe o número de versão do sistema operacional.
ms.topic: reference
ms.assetid: 5a9c6cd4-b67d-4b30-8c56-5f9798eafd2a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 1dcbfc9857d23759f919ad01d98b0436b673206e
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89636333"
---
# <a name="ver"></a>ver



Exibe o número de versão do sistema operacional.

Esse comando tem suporte no prompt de comando do Windows (Cmd.exe), mas não no PowerShell.



## <a name="syntax"></a>Sintaxe

```
ver
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|DESCRIÇÃO|
|---------|-----------|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="examples"></a>Exemplos

Para obter o número de versão do sistema operacional do Shell de comando (cmd.exe), digite:

```
ver
```

O comando ver não funciona no PowerShell. Para obter a versão do sistema operacional do PowerShell, digite:

```powershell
$PSVersionTable.BuildVersion
````


## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
