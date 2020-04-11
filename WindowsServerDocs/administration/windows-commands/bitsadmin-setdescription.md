---
title: bitsadmin setdescription
description: O tópico de comandos do Windows para **Bitsadmin SetDescription**, que define a descrição do trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1e46a5dd-4637-4a2e-b88f-d3f85b177db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0b62e6b030c23c475418cd6f2c63f04edba1acff
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123013"
---
# <a name="bitsadmin-setdescription"></a>bitsadmin setdescription

Define a descrição para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /setdescription <job> <description>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| description | Texto usado para descrever o trabalho. |

## <a name="examples"></a>Exemplos

O exemplo a seguir recupera a descrição para o trabalho chamado *myDownloadJob*.

```
C:\>bitsadmin /setdescription myDownloadJob music_downloads
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)