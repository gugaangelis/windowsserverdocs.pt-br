---
title: bitsadmin makecustomheaderswriteonly
description: Artigo de referência para o comando Bitsadmin makecustomheaderswriteonly, que torna os cabeçalhos HTTP personalizados de um trabalho somente gravação.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 3a97054bb9e8156de23e07d18d9806e04c59e967
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926523"
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
