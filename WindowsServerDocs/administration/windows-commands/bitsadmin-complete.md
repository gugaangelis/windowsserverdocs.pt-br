---
title: bitsadmin complete
description: Artigo de referência para o comando Bitsadmin Complete, que conclui o trabalho.
ms.topic: reference
ms.assetid: a5e86677-8f7b-43b3-929e-97706c57e7f1
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 88e8ac3920adb80dcc453317fbffe5fd661811fb
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632381"
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
