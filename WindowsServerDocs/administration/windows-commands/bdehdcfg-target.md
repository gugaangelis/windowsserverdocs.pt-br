---
title: destino BdeHdCfg
description: Tópico de comandos do Windows para **destino BdeHdCfg**, que prepara uma partição para uso como uma unidade do sistema pelo BitLocker e pela recuperação do Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f761d25d-8349-4ac7-ac46-6bb340a4348f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d0f3e90fbb8725360cf8db335e79721e2328ab3a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851009"
---
# <a name="bdehdcfg-target"></a>BdeHdCfg: destino

Prepara uma partição para uso como uma unidade do sistema pelo BitLocker e pela recuperação do Windows. Por padrão, essa partição é criada sem uma letra de unidade.

Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge}
```

#### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| {1&gt;default&lt;1} | Indica que a ferramenta da linha de comando seguirá o mesmo processo que o assistente de instalação do BitLocker. |
| unallocated | Cria a partição do sistema fora do espaço não alocado disponível no disco. |
| redução de `<DriveLetter>` | Reduz a unidade especificada pela quantidade necessária para criar uma partição do sistema ativa. Para usar esse comando, a unidade especificada deve ter pelo menos 5% de espaço livre. |
| mesclagem de `<DriveLetter>` | Usa a unidade especificada como partição do sistema ativa. A unidade do sistema operacional não pode ser um destino para mesclagem. |

## <a name="examples"></a><a name=BKMK_Examples></a>Disso

O exemplo a seguir descreve o uso do comando **target** para designar uma unidade existente (P) como a unidade do sistema.

```
bdehdcfg -target P: merge
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [BdeHdCfg](bdehdcfg.md)