---
title: bitsadmin cancel
description: Artigo de referência para o comando Bitsadmin Cancel, que remove o trabalho da fila de transferência e exclui todos os arquivos temporários associados ao trabalho.
ms.topic: reference
ms.assetid: 7374b544-6a16-4d3e-872c-dcf4c02ad89d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4ef8810b04141b41851f029f6cde4586b89a90d4
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632427"
---
# <a name="bitsadmin-cancel"></a>bitsadmin cancel

Remove o trabalho da fila de transferência e exclui todos os arquivos temporários associados ao trabalho.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /cancel <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para remover o trabalho *myDownloadJob* da fila de transferência:

```
bitsadmin /cancel myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
