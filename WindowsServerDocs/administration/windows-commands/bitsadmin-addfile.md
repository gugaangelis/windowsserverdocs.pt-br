---
title: bitsadmin addfile
description: Tópico de comandos do Windows para **bitsadmin addfile** -adiciona um arquivo para o trabalho especificado.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 1c3027bdc4f3f8f3e3ca50400b2c5dbf33bf2bc5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861747"
---
# <a name="bitsadmin-addfile"></a>bitsadmin addfile

Adiciona um arquivo para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /AddFile <Job> <RemoteURL> <LocalName>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|
|RemoteURL|A URL do arquivo no servidor.|
|LocalName|O nome do arquivo no computador local. *LocalName* deve conter um caminho absoluto para o arquivo.|

## <a name="BKMK_examples"></a>Exemplos

Adicione um arquivo para o trabalho. Repita esta chamada para cada arquivo que você deseja adicionar. Se usam vários trabalhos *myDownloadJob* como seu nome, você deve substituir *myDownloadJob* com o GUID do trabalho para identificar exclusivamente o trabalho.
```
C:\>bitsadmin /addfile myDownloadJob http://downloadsrv/10mb.zip c:\10mb.zip
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)