---
title: convert basic
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: c4b81126f4a623d841bb5868f786678d7b093581
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379134"
---
# <a name="convert-basic"></a>convert basic



Converte um disco dinâmico vazio em um disco básico.

Para obter instruções sobre como usar esse comando, consulte [alterar um disco dinâmico de volta para um disco básico](https://go.microsoft.com/fwlink/?LinkId=207048) (https://go.microsoft.com/fwlink/?LinkId=207048).

## <a name="syntax"></a>Sintaxe

```
convert basic [noerr]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|NOERR|Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro.|

## <a name="remarks"></a>Comentários

> [!IMPORTANT]
> O disco deve estar vazio para convertê-lo em um disco básico. Faça backup dos dados e exclua todas as partições ou volumes antes de converter o disco.
> -   Um disco dinâmico deve ser selecionado para que essa operação tenha sucesso. Use o comando **selecionar disco** para selecionar um disco dinâmico e deslocar o foco para ele.

## <a name="BKMK_examples"></a>Disso

Para converter o disco dinâmico selecionado em básico, digite:
```
convert basic
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)

