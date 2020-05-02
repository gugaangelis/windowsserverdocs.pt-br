---
title: bitsadmin cancel
description: Tópico de referência para o comando Bitsadmin Cancel, que remove o trabalho da fila de transferência e exclui todos os arquivos temporários associados ao trabalho.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7374b544-6a16-4d3e-872c-dcf4c02ad89d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 95fefbc4a9731c2ccbac22adc27f8231a7f36138
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718260"
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
