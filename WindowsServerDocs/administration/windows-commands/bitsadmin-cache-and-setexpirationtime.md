---
title: cache Bitsadmin e setexpiretime
description: O tópico de comandos do Windows para o **cache Bitsadmin e setexpiretime** -define o tempo de expiração do cache.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 00ea6e4e-b707-4b31-88dd-b61a78565c8d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 386c6659e4410b41669ade39d8af97829d81a1cb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381924"
---
>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="bitsadmin-cache-and-setexpirationtime"></a>cache Bitsadmin e setexpiretime
Define o tempo de expiração do cache.
## <a name="syntax"></a>Sintaxe
```
bitsadmin /Cache /SetExpirationtime secs
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|seg|O número de segundos até que o cache expire.|
## <a name="BKMK_examples"></a>Disso
O exemplo a seguir expira o cache em 60 segundos.
```
C:\>bitsadmin /Cache / SetExpirationtime 60
```
## <a name="additional-references"></a>Referências adicionais
[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
