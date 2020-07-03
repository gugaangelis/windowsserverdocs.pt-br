---
title: convert
description: Artigo de referência para o comando Convert, que converte um disco de um tipo de disco para outro.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ae151297-af21-4701-bd69-21d775518e03
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f93f2b16838a6f54af3f28b7e0883808a6cd013a
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85928900"
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
