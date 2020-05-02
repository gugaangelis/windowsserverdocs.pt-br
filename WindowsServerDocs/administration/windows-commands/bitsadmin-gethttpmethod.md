---
title: bitsadmin gethttpmethod
description: Tópico de referência para o comando Bitsadmin gethttpmethod, que obtém o verbo HTTP a ser usado com o trabalho.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: a458322a5ace69df74df054a537a7365da9e7329
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717889"
---
# <a name="bitsadmin-gethttpmethod"></a>bitsadmin gethttpmethod

Obtém o verbo HTTP a ser usado com o trabalho.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /gethttpmethod <Job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar o verbo HTTP a ser usado com o trabalho chamado *myDownloadJob*:

```
bitsadmin /gethttpmethod myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
