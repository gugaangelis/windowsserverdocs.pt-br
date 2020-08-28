---
title: bitsadmin peercaching e setconfigurationflags
description: Artigo de referência para o comando Bitsadmin do serviço de cache e setconfigurationflags, que define os sinalizadores de configuração que determinam se o computador pode fornecer conteúdo a pares e se ele pode baixar conteúdo de pares.
ms.topic: reference
ms.assetid: ff0a2b49-66e3-4d40-824c-6a3816055d2e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0027fa5c926f09b95541906f168e852292b19aaf
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89026515"
---
# <a name="bitsadmin-peercaching-and-setconfigurationflags"></a>bitsadmin peercaching e setconfigurationflags

Define os sinalizadores de configuração que determinam se o computador pode fornecer conteúdo a pares e se ele pode baixar conteúdo de pares.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /peercaching /setconfigurationflags <job> <value>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| value | Um inteiro não assinado com a seguinte interpretação para os bits na representação binária:<ul><li>Para permitir que os dados do trabalho sejam baixados de um par, defina o bit menos significativo.</li><li>Para permitir que os dados do trabalho sejam servidos para os pares, defina o segundo bit à direita.</li></ul>|

## <a name="examples"></a>Exemplos

Para especificar os dados do trabalho a serem baixados de pares para o trabalho chamado *myDownloadJob*:

```
bitsadmin /peercaching /setconfigurationflags myDownloadJob 1
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)

- [comando de Bitsadmin de cache](bitsadmin-peercaching.md)
