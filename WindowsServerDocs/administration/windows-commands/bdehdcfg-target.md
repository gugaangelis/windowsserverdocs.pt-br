---
title: bdehdcfg target
description: Tópico de comandos do Windows para o destino de bdehdcfg - prepara uma partição para uso como uma unidade de sistema pela recuperação do BitLocker e Windows.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f761d25d-8349-4ac7-ac46-6bb340a4348f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8d180974f480b4c40532dab529ad49dcc33540d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881527"
---
# <a name="bdehdcfg-target"></a>bdehdcfg: target



Prepara uma partição para uso como uma unidade de sistema do BitLocker e a recuperação do Windows. Por padrão, essa partição é criada sem uma letra de unidade. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge}
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|padrão|Indica que a ferramenta da linha de comando seguirá o mesmo processo que o assistente de instalação do BitLocker.|
|unallocated|Cria a partição do sistema fora do espaço não alocado disponível no disco.|
|\<Letra da unidade > reduzir|Reduz a unidade especificada pela quantidade necessária para criar uma partição do sistema ativa. Para usar esse comando, a unidade especificada deve ter pelo menos 5% de espaço livre.|
|\<Letra da unidade > direta|Usa a unidade especificada como partição do sistema ativa. A unidade do sistema operacional não pode ser um destino para mesclagem.|

## <a name="BKMK_Examples"></a>Exemplos

O exemplo a seguir ilustra o uso de **destino** comando para designar uma unidade existente (P) como a unidade do sistema.
```
bdehdcfg -target P: merge
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)