---
title: bitsadmin rawreturn
description: Artigo de referência para o comando Bitsadmin rawreturn, que retorna dados adequados para análise.
ms.topic: reference
ms.assetid: bbe97130-26f6-4cdd-84f1-baf530ce38b7
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 286e84c16087cc000a6af29b3be53d529425dfde
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631212"
---
# <a name="bitsadmin-rawreturn"></a>bitsadmin rawreturn

Retorna os dados adequados para análise. Normalmente, você usa esse comando em conjunto com as opções **/Create** e **/Get*** para receber apenas o valor. Você deve especificar essa opção antes de outras opções.

> [!NOTE]
> Esse comando retira os caracteres de nova linha e a formatação da saída.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /rawreturn
```

## <a name="examples"></a>Exemplos

Para recuperar os dados brutos para o estado do trabalho chamado *myDownloadJob*:

```
bitsadmin /rawreturn /getstate myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
