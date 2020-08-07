---
title: bitsadmin rawreturn
description: Artigo de referência para o comando Bitsadmin rawreturn, que retorna dados adequados para análise.
ms.topic: article
ms.assetid: bbe97130-26f6-4cdd-84f1-baf530ce38b7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 88b1e45d7fad2820a77d9f445065ae0ca182abb6
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893427"
---
# <a name="bitsadmin-rawreturn"></a>bitsadmin rawreturn

Retorna os dados adequados para análise. Normalmente, você usa esse comando em conjunto com as opções **/Create** e **/Get*** para receber apenas o valor. Você deve especificar essa opção antes de outras opções.

> [!NOTE]
> Esse comando retira os caracteres de nova linha e a formatação da saída.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /rawreturn
```

## <a name="examples"></a>Exemplos

Para recuperar os dados brutos para o estado do trabalho chamado *myDownloadJob*:

```
bitsadmin /rawreturn /getstate myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
