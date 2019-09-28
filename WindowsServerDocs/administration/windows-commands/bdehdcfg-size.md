---
title: tamanho do BdeHdCfg
description: Tópico de comandos do Windows – especifica o tamanho da partição do sistema quando uma nova unidade do sistema está sendo criada.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 80f55b1d-a28d-4edf-9997-1fb918b7b5a1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6ec42cdb5716c63c7210ea6cfde8ce8884833b45
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382203"
---
# <a name="bdehdcfg-size"></a>BdeHdCfg: tamanho



Especifica o tamanho da partição do sistema quando uma nova unidade do sistema está sendo criada. Para obter um exemplo de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink} -size <SizeinMB>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<SizeinMB >|Indica o número de megabytes (MB) que deve ser usado para a nova partição.|

## <a name="remarks"></a>Comentários

Se você não especificar um tamanho, a ferramenta usará o valor padrão de 300 MB. O tamanho mínimo da unidade de sistema é 100 MB. Se você for armazenar a recuperação do sistema ou outras ferramentas de sistema na partição do sistema, aumente o tamanho de forma correspondente.

> [!NOTE]
> O comando **size** não pode ser combinado com o comando \<DriveLetter > **Merge** de **destino** .

## <a name="BKMK_Examples"></a>Disso

O exemplo a seguir ilustra o uso do comando **size** para alocar 500 MB para a unidade padrão do sistema.
```
bdehdcfg -target default -size 500
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [BdeHdCfg](bdehdcfg.md)