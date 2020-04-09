---
title: bitsadmin monitor
description: Tópico de comandos do Windows para o **Monitor Bitsadmin**, que monitora os trabalhos na fila de transferência que pertencem ao usuário atual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2c424d27-e011-49c2-b579-a2c235467c39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bda268afd5fda24bba2afb04b32bac9cda9a05bb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850209"
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

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir monitora a fila de transferência de trabalhos pertencentes ao usuário atual e atualiza as informações a cada 60 segundos.

```
C:\>bitsadmin /monitor /refresh 60
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)