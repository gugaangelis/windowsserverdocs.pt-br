---
title: list providers
description: Artigo de referência para o comando list providers, que lista os provedores de cópia de sombra que estão atualmente registrados no sistema.
ms.topic: reference
ms.assetid: 844b4036-c0b9-449d-8347-7d58ef9bf16d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9c6f62c852ea41a9cc355afcaaaf083e68ad1d7a
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639568"
---
# <a name="list-providers"></a>list providers

Lista os provedores de cópia de sombra que estão registrados atualmente no sistema.

## <a name="syntax"></a>Sintaxe

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