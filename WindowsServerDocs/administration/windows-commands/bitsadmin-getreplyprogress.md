---
title: bitsadmin getreplyprogress
description: Tópico de comandos do Windows para **Bitsadmin getreplyprogress** – recupera o tamanho e o progresso da resposta do servidor.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7f7cb0b4-ad95-44fd-a35d-0ddf5fc0b0d0
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c791fe98271b497e5ecf48338ab3bbb0cc50de98
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381245"
---
# <a name="bitsadmin-getreplyprogress"></a>bitsadmin getreplyprogress

Recupera o tamanho e o progresso da resposta do servidor.

**BITS 1,2 e anteriores**: Não compatível.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetReplyProgress <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="remarks"></a>Comentários

Válido somente para trabalhos de resposta de upload.

## <a name="BKMK_examples"></a>Disso

O exemplo a seguir recupera o progresso da resposta para o trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /GetReplyProgress myDownloadJob
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)