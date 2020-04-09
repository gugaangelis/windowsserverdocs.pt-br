---
title: cache Bitsadmin e excluir
description: Tópico de comandos do Windows para **cache Bitsadmin e Delete**, que exclui uma entrada de cache específica.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 22540273-55a5-46ea-869b-6df2aa6808a1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0fd7f1db83a62dd9c1085d6afdcf509c1c3ac8cf
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850939"
---
# <a name="bitsadmin-cache-and-delete"></a>cache Bitsadmin e excluir

Exclui uma entrada de cache específica.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /cache /delete recordID
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| recordID | O GUID associado à entrada de cache. |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir exclui a entrada de cache com recordId de {6511FB02-E195-40A2-B595-E8E2F8F47702}.

```
C:\>bitsadmin /cache /delete {6511FB02-E195-40A2-B595-E8E2F8F47702}
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)