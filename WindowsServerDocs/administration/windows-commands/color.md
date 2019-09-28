---
title: cor
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f5b67131-d196-45ec-a3f9-b5d9f091fd86
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ed792e4626897945e688f1c54767d7680ade6d99
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379244"
---
# <a name="color"></a>cor



Altera as cores de primeiro e segundo plano na janela do prompt de comando para a sessão atual. Se usado sem parâmetros, **Color** restaura as cores de primeiro plano e tela de fundo da janela de prompt de comando padrão.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
color [[<B>]<F>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<B >|Especifica a cor do plano de fundo.|
|\<F >|Especifica a cor de primeiro plano.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   A tabela a seguir lista os dígitos hexadecimais válidos que podem ser usados como valores para *B* e *F*.

|Valor|Cor|
|-----|-----|
|0|Preto|
|1|Azul|
|2|Verde|
|3|Aqua|
|4|Vermelho|
|5|Roxo|
|6|Amarelo|
|7|Branco|
|8|Cinza|
|9|Azul-claro|
|A|Verde-claro|
|B|Azul-claro|
|C|Vermelho-claro|
|D|Roxo-claro|
|E|Amarelo-claro|
|F|Branco brilhante|
    
-   Não use caracteres de espaço entre *B* e *F*.
-   Se você especificar apenas um dígito hexadecimal, a cor correspondente será usada como a cor de primeiro plano e a cor do plano de fundo será definida como a cor padrão.
-   Para definir a cor da janela do prompt de comando padrão, clique no canto superior esquerdo da janela do prompt de comando, clique em **padrões**, clique na guia **cores** e clique nas cores que você deseja usar para o **texto da tela** e o plano de **fundo da tela** .
-   Se *B* e *F* forem os mesmos, o comando **Color** definirá ERRORLEVEL como 1 e nenhuma alteração será feita na cor de primeiro plano ou segundo plano.

## <a name="BKMK_examples"></a>Disso

Para alterar a cor do plano de fundo da janela do prompt de comando para cinza e a cor do primeiro plano para vermelho, digite:
```
color 84
```
Para alterar a cor do primeiro plano da janela do prompt de comando para amarelo claro, digite:
```
color e
```

> [!NOTE]
> Neste exemplo, o plano de fundo é definido como a cor padrão porque apenas um dígito hexadecimal é especificado.

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
