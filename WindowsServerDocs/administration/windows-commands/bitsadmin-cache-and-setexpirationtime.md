---
title: cache Bitsadmin e setexpiretime
description: Tópico de comandos do Windows para o **cache Bitsadmin e setexpiretime**, que define o tempo de expiração do cache.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 00ea6e4e-b707-4b31-88dd-b61a78565c8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bf283a0a8b94fd55c591609e3dcd1d127a2be81a
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850879"
---
>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="bitsadmin-cache-and-setexpirationtime"></a>cache Bitsadmin e setexpiretime

Define o tempo de expiração do cache.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /cache /setexpirationtime secs
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| seg | O número de segundos até que o cache expire. |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir expira o cache em 60 segundos.

```
C:\>bitsadmin /cache / setexpirationtime 60
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
