---
title: bitsadmin rawreturn
description: Artigo de referência para o comando Bitsadmin rawreturn, que retorna dados adequados para análise.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bbe97130-26f6-4cdd-84f1-baf530ce38b7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1b537b21678c100364406d4c59eaa02efd143e21
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926460"
---
# <a name="bitsadmin-rawreturn"></a>bitsadmin rawreturn

Retorna os dados adequados para análise. Normalmente, você usa esse comando em conjunto com as opções **/Create** e **/Get*** para receber apenas o valor. Você deve especificar essa opção antes de outras opções.

> [!NOTE]
> Esse comando retira os caracteres de nova linha e a formatação da saída.

## <a name="syntax"></a>Syntax

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
