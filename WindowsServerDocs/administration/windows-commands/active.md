---
title: ativo
description: Artigo de referência para o comando ativo, que, em discos básicos, marca a partição com foco como ativa.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1f25da2e-87fc-4392-a7ee-f38d09b7873c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a5df2f67c087be31190c512be0f6b20d8a1d72cb
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85924144"
---
# <a name="active"></a>ativo

Em discos básicos, marca a partição com foco como ativa. Somente partições podem ser marcadas como ativas. Uma partição deve ser selecionada para que essa operação seja realizada com sucesso. Use o comando **selecionar partição** para selecionar uma partição e deslocar o foco para ela.

> [!CAUTION]
> O DiskPart apenas informa o BIOS (sistema básico de entrada/saída) ou a EFI (interface de firmware extensível) que a partição ou o volume é uma partição de sistema ou volume de sistema válido e que é capaz de conter os arquivos de inicialização do sistema operacional. O DiskPart não verifica o conteúdo da partição. Se você marcar por engano uma partição como ativa e ela não contiver os arquivos de inicialização do sistema operacional, o computador poderá não ser iniciado.

## <a name="syntax"></a>Syntax

```
active
```

## <a name="examples"></a>Exemplos

Para marcar a partição com o foco como a partição ativa, digite:

```
active
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Selecionar comando de partição](select-partition.md)
