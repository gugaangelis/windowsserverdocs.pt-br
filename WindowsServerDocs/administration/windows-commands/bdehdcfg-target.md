---
title: destino BdeHdCfg
description: Tópico de referência para o comando de destino BdeHdCfg, que prepara uma partição para uso como uma unidade do sistema pelo BitLocker e pela recuperação do Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f761d25d-8349-4ac7-ac46-6bb340a4348f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b7f98f42675a49ab34ca1cf759efb9d40a69c38a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718593"
---
# <a name="bdehdcfg-target"></a>BdeHdCfg: destino

Prepara uma partição para uso como uma unidade do sistema pelo BitLocker e pela recuperação do Windows. Por padrão, essa partição é criada sem uma letra de unidade.

## <a name="syntax"></a>Sintaxe

```
bdehdcfg -target {default|unallocated|<drive_letter> shrink|<drive_letter> merge}
```

#### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| default | Indica que a ferramenta da linha de comando seguirá o mesmo processo que o assistente de instalação do BitLocker. |
| unallocated | Cria a partição do sistema fora do espaço não alocado disponível no disco. |
| `<drive_letter>`PodeReduzir | Reduz a unidade especificada pela quantidade necessária para criar uma partição do sistema ativa. Para usar esse comando, a unidade especificada deve ter pelo menos 5% de espaço livre. |
| `<drive_letter>`Mescle | Usa a unidade especificada como partição do sistema ativa. A unidade do sistema operacional não pode ser um destino para mesclagem. |

## <a name="examples"></a>Exemplos

Para designar uma unidade existente (P) como a unidade do sistema:

```
bdehdcfg -target P: merge
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
