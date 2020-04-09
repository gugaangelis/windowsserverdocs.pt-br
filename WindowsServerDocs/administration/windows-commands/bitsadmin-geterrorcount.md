---
title: bitsadmin geterrorcount
description: O tópico de comandos do Windows para Bitsadmin GetErrorCount, que recupera uma contagem do número de vezes que o trabalho especificado gerou um erro transitório.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8840ae78-52b0-4c7e-b592-0547359a237e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1711781dd416311e45874b7c611f3f8f42a06854
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850689"
---
# <a name="bitsadmin-geterrorcount"></a>bitsadmin geterrorcount

Recupera uma contagem do número de vezes que o trabalho especificado gerou um erro transitório.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /geterrorcount <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir recupera informações de contagem de erros para o trabalho chamado *myDownloadJob*.

```
C:\>bitsadmin /geterrorcount myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)