---
title: bitsadmin setdescription
description: Artigo de referência do comando Bitsadmin SetDescription, que define a descrição do trabalho especificado.
ms.topic: reference
ms.assetid: 1e46a5dd-4637-4a2e-b88f-d3f85b177db8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a306605d447a3bc3a40b16f75a1a63badf75f3b0
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630947"
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

Para recuperar a descrição para o trabalho chamado *myDownloadJob*:

```
bitsadmin /setdescription myDownloadJob music_downloads
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
