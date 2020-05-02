---
title: bitsadmin getreplyprogress
description: Tópico de referência para o comando Bitsadmin getreplyprogress, que recupera o tamanho e o progresso da resposta de upload do servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7f7cb0b4-ad95-44fd-a35d-0ddf5fc0b0d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7c355c796d480e9deb444b8fd9ee7570136cade6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717584"
---
# <a name="bitsadmin-getreplyprogress"></a>bitsadmin getreplyprogress

Recupera o tamanho e o progresso da resposta de upload do servidor.

> [!NOTE]
> Esse comando não tem suporte no BITS 1,2 e versões anteriores.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getreplyprogress <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar o progresso do upload-resposta para o trabalho chamado *myDownloadJob*:

```
bitsadmin /getreplyprogress myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
