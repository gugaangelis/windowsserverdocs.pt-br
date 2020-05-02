---
title: bitsadmin getminretrydelay
description: Tópico de referência para o comando Bitsadmin getminretrydelay, que recupera o período de tempo, em segundos, que o serviço aguarda depois de encontrar um erro transitório antes de tentar transferir o arquivo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 54f0abab-c129-40ed-a603-50f464d26011
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a50b9d98fe0b873dc58b8e86dc672a8f4157208a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717842"
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
