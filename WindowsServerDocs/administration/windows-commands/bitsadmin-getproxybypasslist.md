---
title: bitsadmin getproxybypasslist
description: Tópico de comandos do Windows para **Bitsadmin getproxybypasslist** – recupera a lista de bypass de proxy para o trabalho especificado.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 87cc131402707eac40329750e98218ec52083b94
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381421"
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
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="remarks"></a>Comentários

A lista de bypass contém os nomes de host ou endereços IP, ou ambos, que não devem ser roteados por meio de um proxy. A lista pode conter "\<local >" para se referir a todos os servidores na mesma LAN. A lista pode ser ponto e vírgula ou delimitado por espaço.

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir recupera a lista de bypass de proxy para o trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /GetProxyBypassList myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)