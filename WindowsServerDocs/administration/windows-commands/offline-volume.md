---
title: offline volume
description: Artigo de referência para o comando de volume offline, que leva o volume online com foco para o estado offline.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b8f7192f-ea38-47d0-9d4e-58ef68336ae6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9f64a6924b0033b0e7889ccbcab4fb142a4f7c05
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85936732"
---
# <a name="offline-volume"></a>offline volume

Coloca o volume online com foco no estado offline.

> [!NOTE]
> Um volume deve ser selecionado para que o comando de **volume offline** seja executado com sucesso. Use o comando [selecionar volume](select-volume.md) para selecionar um disco e deslocar o foco para ele.

## <a name="syntax"></a>Sintaxe

```
offline volume [noerr]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| NOERR | Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |

### <a name="examples"></a>Exemplos

Para colocar o disco com foco offline, digite:

```
offline volume
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
