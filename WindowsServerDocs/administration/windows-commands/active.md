---
title: ativo
description: Artigo de referência para o comando ativo, que, em discos básicos, marca a partição com foco como ativa.
ms.topic: article
ms.assetid: 1f25da2e-87fc-4392-a7ee-f38d09b7873c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 60bc5d3034d576d959322d419bfe7c1937da015d
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87895611"
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
