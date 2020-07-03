---
title: importar o DiskShadow
description: Artigo de referência para o comando de importação, que importa uma cópia de sombra transportável de um arquivo de metadados carregado para o sistema.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7bd78d76-0560-4d47-944c-fe960be2c10b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ab1c3c6d324cec939a2529191cbc8ce40165b807
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85924446"
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