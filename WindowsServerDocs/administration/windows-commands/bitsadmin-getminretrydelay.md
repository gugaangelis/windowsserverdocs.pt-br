---
title: bitsadmin getminretrydelay
description: Artigo de referência para o comando Bitsadmin getminretrydelay, que recupera o período de tempo, em segundos, que o serviço aguarda depois de encontrar um erro transitório antes de tentar transferir o arquivo.
ms.topic: reference
ms.assetid: 54f0abab-c129-40ed-a603-50f464d26011
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1f5bc6d69e7dbc46bc7e0df3a34ac97f37fda252
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89030304"
---
# <a name="bitsadmin-getminretrydelay"></a>bitsadmin getminretrydelay

Recupera o período de tempo, em segundos, que o serviço aguardará depois de encontrar um erro transitório antes de tentar transferir o arquivo.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getminretrydelay <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar o atraso mínimo de repetição para o trabalho chamado *myDownloadJob*:

```
bitsadmin /getminretrydelay myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
