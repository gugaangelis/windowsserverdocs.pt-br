---
title: bitsadmin setdescription
description: O tópico de comandos do Windows para Bitsadmin SetDescription, que define a descrição do trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1e46a5dd-4637-4a2e-b88f-d3f85b177db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4a17f864e3bc3b3cdc8ba0d76d553bcfcef27d29
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80849559"
---
# <a name="bitsadmin-setdescription"></a>bitsadmin setdescription

Define a descrição do trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /SetDescription <Job> <Description>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Trabalho|O nome de exibição ou o GUID do trabalho|
|Descrição|Texto usado para descrever o trabalho.|

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir recupera a descrição para o trabalho chamado *myDownloadJob*.
```
C:\>bitsadmin /SetDescription myDownloadJob Music Downloads
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)