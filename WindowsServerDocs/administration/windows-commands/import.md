---
title: importar
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7bd78d76-0560-4d47-944c-fe960be2c10b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 72cbd6195de64a6a0a7f2c258e19b2d5eb1378b1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724851"
---
# <a name="import"></a>importar



Importa uma cópia de sombra transportável de um arquivo de metadados carregado para o sistema.



## <a name="syntax"></a>Sintaxe

```
import
```

## <a name="remarks"></a>Comentários

-   As cópias de sombra transportáveis não são armazenadas imediatamente no sistema. Seus detalhes são armazenados em um arquivo XML de documentos de componentes de backup, que o DiskShadow solicita e salva automaticamente em um arquivo de metadados. cab no diretório de trabalho. Você pode alterar o caminho e o nome desse arquivo usando o comando **set Metadata** .
-   Antes de poder usar a **importação**, você deve carregar um arquivo de metadados do DiskShadow usando o comando **carregar metadados** .

## <a name="examples"></a>Exemplos

Veja a seguir um exemplo de script de DiskShadow que demonstra o uso do comando de **importação** :
```
#Sample DiskShadow script demonstrating IMPORT
SET CONTEXT PERSISTENT
SET CONTEXT TRANSPORTABLE
SET METADATA transHWshadow_p.cab
#P: is the volume supported by the Hardware Shadow Copy provider
ADD VOLUME P:
CREATE
END BACKUP
#The (transportable) shadow copy is not in the system yet.
#You can reset or exit now if you wish.

LOAD METADATA transHWshadow_p.cab
IMPORT
#The shadow copy will now be loaded into the system.
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)