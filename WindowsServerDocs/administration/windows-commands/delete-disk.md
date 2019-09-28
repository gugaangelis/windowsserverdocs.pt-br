---
title: excluir disco
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 44079900-e4ed-49d0-81e4-d652c38cd636
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 886e84edf80227d42ad77e36aed388b9e853ca62
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378672"
---
# <a name="delete-disk"></a>excluir disco



Exclui um disco dinâmico ausente da lista de discos.

Para obter instruções sobre como usar esse comando, consulte [remover um disco dinâmico ausente](https://go.microsoft.com/fwlink/?LinkId=207055) (https://go.microsoft.com/fwlink/?LinkId=207055).

## <a name="syntax"></a>Sintaxe

```
delete disk [noerr] [override]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|NOERR|Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro.|
|substituição|Permite que o DiskPart exclua todos os volumes simples no disco. Se o disco contiver metade de um volume espelhado, a metade do espelho no disco será excluída. O comando excluir substituição de disco falhará se o disco for membro de um volume RAID-5.|

## <a name="BKMK_examples"></a>Disso

Para excluir um disco dinâmico ausente da lista de discos, digite:
```
delete disk
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)

