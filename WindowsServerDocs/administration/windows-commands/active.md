---
title: ativo
description: Artigo de referência para o comando ativo, que, em discos básicos, marca a partição com foco como ativa.
ms.topic: reference
ms.assetid: 1f25da2e-87fc-4392-a7ee-f38d09b7873c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6baad8bd60307eeddf7b777f059ab5ba3846e1d4
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89029467"
---
# <a name="active"></a>ativo

Em discos básicos, marca a partição com foco como ativa. Somente partições podem ser marcadas como ativas. Uma partição deve ser selecionada para que essa operação seja realizada com sucesso. Use o comando **selecionar partição** para selecionar uma partição e deslocar o foco para ela.

> [!CAUTION]
> O DiskPart apenas informa o BIOS (sistema básico de entrada/saída) ou a EFI (interface de firmware extensível) que a partição ou o volume é uma partição de sistema ou volume de sistema válido e que é capaz de conter os arquivos de inicialização do sistema operacional. O DiskPart não verifica o conteúdo da partição. Se você marcar por engano uma partição como ativa e ela não contiver os arquivos de inicialização do sistema operacional, o computador poderá não ser iniciado.

## <a name="syntax"></a>Sintaxe

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
