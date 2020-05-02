---
title: bitsadmin getpeercachingflags
description: Tópico de referência para o comando Bitsadmin getpeercachingflags, que recupera sinalizadores que determinam se os arquivos do trabalho podem ser armazenados em cache e servidos para os pares e se o BITS pode baixar conteúdo para o trabalho de pares.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3c3c9f28-4c04-4c49-a23a-dee5bbcc8981
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f30eead56958af3cd0fb0d91f6ee2bf9f79fdc4e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717687"
---
# <a name="bitsadmin-getpeercachingflags"></a>bitsadmin getpeercachingflags

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Recupera sinalizadores que determinam se os arquivos do trabalho podem ser armazenados em cache e servidos para os pares e se o BITS pode baixar conteúdo para o trabalho de pares.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getpeercachingflags <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar os sinalizadores para o trabalho chamado *myDownloadJob*:

```
bitsadmin /getpeercachingflags myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
