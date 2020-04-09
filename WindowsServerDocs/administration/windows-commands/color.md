---
title: cor
description: Tópico de comandos do Windows para cor, que altera as cores de primeiro e segundo plano na janela do prompt de comando da sessão atual.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f5b67131-d196-45ec-a3f9-b5d9f091fd86
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0e89c20d90a3b812fa67b597c4c205d34e725785
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847479"
---
# <a name="color"></a>cor

Altera as cores de primeiro e segundo plano na janela do prompt de comando para a sessão atual. Se usado sem parâmetros, **Color** restaura as cores de primeiro plano e tela de fundo da janela de prompt de comando padrão.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
color [[<B>]<F>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<B >|Especifica a cor do plano de fundo.|
|\<F >|Especifica a cor do primeiro plano.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   A tabela a seguir lista os dígitos hexadecimais válidos que você pode usar como os valores para *B* e *F*.

|{1&gt;Valor&lt;1}|Cor|
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
-   Para definir a cor da janela do prompt de comando padrão, clique no canto superior esquerdo da janela do prompt de comando, clique em **padrões**, clique na guia **cores** e clique nas cores que você deseja usar para o **texto da tela** e o plano de **fundo da tela**.
-   Se *B* e *F* forem os mesmos, o comando **Color** definirá ERRORLEVEL como 1 e nenhuma alteração será feita na cor de primeiro plano ou segundo plano.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

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

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
