---
title: active
description: Tópico de comandos do Windows para **active** – em discos básicos, marca a partição com foco como ativa.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 3a039e0200fb84d446739ac7017556b6c302f4af
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868757"
---
# <a name="active"></a>active



Em discos básicos, marca a partição com foco como ativa.

> [!CAUTION]
> DiskPart verifica somente se a partição é capaz de conter os arquivos de inicialização do sistema operacional. DiskPart não verifica o conteúdo da partição. Se você marcar por engano uma partição como ativa e não contém os arquivos de inicialização do sistema operacional, o computador talvez não seja iniciado.

## <a name="syntax"></a>Sintaxe

```
active
```

## <a name="remarks"></a>Comentários

-   Isso informa ao sistema básico de entrada/saída (BIOS) ou a Interface de Firmware extensível (EFI) que a partição ou volume é uma partição de sistema válida ou o volume do sistema.
-   Somente partições podem ser marcada como ativa.
-   Uma partição deve ser selecionada para essa operação seja bem-sucedida. Use o **Selecionar partição** comando para selecionar uma partição e mudar o foco a ele.

## <a name="BKMK_examples"></a>Exemplos

Para marcar a partição com foco como partição ativa, digite:
```
active
```

#### <a name="additional-references"></a>Referências adicionais

