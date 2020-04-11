---
title: bitsadmin setminretrydelay
description: O tópico de comandos do Windows para **Bitsadmin setminretrydelay**, que define o período mínimo de tempo, em segundos, que o bits aguarda depois de encontrar um erro transitório antes de tentar transferir o arquivo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ce8674ca-6cc5-4bb2-8dda-7dfbb1cd6830
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ddae9a62a49ca07bb03649f131a0a1ebad8ee3fe
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122877"
---
# <a name="bitsadmin-setminretrydelay"></a>bitsadmin setminretrydelay

Define o período mínimo de tempo, em segundos, que o BITS aguarda depois de encontrar um erro transitório antes de tentar transferir o arquivo.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /setminretrydelay <job> <retrydelay>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| retrydelay | Período mínimo de tempo para que os BITS aguardem após um erro durante a transferência, em segundos. |

## <a name="examples"></a>Exemplos

O exemplo a seguir define o atraso mínimo de repetição para o trabalho chamado *myDownloadJob* a 35 segundos.

```
C:\>bitsadmin /setminretrydelay myDownloadJob 35
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)