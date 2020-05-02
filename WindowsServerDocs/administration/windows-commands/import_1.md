---
title: importar
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4b9d2751-7637-4738-83b0-8c578eb28f27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 569d986c57ae8b3d7253c050146ac0583c7c92df
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724841"
---
# <a name="import"></a>importar



Importa um grupo de discos externos para o grupo de discos do computador local.

## <a name="syntax"></a>Sintaxe

```
import [noerr]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|NOERR|Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro.|

## <a name="remarks"></a>Comentários

-   O comando importar importa todos os discos que estão no mesmo grupo que o disco com foco.
-   Um disco dinâmico em um grupo de discos externos deve ser selecionado para que essa operação seja realizada com sucesso. Use o comando **selecionar disco** para selecionar um disco e deslocar o foco para ele.

## <a name="examples"></a>Exemplos

Para importar todos os discos que estão no mesmo grupo de discos que o disco com foco no grupo de discos do computador local, digite:
```
import
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

