---
title: bitsadmin getpeercachingflags
description: Artigo de referência para o comando Bitsadmin getpeercachingflags, que recupera sinalizadores que determinam se os arquivos do trabalho podem ser armazenados em cache e servidos para os pares e se o BITS pode baixar conteúdo para o trabalho de pares.
ms.topic: reference
ms.assetid: 3c3c9f28-4c04-4c49-a23a-dee5bbcc8981
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6ec765183543bf42152d198c10ebc5debfa81edb
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631843"
---
# <a name="bitsadmin-getpeercachingflags"></a>bitsadmin getpeercachingflags

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera sinalizadores que determinam se os arquivos do trabalho podem ser armazenados em cache e servidos para os pares e se o BITS pode baixar conteúdo para o trabalho de pares.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getpeercachingflags <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar os sinalizadores para o trabalho chamado *myDownloadJob*:

```
bitsadmin /getpeercachingflags myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
