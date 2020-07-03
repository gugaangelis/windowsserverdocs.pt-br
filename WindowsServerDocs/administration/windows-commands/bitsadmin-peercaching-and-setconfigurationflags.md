---
title: bitsadmin peercaching e setconfigurationflags
description: Artigo de referência para o comando Bitsadmin do serviço de cache e setconfigurationflags, que define os sinalizadores de configuração que determinam se o computador pode fornecer conteúdo a pares e se ele pode baixar conteúdo de pares.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ff0a2b49-66e3-4d40-824c-6a3816055d2e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 868ef39104f1d16c760d91eee401c0d48b27ea1f
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85928120"
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
