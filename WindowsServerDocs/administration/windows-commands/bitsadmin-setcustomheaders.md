---
title: bitsadmin setcustomheaders
description: Tópico de comandos do Windows para Bitsadmin setcustomheaders, que adiciona um cabeçalho HTTP personalizado a uma solicitação GET.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ed926410-80d0-46ed-9a90-f752c164bb9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e5d97fae5f84637c80c3d1ef00aa36f09049bb17
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849609"
---
# <a name="bitsadmin-setcustomheaders"></a>bitsadmin setcustomheaders

Adicione um cabeçalho HTTP personalizado a uma solicitação GET.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetCustomHeaders <Job> <Header1> <Header2> <. . .>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Trabalho|O nome de exibição ou o GUID do trabalho|
|Header1 Header2 . . .|Os cabeçalhos personalizados para o trabalho|

## <a name="remarks"></a>Comentários

-   Essa opção é usada para adicionar um cabeçalho HTTP personalizado a uma solicitação GET enviada a um servidor HTTP.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir adiciona um cabeçalho HTTP personalizado para o trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin / SetCustomHeaders myDownloadJob Accept-encoding:deflate/gzip
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)