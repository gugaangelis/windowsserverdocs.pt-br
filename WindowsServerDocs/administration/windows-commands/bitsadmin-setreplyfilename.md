---
title: bitsadmin setreplyfilename
description: O tópico de comandos do Windows para Bitsadmin setreplyfilename, que especifica o caminho do arquivo que contém a resposta do servidor.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c26d3342-0533-40b1-a13e-e09678232b25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fd45174a7deac89cc943fb19d544e372c0198139
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849179"
---
# <a name="bitsadmin-setreplyfilename"></a>bitsadmin setreplyfilename

Especifica o caminho do arquivo que contém a resposta do servidor.

**BITS 1,2 e anteriores**: sem suporte.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetReplyFileName <Job> <Path>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Trabalho|O nome de exibição ou o GUID do trabalho|
|Caminho|Local onde o servidor deve ser respondido|

## <a name="remarks"></a>Comentários

Válido somente para trabalhos de resposta de upload.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir define o nome de arquivo de resposta pathfor o trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /SetReplyFileName myDownloadJob c:\reply
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)