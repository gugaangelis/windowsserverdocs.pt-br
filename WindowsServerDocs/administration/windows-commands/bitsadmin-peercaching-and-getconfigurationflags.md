---
title: bitsadmin peercaching e getconfigurationflags
description: Tópico de referência para o comando Bitsadmin do serviço de cache e getconfigurationflags, que obtém os sinalizadores de configuração que determinam se o computador fornece conteúdo a pares e se pode baixar conteúdo de pares.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 124ddc15-3444-4bd5-96e5-c6bfabe4f9c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 62b6848dec30a9a9fef401b1b2372605dbb9934a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717330"
---
# <a name="bitsadmin-peercaching-and-getconfigurationflags"></a>bitsadmin peercaching e getconfigurationflags

Obtém os sinalizadores de configuração que determinam se o computador fornece conteúdo a pares e se pode baixar conteúdo de pares.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /peercaching /getconfigurationflags <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para obter os sinalizadores de configuração para o trabalho chamado *myDownloadJob*:

```
bitsadmin /peercaching /getconfigurationflags myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)

- [comando de Bitsadmin de cache](bitsadmin-peercaching.md)
