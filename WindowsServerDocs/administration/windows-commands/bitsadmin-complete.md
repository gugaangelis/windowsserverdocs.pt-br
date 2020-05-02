---
title: bitsadmin complete
description: Tópico de referência para o comando Bitsadmin Complete, que conclui o trabalho.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a5e86677-8f7b-43b3-929e-97706c57e7f1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b61f3475afdb0e29e5777940e6426a04fe33e78
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718225"
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

Para concluir o trabalho *myDownloadJob* , depois que ele atingir `TRANSFERRED` o estado:

```
bitsadmin /complete myDownloadJob
```

Se vários trabalhos usarem *myDownloadJob* como seu nome, você deverá usar o GUID do trabalho para identificá-lo exclusivamente para conclusão.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
