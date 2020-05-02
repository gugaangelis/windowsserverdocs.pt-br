---
title: bitsadmin info
description: Tópico de referência para o comando Bitsadmin info, que exibe informações de resumo sobre o trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5c306677-0d64-41c0-8276-5bba7750cecb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 904f70c82ab4bcc4fb25f759898674cc719b1954
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717435"
---
# <a name="bitsadmin-info"></a>bitsadmin info

Exibe informações de resumo sobre o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /info <job> [/verbose]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| /verbose | Opcional. Fornece informações detalhadas sobre cada trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar informações sobre o trabalho chamado *myDownloadJob*:

```
bitsadmin /info myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [bitsadmin info](bitsadmin-info.md)
