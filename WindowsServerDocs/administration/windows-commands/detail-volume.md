---
title: volume de detalhes
description: O tópico de comandos do Windows para obter detalhes sobre o volume, que exibe os discos nos quais reside o volume atual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 38f2bc75-2ed6-4e80-aa74-ab83133db1cd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1f0441beba769066c593e77b55b9266918e5f778
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80846310"
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

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para ver todos os discos nos quais reside o volume atual, digite:
```
detail volume
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

