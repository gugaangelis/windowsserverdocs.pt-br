---
title: listar provedores
description: Tópico de referência para o comando list providers, que lista os provedores de cópia de sombra que estão atualmente registrados no sistema.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 844b4036-c0b9-449d-8347-7d58ef9bf16d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 98615dfa92c24b91babb55ae3545065834887e5d
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2020
ms.locfileid: "83817236"
---
# <a name="list-providers"></a>listar provedores

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