---
title: convert
description: Tópico de referência para o comando Convert, que converte um disco de um tipo de disco para outro.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ae151297-af21-4701-bd69-21d775518e03
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ab7189ea774750f8de2ceaecd9511fc8c3a71a97
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720743"
---
# <a name="convert"></a>convert

Converte um disco de um tipo de disco para outro.

## <a name="syntax"></a>Sintaxe

```
convert basic
convert dynamic
convert gpt
convert mbr
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| [converter comando básico](convert-basic.md) | Converte um disco dinâmico vazio em um disco básico. |
| [converter comando dinâmico](convert-dynamic.md) | Converte um disco básico em um disco dinâmico. |
| [converter comando GPT](convert-gpt.md) | Converte um disco básico vazio com o estilo de partição MBR (registro mestre de inicialização) em um disco básico com o estilo de partição GPT (tabela de partição GUID). |
| [comando converter MBR](convert-mbr.md) | Converte um disco básico vazio com o estilo de partição GPT (tabela de partição GUID) em um disco básico com o estilo de partição MBR (registro mestre de inicialização). |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
