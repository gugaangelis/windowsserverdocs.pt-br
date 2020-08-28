---
title: bitsadmin makecustomheaderswriteonly
description: Artigo de referência para o comando Bitsadmin makecustomheaderswriteonly, que torna os cabeçalhos HTTP personalizados de um trabalho somente gravação.
ms.topic: reference
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 4d31f51c2531079342e223752c626b0b7e8d19f8
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89024260"
---
# <a name="bitsadmin-makecustomheaderswriteonly"></a>bitsadmin makecustomheaderswriteonly

Faça com que os cabeçalhos HTTP personalizados de um trabalho somente sejam gravados.

> [!IMPORTANT]
> Esta ação não pode ser desfeita.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /makecustomheaderswriteonly <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para fazer com que cabeçalhos HTTP personalizados sejam gravados somente para o trabalho chamado *myDownloadJob*:

```
bitsadmin /makecustomheaderswriteonly myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
