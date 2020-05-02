---
title: volume de detalhes
description: Tópico de referência para o volume de detalhes, que exibe os discos nos quais o volume atual reside.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 38f2bc75-2ed6-4e80-aa74-ab83133db1cd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2958c82b1dfc3b99d0e15690ef9857e7d83b244f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719615"
---
# <a name="detail-volume"></a>volume de detalhes

Exibe os discos nos quais o volume atual reside.

## <a name="syntax"></a>Sintaxe

```
detail volume
```

## <a name="remarks"></a>Comentários

-   Um volume deve ser selecionado para que essa operação seja realizada com sucesso. Use o comando **selecionar volume** para selecionar um volume e deslocar o foco para ele.
-   Os detalhes do volume não são aplicáveis a volumes somente leitura, como uma unidade de DVD-ROM ou CD-ROM.

## <a name="examples"></a>Exemplos

Para ver todos os discos nos quais reside o volume atual, digite:
```
detail volume
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

