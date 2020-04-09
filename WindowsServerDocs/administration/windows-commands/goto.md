---
title: goto
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e0de1458-1f78-48ff-a746-c285a945a510
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 928a9031a7f86261789676257afe95ffc3be8a99
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842559"
---
# <a name="goto"></a>goto



Direciona o cmd. exe para uma linha rotulada em um programa em lotes. Em um programa em lotes, **goto** direciona o processamento de comandos para uma linha identificada por um rótulo. Quando o rótulo é encontrado, o processamento continua começando com os comandos que começam na próxima linha.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
goto <Label> 
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Rótulo de \<>|Especifica uma cadeia de texto que é usada como um rótulo no programa em lotes.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Trabalhando com extensões de comando

    Se as extensões de comando estiverem habilitadas (o padrão) e você usar o comando **goto** com um rótulo de destino de **: EOF**, você transfere o controle para o final do arquivo de script do lote atual e sai do arquivo de script do lote sem definir um rótulo. Ao usar **goto** com o rótulo **: EOF** , você deve inserir dois-pontos antes do rótulo. Por exemplo:  
    ```
    goto:EOF
    ```  
-   Usando valores de *rótulo* válidos

    Você pode usar espaços no parâmetro de *rótulo* , mas não pode incluir outros separadores (por exemplo, pontos-e-vírgulas ou sinais de igualdade).
-   *Rótulo* correspondente com o rótulo no programa em lotes

    O valor de *rótulo* especificado deve corresponder a um rótulo no programa em lotes. O rótulo dentro do programa do lote deve começar com dois-pontos (:). Se uma linha começar com dois-pontos, ela será tratada como um rótulo e todos os comandos nessa linha serão ignorados. Se o programa do lote não contiver o rótulo que você especificar no *rótulo*, o programa do lote será interrompido e exibirá a seguinte mensagem:  
    ```
    Label not found
    ```  
-   Usando **goto** para operações condicionais

    Você pode usar **goto** com outros comandos para executar operações condicionais. Para obter mais informações sobre como usar **goto** para operações condicionais, consulte a referência de comando [If](if.md) .

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O programa em lotes a seguir formata um disco na unidade A como um disco do sistema. Se a operação for bem-sucedida, o comando **goto** direcionará o processamento para o rótulo **: End** :
```
echo off
format a: /s
if not errorlevel 1 goto end
echo An error occurred during formatting.
:end
echo End of batch program. 
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

[Cmd](cmd.md)

[Que](if.md)