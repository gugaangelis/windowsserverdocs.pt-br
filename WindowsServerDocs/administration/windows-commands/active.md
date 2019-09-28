---
title: active
description: Os comandos do Windows tópico para **Active** -on Basic disks, marca a partição com foco como ativa.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1f25da2e-87fc-4392-a7ee-f38d09b7873c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c926bf9b7a583cf7eaa23166e09e6f0a1599e625
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382852"
---
# <a name="active"></a>active



Em discos básicos, marca a partição com foco como ativa.

> [!CAUTION]
> O DiskPart verifica apenas se a partição é capaz de conter os arquivos de inicialização do sistema operacional. O DiskPart não verifica o conteúdo da partição. Se você marcar por engano uma partição como ativa e ela não contiver os arquivos de inicialização do sistema operacional, o computador poderá não ser iniciado.

## <a name="syntax"></a>Sintaxe

```
active
```

## <a name="remarks"></a>Comentários

-   Isso informa o BIOS (sistema básico de entrada/saída) ou a EFI (interface de firmware extensível) que a partição ou o volume é uma partição de sistema ou um volume de sistema válido.
-   Somente partições podem ser marcadas como ativas.
-   Uma partição deve ser selecionada para que essa operação seja realizada com sucesso. Use o comando **selecionar partição** para selecionar uma partição e deslocar o foco para ela.

## <a name="BKMK_examples"></a>Disso

Para marcar a partição com o foco como a partição ativa, digite:
```
active
```

#### <a name="additional-references"></a>Referências adicionais

