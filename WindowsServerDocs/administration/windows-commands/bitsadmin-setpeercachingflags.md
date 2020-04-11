---
title: bitsadmin setpeercachingflags
description: O tópico de comandos do Windows para **Bitsadmin setpeercachingflags**, que define sinalizadores que determinam se os arquivos do trabalho podem ser armazenados em cache e servidos para os colegas e se o trabalho pode baixar conteúdo de pares.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3f54a127-fb68-49a5-b843-664ec833df67
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1b4a7807975fb46440301e30b1fdbd01784d7c85
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122772"
---
# <a name="bitsadmin-setpeercachingflags"></a>bitsadmin setpeercachingflags

Define sinalizadores que determinam se os arquivos do trabalho podem ser armazenados em cache e servidos para os colegas e se o trabalho pode baixar conteúdo de pares.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /setpeercachingflags <job> <value>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| {1&gt;Valor&lt;1} | Um inteiro sem sinal, incluindo:<ul><li>**1.** o trabalho pode baixar conteúdo de pares.</li><li>**2.** os arquivos do trabalho podem ser armazenados em cache e servidos para os pares.</li></ul> |

## <a name="examples"></a>Exemplos

O exemplo a seguir define sinalizadores para o trabalho chamado *myDownloadJob*, permitindo que ele baixe o conteúdo de pares.

```
C:\>bitsadmin /setpeercachingflags myDownloadJob 1
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)