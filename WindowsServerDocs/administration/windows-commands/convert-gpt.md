---
title: convert gpt
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b3b1b747-0a7a-4be2-8487-2c4be16ee190
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9a6392cbcff618c642b9d0f168fe555e8be9e759
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379088"
---
# <a name="convert-gpt"></a>convert gpt



Converte um disco básico vazio com o estilo de partição MBR (registro mestre de inicialização) em um disco básico com o estilo de partição GPT (tabela de partição GUID).

Para obter instruções sobre como usar esse comando, consulte [alterar um disco de registro mestre de inicialização para um disco de tabela de partição GUID](https://go.microsoft.com/fwlink/?LinkId=207049) (https://go.microsoft.com/fwlink/?LinkId=207049).

## <a name="syntax"></a>Sintaxe

```
convert gpt [noerr]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|NOERR|Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro.|

## <a name="remarks"></a>Comentários

> [!IMPORTANT]
> O disco deve estar vazio para convertê-lo em um disco GPT. Faça backup dos dados e exclua todas as partições ou volumes antes de converter o disco.
> -   O tamanho mínimo de disco necessário para conversão em GPT é de 128 megabytes.
> -   Um disco MBR básico deve ser selecionado para que essa operação tenha sucesso. Use o comando **selecionar disco** para selecionar um disco básico e deslocar o foco para ele.

## <a name="BKMK_examples"></a>Disso

Para converter um disco básico do estilo de partição MBR para o estilo de partição GPT, digite:
```
convert gpt
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)

