---
title: Bitsadmin getproxylist – recupera a lista de proxy para o trabalho especificado.
description: Tópico de referência para o comando Bitsadmin getproxylist, que recupera a lista de proxy para o trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: eebfa727-d8f1-4ae3-9382-6d8ffe8c3df3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a92703d83cc872204d3dc488c15d703dfd50a780
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717653"
---
# <a name="bitsadmin-getproxylist"></a>bitsadmin getproxylist

Recupera a lista delimitada por vírgulas de servidores proxy a serem usados para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getproxylist <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar a lista de proxy para o trabalho chamado *myDownloadJob*:

```
bitsadmin /getproxylist myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
