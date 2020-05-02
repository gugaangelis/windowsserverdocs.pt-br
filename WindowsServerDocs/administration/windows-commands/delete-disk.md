---
title: excluir disco
description: Tópico de referência para excluir o disco, que exclui um disco dinâmico ausente da lista de discos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 44079900-e4ed-49d0-81e4-d652c38cd636
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ad4888835c0bb1862344f104099b8b59027d1de9
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716760"
---
# <a name="delete-disk"></a>excluir disco

Exclui um disco dinâmico ausente da lista de discos.

Para obter instruções sobre como usar esse comando, consulte [remover um disco dinâmico ausente](https://go.microsoft.com/fwlink/?LinkId=207055) (https://go.microsoft.com/fwlink/?LinkId=207055).

## <a name="syntax"></a>Sintaxe

```
delete disk [noerr] [override]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|NOERR|Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro.|
|override|Permite que o DiskPart exclua todos os volumes simples no disco. Se o disco contiver metade de um volume espelhado, a metade do espelho no disco será excluída. O comando excluir substituição de disco falhará se o disco for membro de um volume RAID-5.|

## <a name="examples"></a>Exemplos

Para excluir um disco dinâmico ausente da lista de discos, digite:
```
delete disk
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

