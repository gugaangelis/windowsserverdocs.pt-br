---
title: bitsadmin makecustomheaderswriteonly
description: Tópico de referência para o comando Bitsadmin makecustomheaderswriteonly, que torna os cabeçalhos HTTP personalizados de um trabalho somente gravação.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 2aeab7e0ee7797b3e0be7be1156920f3bafc84dc
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717410"
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
