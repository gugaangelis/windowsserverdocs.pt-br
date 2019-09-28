---
title: bitsadmin addfile
description: O tópico de comandos do Windows para **Bitsadmin AddFile** – adiciona um arquivo ao trabalho especificado.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1b31aa93-0364-465b-af36-754968825989
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8dfddda92e506dbfca2a47394a310edf16fe78aa
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382037"
---
# <a name="bitsadmin-addfile"></a>bitsadmin addfile

Adiciona um arquivo ao trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /AddFile <Job> <RemoteURL> <LocalName>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|
|RemoteURL|A URL do arquivo no servidor.|
|LocalName|O nome do arquivo no computador local. *LocalName* deve conter um caminho absoluto para o arquivo.|

## <a name="BKMK_examples"></a>Disso

Adicione um arquivo ao trabalho. Repita essa chamada para cada arquivo que você deseja adicionar. Se vários trabalhos usarem *myDownloadJob* como seu nome, você deverá substituir *myDownloadJob* pelo GUID do trabalho para identificar exclusivamente o trabalho.
```
C:\>bitsadmin /addfile myDownloadJob http://downloadsrv/10mb.zip c:\10mb.zip
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)