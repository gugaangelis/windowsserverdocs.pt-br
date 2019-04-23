---
title: bitsadmin getproxybypasslist
description: Tópico de comandos do Windows para **getproxybypasslist bitsadmin** -recupera a lista de bypass de proxy para o trabalho especificado.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 50959be3-7014-4bc9-9a7b-68f1ff94a94a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 020b8fc0019eb103a0e469258be8705b80dd45de
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854107"
---
# <a name="bitsadmin-getproxybypasslist"></a>bitsadmin getproxybypasslist

Recupera a lista de bypass de proxy para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetProxyBypassList <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|

## <a name="remarks"></a>Comentários

A lista de bypass contém os nomes de host ou endereços IP ou ambos, que não devem ser roteadas através de um proxy. A lista pode conter "\<local >" para referir-se a todos os servidores na mesma LAN. A lista pode ser um ponto e vírgula ou delimitada por espaço.

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir recupera a lista de bypass de proxy do trabalho nomeado *myDownloadJob*.
```
C:\>bitsadmin /GetProxyBypassList myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)