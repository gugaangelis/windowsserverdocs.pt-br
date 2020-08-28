---
title: bitsadmin cancel
description: Artigo de referência para o comando Bitsadmin Cancel, que remove o trabalho da fila de transferência e exclui todos os arquivos temporários associados ao trabalho.
ms.topic: reference
ms.assetid: 7374b544-6a16-4d3e-872c-dcf4c02ad89d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: da7a858603875affc7acc8bcb60d02451a641690
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024430"
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
