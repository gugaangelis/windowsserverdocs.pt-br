---
title: importar o DiskPart
description: Artigo de referência para o comando de importação, que importa um grupo de discos externos para o grupo de discos do computador local.
ms.topic: reference
ms.assetid: 4b9d2751-7637-4738-83b0-8c578eb28f27
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1a6bbd2a2adb6017f67667e7e2e71b0f6302b517
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037984"
---
# <a name="import-diskpart"></a>import (diskpart)

Importa um grupo de discos externos para o grupo de discos do computador local. Esse comando importa todos os discos que estão no mesmo grupo que o disco com foco.

> FUNDAMENTAL Antes de usar esse comando, você deve usar o [comando selecionar disco](select-disk.md) para selecionar um disco dinâmico em um grupo de discos externos e deslocar o foco para ele.

## <a name="syntax"></a>Sintaxe

```
import [noerr]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| NOERR | Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |

### <a name="examples"></a>Exemplos

Para importar todos os discos que estão no mesmo grupo de discos que o disco com foco no grupo de discos do computador local, digite:

```
import
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando diskpart](diskpart.md)
