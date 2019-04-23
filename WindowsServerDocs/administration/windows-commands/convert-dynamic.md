---
title: Converter dinâmico
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 7b8fa4b1-850f-4e48-b05f-871c883ea33c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 353c1e4558ab2b0c948ec78c0cd87b579c738ec8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841607"
---
# <a name="convert-dynamic"></a>Converter dinâmico



Converte um disco básico em um disco dinâmico.

Para obter instruções sobre como usar esse comando, consulte [converter um disco básico em um disco dinâmico](https://go.microsoft.com/fwlink/?LinkId=207047) (https://go.microsoft.com/fwlink/?LinkId=207047).

## <a name="syntax"></a>Sintaxe

```
convert dynamic [noerr]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|noerr|Somente para scripts. Quando um erro for encontrado, o DiskPart continua a processar comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro causar o DiskPart sair com um código de erro.|

## <a name="remarks"></a>Comentários

-   As partições existentes no disco básico se tornam volumes simples.
-   Um disco básico deve ser selecionado para essa operação seja bem-sucedida. Use o **Selecionar disco** comando para selecionar um disco básico e mudar o foco a ele.

## <a name="BKMK_examples"></a>Exemplos

Para converter um disco básico em um disco dinâmico, digite:
```
convert dynamic
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)

