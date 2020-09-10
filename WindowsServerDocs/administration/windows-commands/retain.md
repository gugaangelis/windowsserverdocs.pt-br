---
title: retain
description: Artigo de referência para o comando Retain, que prepara um volume dinâmico existente para uso como um volume do sistema ou de inicialização.
ms.topic: reference
ms.assetid: eeab0aef-2ba5-441a-a10d-bbef6f0d7e3e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 201aa8b79f8ac0f89d44be7db3f84c9f3059e206
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89626902"
---
# <a name="retain"></a>retain

Prepara um volume dinâmico simples existente para uso como um volume do sistema ou de inicialização. Se você usar um disco dinâmico MBR (registro mestre de inicialização), esse comando criará uma entrada de partição no registro mestre de inicialização. Se você usar um disco dinâmico GPT (tabela de partição GUID), esse comando criará uma entrada de partição na tabela de partição GUID.

## <a name="syntax"></a>Sintaxe

```
retain
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
