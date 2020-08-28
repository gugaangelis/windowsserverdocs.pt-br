---
title: bitsadmin listfiles
description: Artigo de referência para o comando Bitsadmin ListFiles, que lista os arquivos no trabalho especificado.
ms.topic: reference
ms.assetid: ad0d1eaa-3bd8-45e5-8f72-4da7366f0d59
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b6afadf78acd187b336484db47b128afb49e27fe
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024300"
---
# <a name="bitsadmin-listfiles"></a>bitsadmin listfiles

Lista os arquivos no trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /listfiles <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar a lista de arquivos para o trabalho chamado *myDownloadJob*:

```
bitsadmin /listfiles myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
