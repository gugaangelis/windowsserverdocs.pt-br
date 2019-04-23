---
title: Bitsadmin getproxylist - recupera a lista de proxy para o trabalho especificado.
description: Tópico de comandos do Windows para **getproxylist bitsadmin** -recupera a lista de proxy para o trabalho especificado.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: eebfa727-d8f1-4ae3-9382-6d8ffe8c3df3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e8c3ffb1e425552cda5b14a00287817ace77a90f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840507"
---
# <a name="bitsadmin-getproxylist"></a>bitsadmin getproxylist

Recupera a lista de proxy para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetProxyList <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|

## <a name="remarks"></a>Comentários

A lista de proxy é a lista de servidores proxy para usar. A lista é delimitada por vírgulas.

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir recupera a lista de proxy do trabalho nomeado *myDownloadJob*.
```
C:\>bitsadmin /GetProxyList myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)