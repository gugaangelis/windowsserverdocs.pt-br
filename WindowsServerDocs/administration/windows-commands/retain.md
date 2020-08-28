---
title: retain
description: Artigo de referência para o comando Retain, que prepara um volume dinâmico existente para uso como um volume do sistema ou de inicialização.
ms.topic: reference
ms.assetid: eeab0aef-2ba5-441a-a10d-bbef6f0d7e3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 98c27c62ab7e0ac3320986dde6049be40d10db57
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89036985"
---
# <a name="retain"></a>retain

Prepara um volume dinâmico simples existente para uso como um volume do sistema ou de inicialização. Se você usar um disco dinâmico MBR (registro mestre de inicialização), esse comando criará uma entrada de partição no registro mestre de inicialização. Se você usar um disco dinâmico GPT (tabela de partição GUID), esse comando criará uma entrada de partição na tabela de partição GUID.

## <a name="syntax"></a>Sintaxe

```
retain
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
