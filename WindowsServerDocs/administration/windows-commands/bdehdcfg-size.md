---
title: bdehdcfg size
description: Artigo de referência para o comando de tamanho BdeHdCfg, que especifica o tamanho da partição do sistema quando uma nova unidade do sistema está sendo criada.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 80f55b1d-a28d-4edf-9997-1fb918b7b5a1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 365ed82e90b00189a400725cfcaaec09b0ba3b53
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923423"
---
# <a name="bdehdcfg-size"></a>BdeHdCfg: tamanho

Especifica o tamanho da partição do sistema quando uma nova unidade do sistema está sendo criada. Se você não especificar um tamanho, a ferramenta usará o valor padrão de 300 MB. O tamanho mínimo da unidade de sistema é 100 MB. Se você for armazenar a recuperação do sistema ou outras ferramentas de sistema na partição do sistema, aumente o tamanho de forma correspondente.

> [!NOTE]
> O comando **size** não pode ser combinado com o `target <drive_letter> merge` comando.

## <a name="syntax"></a>Sintaxe

```
bdehdcfg -target {default|unallocated|<drive_letter> shrink} -size <size_in_mb>
```

#### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<size_in_mb>` | Indica o número de megabytes (MB) que deve ser usado para a nova partição. |

## <a name="examples"></a>Exemplos

Para alocar 500 MB para a unidade padrão do sistema:

```
bdehdcfg -target default -size 500
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
