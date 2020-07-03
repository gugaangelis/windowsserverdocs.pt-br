---
title: detail disk
description: Artigo de referência para o comando de disco de detalhes, que exibe as propriedades do disco selecionado e os volumes nesse disco.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6b09cf40-8d93-452b-b449-5242e62a4102
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5bb5ec5a51f16aecf1b8ee35c78736f1791d3106
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85929466"
---
# <a name="detail-disk"></a>detail disk

Exibe as propriedades do disco selecionado e os volumes existentes nele. Antes de começar, você deve selecionar um disco para que essa operação seja realizada com sucesso. Use o comando [selecionar disco](select-disk.md) para selecionar um disco e deslocar o foco para ele. Se você selecionar um VHD (disco rígido virtual), esse comando mostrará o tipo de barramento do disco como *virtual*.

## <a name="syntax"></a>Syntax

```
detail disk
```

## <a name="examples"></a>Exemplos

Para ver as propriedades do disco selecionado e informações sobre os volumes no disco, digite:

```
detail disk
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando de detalhe](detail.md)
