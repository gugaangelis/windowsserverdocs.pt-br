---
title: Bitsadmin getproxylist – recupera a lista de proxy para o trabalho especificado.
description: O tópico de comandos do Windows para **Bitsadmin getproxylist** -recupera a lista de proxy para o trabalho especificado.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 6f176d268c816725b183da0a948afcb25272b2fb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381307"
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
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="remarks"></a>Comentários

A lista de proxy é a lista de servidores proxy a serem usados. A lista é delimitada por vírgula.

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir recupera a lista de proxy para o trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /GetProxyList myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)