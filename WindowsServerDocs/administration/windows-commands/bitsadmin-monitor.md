---
title: bitsadmin monitor
description: Artigo de referência do comando monitor Bitsadmin, que monitora os trabalhos na fila de transferência que pertencem ao usuário atual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2c424d27-e011-49c2-b579-a2c235467c39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7ce08eccf46fc17086d216bc6797bec451ace7eb
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926492"
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
| /Refresh | Opcional. Atualiza os dados em um intervalo especificado por `<seconds>` . O intervalo de atualização padrão é cinco segundos. Para interromper a atualização, pressione CTRL + C. |

## <a name="examples"></a>Exemplos

Para monitorar a fila de transferência para trabalhos pertencentes ao usuário atual e atualiza as informações a cada 60 segundos.

```
bitsadmin /monitor /refresh 60
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
