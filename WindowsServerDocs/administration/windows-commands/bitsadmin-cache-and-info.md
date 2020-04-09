---
title: cache Bitsadmin e informações
description: Tópico de comandos do Windows para **Bitsadmin cache e info**, que despeja uma entrada de cache específica.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 15975cbf-dba6-49ca-a725-d15ce1952de5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3e9c6ce1eb972a76408483b8a27a3abca5500e56
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850889"
---
# <a name="bitsadmin-cache-and-info"></a>cache Bitsadmin e informações

Despeja uma entrada de cache específica.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /cache /info recordID [/verbose]
```

### <a name="parameters"></a>Parâmetros

| Paramreter | Descrição |
| -------------- | -------------- |
| recordID | O GUID associado à entrada de cache. |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir despeja a entrada de cache com o valor recordId de {6511FB02-E195-40A2-B595-E8E2F8F47702}.

```
C:\>bitsadmin /cache /info {6511FB02-E195-40A2-B595-E8E2F8F47702}
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)