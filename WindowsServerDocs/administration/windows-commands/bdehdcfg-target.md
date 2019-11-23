---
title: destino BdeHdCfg
description: Tópico de comandos do Windows para BdeHdCfg Target – prepara uma partição para uso como uma unidade do sistema pelo BitLocker e pela recuperação do Windows.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 2fb0a1daa257ef2c9f1cd77b88e5ef14f84a0dfa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382183"
---
# <a name="bdehdcfg-target"></a>BdeHdCfg: destino



Prepara uma partição para uso como uma unidade do sistema pelo BitLocker e pela recuperação do Windows. Por padrão, essa partição é criada sem uma letra de unidade. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge}
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|padrão|Indica que a ferramenta da linha de comando seguirá o mesmo processo que o assistente de instalação do BitLocker.|
|unallocated|Cria a partição do sistema fora do espaço não alocado disponível no disco.|
|\<DriveLetter > reduzir|Reduz a unidade especificada pela quantidade necessária para criar uma partição do sistema ativa. Para usar esse comando, a unidade especificada deve ter pelo menos 5% de espaço livre.|
|\<DriveLetter > mesclagem|Usa a unidade especificada como partição do sistema ativa. A unidade do sistema operacional não pode ser um destino para mesclagem.|

## <a name="BKMK_Examples"></a>Disso

O exemplo a seguir descreve o uso do comando **target** para designar uma unidade existente (P) como a unidade do sistema.
```
bdehdcfg -target P: merge
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [BdeHdCfg](bdehdcfg.md)