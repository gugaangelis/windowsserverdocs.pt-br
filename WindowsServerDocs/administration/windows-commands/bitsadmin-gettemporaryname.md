---
title: bitsadmin gettemporaryname
description: Tópico de referência para o comando gettemporárioname Bitsadmin, que relata o nome de arquivo temporário do arquivo fornecido dentro do trabalho.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 68925edc-a801-4292-a812-7471c4f60fdd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f7780691f37fb78f1553fa993fd408d224be39ff
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717491"
---
# <a name="bitsadmin-gettemporaryname"></a>bitsadmin gettemporaryname

Informa o nome de arquivo temporário do arquivo fornecido dentro do trabalho.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /gettemporaryname <job> <file_index>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| file_index | Começa em 0. |

## <a name="examples"></a>Exemplos

Para relatar o nome de arquivo temporário de File 2 para o trabalho chamado *myDownloadJob*:

```
bitsadmin /gettemporaryname myDownloadJob 1
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
