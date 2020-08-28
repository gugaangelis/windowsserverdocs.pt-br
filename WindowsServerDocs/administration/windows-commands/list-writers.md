---
title: list writers
description: Artigo de referência para o comando list writers, que lista os gravadores que estão no sistema.
ms.topic: reference
ms.assetid: 1c30cbc4-f568-4fa7-b564-66c41d3ca82d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2fd3f035db63281b75ab2232503587dd535ef8d9
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89037904"
---
# <a name="list-writers"></a>list writers

Lista gravadores que estão no sistema. Se usado sem parâmetros, **list** exibirá a saída para **listar metadados** por padrão.

## <a name="syntax"></a>Sintaxe

```
list writers [metadata | detailed | status]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| metadata | Lista a identidade e o status dos gravadores e exibe os metadados, como detalhes do componente e arquivos excluídos. Esse é o parâmetro padrão. |
| detalhado | Lista as mesmas informações como **metadados**, mas também inclui a lista completa de arquivos para todos os componentes. |
| status | Lista somente a identidade e o status dos gravadores registrados. |

### <a name="examples"></a>Exemplos

Para listar apenas a identidade e o status dos gravadores, digite:

```
list writers status
```

Saída semelhante às seguintes exibições:

```
Listing writer status ...
* WRITER System Writer
        - Status: 5 (VSS_WS_WAITING_FOR_BACKUP_COMPLETE)
        - Writer Failure code: 0x00000000 (S_OK)
        - Writer ID: {e8132975-6f93-4464-a53e-1050253ae220}
        - Instance ID: {7e631031-c695-4229-9da1-a7de057e64cb}
* WRITER Shadow Copy Optimization Writer
        - Status: 1 (VSS_WS_STABLE)
        - Writer Failure code: 0x00000000 (S_OK)
        - Writer ID: {4dc3bdd4-ab48-4d07-adb0-3bee2926fd7f}
        - Instance ID: {9e362607-9794-4dd4-a7cd-b3d5de0aad20}
* WRITER Registry Writer
        - Status: 1 (VSS_WS_STABLE)
        - Writer Failure code: 0x00000000 (S_OK)
        - Writer ID: {afbab4a2-367d-4d15-a586-71dbb18f8485}
        - Instance ID: {e87ba7e3-f8d8-42d8-b2ee-c76ae26b98e8}
8 writers listed.
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)