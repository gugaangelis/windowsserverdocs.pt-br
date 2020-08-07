---
title: bitsadmin monitor
description: Artigo de referência do comando monitor Bitsadmin, que monitora os trabalhos na fila de transferência que pertencem ao usuário atual.
ms.topic: article
ms.assetid: 2c424d27-e011-49c2-b579-a2c235467c39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3ba2cb315fb0696b8363506669aa41f1693bb758
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893667"
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
