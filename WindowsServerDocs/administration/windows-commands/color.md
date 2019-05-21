---
title: color
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f8d23cc1bb5739b47c755d90c927cbcf82b8da7f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825017"
---
# <a name="color"></a>color



Alterações de primeiro plano e plano de fundo de cores na janela do Prompt de comando para a sessão atual. Se usado sem parâmetros, **cor** restaura as cores de primeiro plano e plano de fundo de janela do Prompt de comando padrão.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
color [[<B>]<F>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<B>|Especifica a cor do plano de fundo.|
|\<F>|Especifica a cor de primeiro plano.|
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
    |A|Verde claro|
    |B|Azul-piscina claro|
    |C|Vermelho-claro|
    |D|Roxo-claro|
    |E|Amarelo-claro|
    |F|Branco brilhante|
-   Não use caracteres de espaço entre *B* e *F*.
-   Se você especificar apenas um dígito hexadecimal, a cor correspondente é usada como a cor de primeiro plano e a cor do plano de fundo é definida como a cor padrão.
-   Para definir a cor padrão da janela de Prompt de comando, clique no canto superior esquerdo da janela do Prompt de comando, clique em **padrões**, clique no **cores** guia e, em seguida, clique nas cores que você deseja usar para a  **Tela de texto** e **tela de fundo**.
-   Se *B* e *F* são os mesmos, o **cor** comando define ERRORLEVEL para 1, e nenhuma alteração é feita em primeiro plano ou a cor do plano de fundo.

## <a name="BKMK_examples"></a>Exemplos

Para alterar a cor de plano de fundo da janela de Prompt de comando como cinza e a cor de primeiro plano para vermelho, digite:
```
color 84
```
Para alterar a cor de primeiro plano da janela de Prompt de comando para amarelo-claro, digite:
```
color e
```

> [!NOTE]
> Neste exemplo, o plano de fundo é definido para a cor padrão porque apenas um dígito hexadecimal é especificado.

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)