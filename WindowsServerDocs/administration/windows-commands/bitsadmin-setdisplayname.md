---
title: bitsadmin setdisplayname
description: Tópico de comandos do Windows para Bitsadmin SetDisplayName, que define o nome de exibição do trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 13706c53-fb5f-4879-b5ca-82531361d6e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 601c5b406132e70fb7d4facb97329f7456002bb4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849539"
---
# <a name="bitsadmin-setdisplayname"></a>bitsadmin setdisplayname

Define o nome de exibição do trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetDisplayName <Job> <DisplayName>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Trabalho|O nome de exibição ou o GUID do trabalho|
|DisplayName|Texto usado para o nome de exibição do trabalho especificado.|

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir define o nome para exibição do trabalho chamado *myDownloadJob* como *myDownloadJob2*.
```
C:\>bitsadmin /SetDisplayName myDownloadJob Download Music Job
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)