---
title: disco de detalhes
description: Tópico de referência para o disco de detalhes, que exibe as propriedades do disco selecionado e os volumes nesse disco.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6b09cf40-8d93-452b-b449-5242e62a4102
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a746506d6c9609e3214dbd48e5fa91f52d16ab4d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82710514"
---
# <a name="detail-disk"></a>disco de detalhes

Exibe as propriedades do disco selecionado e os volumes existentes nele.

## <a name="syntax"></a>Sintaxe

```
detail disk
```

## <a name="remarks"></a>Comentários

-   Um disco deve ser selecionado para que essa operação tenha sucesso. Use o comando **selecionar disco** para selecionar um disco e deslocar o foco para ele.
-   Se o disco selecionado for um VHD (disco rígido virtual), o **disco de detalhes** relatará o tipo de barramento do disco como virtual.

## <a name="examples"></a>Exemplos

Para ver as propriedades do disco selecionado e informações sobre os volumes no disco, digite:
```
detail disk
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

