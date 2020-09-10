---
title: bitsadmin getbytestotal
description: Artigo de referência para o comando Bitsadmin getbytestotal, que recupera o tamanho do trabalho especificado.
ms.topic: reference
ms.assetid: 784e0bfa-7b09-4262-9104-adbc9beb479b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 50b926b8d89e402ef4fd9e58896963edd3c368c9
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632343"
---
# <a name="bitsadmin-getbytestotal"></a>bitsadmin getbytestotal

Recupera o tamanho do trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getbytestotal <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar o tamanho do trabalho chamado *myDownloadJob*:

```
bitsadmin /getbytestotal myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
