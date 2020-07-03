---
title: bitsadmin listfiles
description: Artigo de referência para o comando Bitsadmin ListFiles, que lista os arquivos no trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ad0d1eaa-3bd8-45e5-8f72-4da7366f0d59
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2702fbaec76aac666d931264c9855017b602e8ea
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926533"
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
