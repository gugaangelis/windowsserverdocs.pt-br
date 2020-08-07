---
title: importar o DiskShadow
description: Artigo de referência para o comando de importação, que importa uma cópia de sombra transportável de um arquivo de metadados carregado para o sistema.
ms.topic: article
ms.assetid: 7bd78d76-0560-4d47-944c-fe960be2c10b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4d0d76c9565904d6e24c41f4c728bf43061f5040
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87888365"
---
# <a name="import-diskshadow"></a>importar (DiskShadow)

Importa uma cópia de sombra transportável de um arquivo de metadados carregado para o sistema.

> FUNDAMENTAL Antes de usar esse comando, você deve usar o [comando carregar metadados](load-metadata.md) para carregar um arquivo de metadados do DiskShadow.

## <a name="syntax"></a>Sintaxe

```
import
```

#### <a name="remarks"></a>Comentários

- As cópias de sombra transportáveis não são armazenadas imediatamente no sistema. Seus detalhes são armazenados em um arquivo XML de documentos de componentes de backup, que o DiskShadow solicita e salva automaticamente em um arquivo de metadados. cab no diretório de trabalho. Use o [comando Set Metadata](set-metadata.md) para alterar o caminho e o nome desse arquivo XML.

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

- [comando DiskShadow](diskshadow.md)