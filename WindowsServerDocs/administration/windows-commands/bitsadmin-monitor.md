---
title: bitsadmin monitor
description: Tópico de referência para o comando Bitsadmin monitor, que monitora os trabalhos na fila de transferência que pertencem ao usuário atual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2c424d27-e011-49c2-b579-a2c235467c39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4c8fa52f9fcf30a66b41c9cdbf7b7e1fab69f06e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717372"
---
# <a name="bitsadmin-monitor"></a>bitsadmin monitor

Monitora trabalhos na fila de transferência que pertencem ao usuário atual.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /monitor [/allusers] [/refresh <seconds>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| /allusers | Opcional. Monitora trabalhos para todos os usuários. Você deve ter privilégios de administrador para usar esse parâmetro. |
| /Refresh | Opcional. Atualiza os dados em um intervalo especificado por `<seconds>`. O intervalo de atualização padrão é cinco segundos. Para interromper a atualização, pressione CTRL + C. |

## <a name="examples"></a>Exemplos

Para monitorar a fila de transferência para trabalhos pertencentes ao usuário atual e atualiza as informações a cada 60 segundos.

```
bitsadmin /monitor /refresh 60
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
