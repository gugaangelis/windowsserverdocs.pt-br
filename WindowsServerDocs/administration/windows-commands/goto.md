---
title: goto
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e0de1458-1f78-48ff-a746-c285a945a510
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f1ad0190519d58bd879ae391f378d800760c204f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857527"
---
# <a name="goto"></a>goto



Direciona cmd.exe para uma linha identificada em um arquivo em lotes. Dentro de um arquivo em lotes, **goto** direciona o processamento de comandos para uma linha que é identificado por um rótulo. Quando o rótulo for encontrado, o processamento continuará começando com os comandos que começam na próxima linha.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
goto <Label> 
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Label>|Especifica uma cadeia de caracteres de texto que é usada como um rótulo no arquivo em lotes.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Trabalhando com as extensões de comando

    Se as extensões de comando são habilitadas (padrão), e você usar o **goto** com um rótulo de destino **: EOF**, transferir o controle para o final do arquivo de script em lotes atual e o arquivo de script em lotes de saída sem definir um rótulo. Quando você usa **goto** com o **: EOF** rótulo, você deve inserir dois-pontos antes do rótulo. Por exemplo:  
    ```
    goto:EOF
    ```  
-   Usando válida *rótulo* valores

    Você pode usar espaços na *rótulo* parâmetro, mas você não pode incluir outros separadores (por exemplo, ponto e vírgula ou sinais de igual).
-   Correspondência *rótulo* com o rótulo no arquivo em lotes

    O *rótulo* valor que você especificar deve corresponder a um rótulo no arquivo em lotes. O rótulo em lotes deve começar com dois-pontos (:). Se uma linha começa com dois-pontos, ele será tratado como um rótulo e todos os comandos nessa linha são ignorados. Se seu arquivo em lotes não contém o rótulo que você especificar na *rótulo*, esse programa para e exibe a seguinte mensagem:  
    ```
    Label not found
    ```  
-   Usando o **goto** para operações condicionais

    Você pode usar **goto** com outros comandos para executar operações condicionais. Para obter mais informações sobre como usar **goto** para operações condicionais, consulte o [se](if.md) referência do comando.

## <a name="BKMK_examples"></a>Exemplos

O programa de lote a seguir formata um disco na unidade A como disco do sistema. Se a operação for bem-sucedida, o **goto** comando direciona o processamento para o **: final** rótulo:
```
echo off
format a: /s
if not errorlevel 1 goto end
echo An error occurred during formatting.
:end
echo End of batch program. 
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)

[Cmd](cmd.md)

[If](if.md)