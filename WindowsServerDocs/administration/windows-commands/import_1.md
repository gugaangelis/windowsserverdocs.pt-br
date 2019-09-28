---
title: importar
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4b9d2751-7637-4738-83b0-8c578eb28f27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ca0a15e73e4aa913ece34e083a8070be3b4b190d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375409"
---
# <a name="import"></a>importar



Importa um grupo de discos externos para o grupo de discos do computador local.

## <a name="syntax"></a>Sintaxe

```
import [noerr]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|NOERR|Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro.|

## <a name="remarks"></a>Comentários

-   O comando importar importa todos os discos que estão no mesmo grupo que o disco com foco.
-   Um disco dinâmico em um grupo de discos externos deve ser selecionado para que essa operação seja realizada com sucesso. Use o comando **selecionar disco** para selecionar um disco e deslocar o foco para ele.

## <a name="BKMK_examples"></a>Disso

Para importar todos os discos que estão no mesmo grupo de discos que o disco com foco no grupo de discos do computador local, digite:
```
import
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)

