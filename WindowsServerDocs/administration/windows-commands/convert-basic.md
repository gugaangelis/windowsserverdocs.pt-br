---
title: convert basic
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 61329896-3b56-4959-8d58-45cbe18ba860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5a5aef930689e809b9b697e5f428177610ff723b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862397"
---
# <a name="convert-basic"></a>convert basic



Converte um disco dinâmico vazio em um disco básico.

Para obter instruções sobre como usar esse comando, consulte [alterar um disco dinâmico volta para um disco básico](https://go.microsoft.com/fwlink/?LinkId=207048) (https://go.microsoft.com/fwlink/?LinkId=207048).

## <a name="syntax"></a>Sintaxe

```
convert basic [noerr]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|noerr|Somente para scripts. Quando um erro for encontrado, o DiskPart continua a processar comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro causar o DiskPart sair com um código de erro.|

## <a name="remarks"></a>Comentários

> [!IMPORTANT]
> O disco deve estar vazio para convertê-lo em um disco básico. Fazer backup dos dados e, em seguida, exclua todas as partições ou volumes antes de converter o disco.
-   Um disco dinâmico deve ser selecionado para essa operação seja bem-sucedida. Use o **Selecionar disco** comando para selecionar um disco dinâmico e mudar o foco a ele.

## <a name="BKMK_examples"></a>Exemplos

Para converter o disco dinâmico selecionado para basic, digite:
```
convert basic
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)

