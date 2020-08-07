---
title: ver
description: Artigo de referência para ver, que exibe o número de versão do sistema operacional.
ms.topic: article
ms.assetid: 5a9c6cd4-b67d-4b30-8c56-5f9798eafd2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: de080395c2e26f03371e0b27609238b66d7317f5
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87891888"
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
