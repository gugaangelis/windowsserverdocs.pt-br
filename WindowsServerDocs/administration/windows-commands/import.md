---
title: importar
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7bd78d76-0560-4d47-944c-fe960be2c10b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 07fdd03c73c454e92218a4c6983eac7f29b50883
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842169"
---
# <a name="import"></a>importar



importa uma cópia de sombra transportável de um arquivo de metadados carregado para o sistema.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
import
```

## <a name="remarks"></a>Comentários

-   As cópias de sombra transportáveis não são armazenadas imediatamente no sistema. Seus detalhes são armazenados em um arquivo XML de documentos de componentes de backup, que o DiskShadow solicita e salva automaticamente em um arquivo de metadados. cab no diretório de trabalho. Você pode alterar o caminho e o nome desse arquivo usando o comando **set Metadata** .
-   Antes de poder usar a **importação**, você deve carregar um arquivo de metadados do DiskShadow usando o comando **carregar metadados** .

## <a name="examples"></a><a name=BKMK_examples></a>Disso

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