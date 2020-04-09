---
title: bitsadmin getcompletiontime
description: O tópico de comandos do Windows para **Bitsadmin getcompletetime**, que recupera a hora em que o trabalho concluiu a transferência de dados.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7a4b3c1c-9832-4724-86b2-cce3c01bfa28
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5408e7e8c35135601a4a0af0ab7e9c55cea4c8dd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850749"
---
# <a name="bitsadmin-getcompletiontime"></a>bitsadmin getcompletiontime

Recupera a hora em que o trabalho concluiu a transferência de dados.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getcompletiontime <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir recupera a hora em que o trabalho nomeado *myDownloadJob* concluiu a transferência de dados.

```
C:\>bitsadmin /getcompletiontime myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)