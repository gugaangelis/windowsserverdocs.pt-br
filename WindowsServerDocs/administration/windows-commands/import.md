---
title: importar
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7bd78d76-0560-4d47-944c-fe960be2c10b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ddef3958bc431519e3cb89b658a58d1f4dba6938
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835257"
---
# <a name="import"></a>importar



Importa uma cópia de sombra transportáveis de um arquivo de metadados carregados no sistema.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
import
```

## <a name="remarks"></a>Comentários

-   Cópias de sombra transportáveis não são armazenadas no sistema imediatamente. Os detalhes são armazenados em um arquivo XML de documento de componentes de Backup, que o DiskShadow solicita automaticamente e salva em um arquivo de metadados. cab no diretório de trabalho. Você pode alterar o caminho e o nome desse arquivo usando o **definir metadados** comando.
-   Antes de usar **importação**, você deve carregar um arquivo de metadados de DiskShadow usando o **carregar metadados** comando.

## <a name="BKMK_examples"></a>Exemplos

A seguir está um exemplo de script DiskShadow que demonstra o uso de **importação** comando:
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

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)