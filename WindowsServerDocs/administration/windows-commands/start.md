---
title: start
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0173f9b3-5cd7-4edb-b01e-d02193b4fadc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 59927015f63243f16ba6e9674bc74adbd3c4f96a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852627"
---
# <a name="start"></a>start



Inicia uma janela de Prompt de comando separada para executar um programa ou comando especificado.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
start ["<Title>"] [/d <Path>] [/i] [{/min | /max}] [{/separate | /shared}] [{/low | /normal | /high | /realtime | /abovenormal | belownormal}] [/affinity <HexAffinity>] [/wait] [/b {<Command> | <Program>} [<Parameters>]]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|"\<Title >"|Especifica o título a ser exibido na barra de título da janela de Prompt de comando.|
|/d \<Path>|Especifica o diretório de inicialização.|
|/i|Passa o ambiente de inicialização Cmd.exe para a nova janela de Prompt de comando. Se **/i** não for especificado, o ambiente atual é usado.|
|/min  \| /máx|Especifica para minimizar (**/min**) ou maximizar (**/máx**) a nova janela de Prompt de comando.|
|/separate \| / compartilhado|Inicia programas de 16 bits em um espaço de memória separados (**/separate**) ou o espaço de memória compartilhado (**/ compartilhado**). Essas opções não têm suporte em plataformas de 64 bits.|
|/ baixa \| /normal \| alta \| /realtime \| /abovenormal \| /belownormal|Inicia um aplicativo na classe de prioridade especificada. Valores de classe de prioridade válidos são **/baixa**, **/normal**, **alta**, **/realtime**, **/abovenormal**, e **/belownormal**.|
|/affinity \<HexAffinity >|Aplica-se a máscara de afinidade de processador especificado (expressada como um número hexadecimal) para o novo aplicativo.|
|/wait|Inicia um aplicativo e aguarda até que ele termine.|
|/b|Inicia um aplicativo sem abrir uma nova janela de Prompt de comando. Tratamento de CTRL + C é ignorado, a menos que o aplicativo permite o processamento de CTRL + C. Use CTRL + BREAK para interromper o aplicativo.|
|/ b \<comando > \| \<programa >|Especifica o comando ou um programa para iniciar.|
|\<Parâmetros >|Especifica os parâmetros a serem passados para o programa ou comando.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Você pode executar arquivos não executáveis através de sua associação de arquivo, digitando o nome do arquivo como um comando.
-   Quando você executar um comando que contém a cadeia de caracteres "CMD" como o primeiro token sem um qualificador de extensão ou caminho, "CMD" é substituído pelo valor da variável COMSPEC. Isso impede que os usuários pegar **cmd** do diretório atual.
-   Quando você executa um aplicativo de GUI (interface) de 32 bits gráfica do usuário, **cmd** não espera até que o aplicativo sair antes de retornar ao prompt de comando. Esse comportamento não ocorre se você executar o aplicativo a partir de um script de comando.
-   Quando você executa um comando que usa o primeiro símbolo não contém uma extensão, Cmd.exe usa o valor da variável de ambiente PATHEXT para determinar quais extensões procurar e em qual ordem. O valor padrão da variável PATHEXT é:  
    ```
    .COM;.EXE;.BAT;.CMD 
    ```  
    Observe que a sintaxe é o mesmo que a variável de caminho, com ponto e vírgula, separando cada extensão.
-   Ao procurar um arquivo executável, se não houver nenhuma correspondência em qualquer extensão, **iniciar** verifica se o nome corresponde a um nome de diretório. Se isso acontecer, **iniciar** abre Explorer.exe nesse caminho.

## <a name="BKMK_examples"></a>Exemplos

Para iniciar o programa Myapp no prompt de comando e manter o uso da janela de Prompt de comando atual, digite:
```
start myapp 
```
Para exibir o **iniciar** tópico da Ajuda de linha de comando em um separado maximizado a janela de Prompt de comando, digite:
```
start /max start /?
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
