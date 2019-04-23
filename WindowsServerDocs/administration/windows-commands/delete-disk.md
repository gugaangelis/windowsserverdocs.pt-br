---
title: Excluir disco
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: e401133854118e82a45fd5e01466288ae41f67da
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863907"
---
# <a name="delete-disk"></a>Excluir disco



Exclui um disco dinâmico ausente da lista de discos.

Para obter instruções sobre como usar esse comando, consulte [remover um disco dinâmico ausente](https://go.microsoft.com/fwlink/?LinkId=207055) (https://go.microsoft.com/fwlink/?LinkId=207055).

## <a name="syntax"></a>Sintaxe

```
delete disk [noerr] [override]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|noerr|Somente para scripts. Quando um erro for encontrado, o DiskPart continua a processar comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro causar o DiskPart sair com um código de erro.|
|Substituir|Permite que o DiskPart excluir todos os volumes simples no disco. Se o disco contiver metade de um volume espelhado, a metade do espelho no disco é excluída. O comando de substituição do disco de exclusão falhará se o disco for um membro de um volume RAID-5.|

## <a name="BKMK_examples"></a>Exemplos

Para excluir um disco dinâmico ausente na lista de discos, digite:
```
delete disk
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)

