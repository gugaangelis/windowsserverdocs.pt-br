---
title: bitsadmin getfilestotal
description: Tópico de comandos do Windows para **Bitsadmin getfilestotal** – recupera o número de arquivos no trabalho especificado.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c5de113e-f29c-4cd3-9392-0e300018d516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e5c28d8e970bd7db896073bf8cddb168ffe9deff
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75946842"
---
# <a name="bitsadmin-getfilestotal"></a>bitsadmin getfilestotal



Recupera o número de arquivos no trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /GetFilesTotal <Job>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|

## <a name="BKMK_examples"></a>Exemplos

O exemplo a seguir recupera o número de arquivos incluídos no trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /GetFilesTotal myDownloadJob
```

## <a name="see-also"></a>Veja também

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
