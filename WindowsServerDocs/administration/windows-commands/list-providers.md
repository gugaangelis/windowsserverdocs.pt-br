---
title: list providers
description: Artigo de referência para o comando list providers, que lista os provedores de cópia de sombra que estão atualmente registrados no sistema.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 844b4036-c0b9-449d-8347-7d58ef9bf16d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 64996c7a85fa7ad20a5ffbb1c22ae3396820b676
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931803"
---
# <a name="list-providers"></a>list providers

Lista os provedores de cópia de sombra que estão registrados atualmente no sistema.

## <a name="syntax"></a>Syntax

```
list providers
```

### <a name="examples"></a>Exemplos

Para listar os provedores de cópia de sombra atualmente registrados, digite:

```
list providers
```

Saída semelhante às seguintes exibições:

```
* ProviderID: {b5946137-7b9f-4925-af80-51abd60b20d5}
        Type: [1] VSS_PROV_SYSTEM
        Name: Microsoft Software Shadow Copy provider 1.0
        Version: 1.0.0.7
        CLSID: {65ee1dba-8ff4-4a58-ac1c-3470ee2f376a}
1 provider registered.
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)