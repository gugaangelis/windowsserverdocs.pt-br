---
title: tamanho de bdehdcfg
description: Tópico de comandos do Windows - Especifica o tamanho da partição do sistema quando uma nova unidade do sistema está sendo criada.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d024bb4092f93782300d6afb9053cee1da32629a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817517"
---
# <a name="bdehdcfg-size"></a>bdehdcfg: size



Especifica o tamanho da partição do sistema quando uma nova unidade do sistema está sendo criada. Para obter um exemplo de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink} -size <SizeinMB>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<SizeinMB>|Indica o número de megabytes (MB) que deve ser usado para a nova partição.|

## <a name="remarks"></a>Comentários

Se você não especificar um tamanho, a ferramenta usará o valor padrão de 300 MB. O tamanho mínimo da unidade de sistema é 100 MB. Se você for armazenar a recuperação do sistema ou outras ferramentas de sistema na partição do sistema, aumente o tamanho de forma correspondente.

> [!NOTE]
> O **tamanho** comando não pode ser combinado com o **destino** \<DriveLetter > **mesclagem** comando.

## <a name="BKMK_Examples"></a>Exemplos

O exemplo a seguir ilustra o uso de **tamanho** comando alocar 500 MB para a unidade do sistema padrão.
```
bdehdcfg -target default -size 500
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
-   [Bdehdcfg](bdehdcfg.md)