---
title: bitsadmin complete
description: Artigo de referência para o comando Bitsadmin Complete, que conclui o trabalho.
ms.topic: reference
ms.assetid: a5e86677-8f7b-43b3-929e-97706c57e7f1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c21cd7919d49b2ac68c665dfd7043cbe55937e45
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89026684"
---
# <a name="bitsadmin-complete"></a>bitsadmin complete

Conclui o trabalho. Use essa opção depois que o trabalho for movido para o estado transferido. Caso contrário, somente os arquivos que foram transferidos com êxito estarão disponíveis.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /complete <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="example"></a>Exemplo

Para concluir o trabalho *myDownloadJob* , depois que ele atingir o `TRANSFERRED` estado:

```
bitsadmin /complete myDownloadJob
```

Se vários trabalhos usarem *myDownloadJob* como seu nome, você deverá usar o GUID do trabalho para identificá-lo exclusivamente para conclusão.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
