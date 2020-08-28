---
title: start
description: Artigo de referência para Start, que inicia uma janela de prompt de comando separada para executar um programa ou comando especificado.
ms.topic: reference
ms.assetid: 0173f9b3-5cd7-4edb-b01e-d02193b4fadc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5f17b4093bbe82d869ad561dce45437389dc347e
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89036924"
---
# <a name="start"></a>start

Inicia uma janela de prompt de comando separada para executar um programa ou comando especificado.



## <a name="syntax"></a>Sintaxe

```
start [<Title>] [/d <Path>] [/i] [{/min | /max}] [{/separate | /shared}] [{/low | /normal | /high | /realtime | /abovenormal | belownormal}] [/affinity <HexAffinity>] [/wait] [/elevate] [/b] [<Command> [<Parameter>... ] | <Program> [<Parameter>... ]]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Title>|Especifica o título a ser exibido na barra de título da janela do prompt de comando.|
|/d \<Path>|Especifica o diretório de inicialização.|
|/i|Passa o ambiente de inicialização Cmd.exe para a nova janela do prompt de comando. Se **/i** não for especificado, o ambiente atual será usado.|
|/Min \| /Max|Especifica para minimizar (**/min**) ou maximizar (**/Max**) a nova janela do prompt de comando.|
|/Separate \| /Shared|Inicia programas de 16 bits em um espaço de memória separado (**/separate**) ou espaço de memória compartilhada (**/Shared**). Essas opções não têm suporte em plataformas de 64 bits.|
|/Low \| /normal \| /High \| /Realtime \| /AboveNormal \| /BelowNormal|Inicia um aplicativo na classe de prioridade especificada. Os valores de classe de prioridade válidos são **/Low**, **/normal**, **/High**, **/Realtime**, **/AboveNormal**e **/BelowNormal**.|
|/affinity \<HexAffinity>|Aplica a máscara de afinidade de processador especificada (expressa como um número hexadecimal) ao novo aplicativo.|
|/Wait|Inicia um aplicativo e aguarda sua finalização.|
|/elevate|Executa o aplicativo como administrador.|
|/b|Inicia um aplicativo sem abrir uma nova janela de prompt de comando. A manipulação CTRL + C é ignorada, a menos que o aplicativo permita o processamento de CTRL + C. Use CTRL + BREAK para interromper o aplicativo.|
|\<Command> \| \<Program>|Especifica o comando ou programa a ser iniciado.|
|\<Parameter>...|Especifica os parâmetros a serem passados para o comando ou o programa.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

- Você pode executar arquivos não executáveis por meio de sua associação de arquivo digitando o nome do arquivo como um comando.
- Quando você executa um comando que contém a cadeia de caracteres CMD como o primeiro token sem um qualificador de extensão ou caminho, o CMD é substituído pelo valor da variável COMSPEC. Isso impede que os usuários escolham **cmd** do diretório atual.
- Quando você executa um aplicativo de GUI (interface gráfica do usuário) de 32 bits, o **cmd** não aguarda o encerramento do aplicativo antes de retornar ao prompt de comando. Esse comportamento não ocorrerá se você executar o aplicativo a partir de um script de comando.
- Quando você executa um comando que usa um primeiro token que não contém uma extensão, Cmd.exe usa o valor da variável de ambiente PATHEXT para determinar quais extensões procurar e em qual ordem. O valor padrão para a variável PATHEXT é:
  ```
  .COM;.EXE;.BAT;.CMD;.VBS;.VBE;.JS;.JSE;.WSF;.WSH;.MSC
  ```
  Observe que a sintaxe é a mesma que a variável PATH, com ponto-e-vírgula separando cada extensão.
- Quando ele pesquisa um arquivo executável, se não houver nenhuma correspondência em nenhuma extensão, **Iniciar** verificará se o nome corresponde a um nome de diretório. Se tiver, **Iniciar** abrirá Explorer.exe nesse caminho.

## <a name="examples"></a>Exemplos

Para iniciar o programa MyApp no prompt de comando e manter o uso da janela do prompt de comando atual, digite:
```
start myapp
```
Para exibir o tópico da ajuda da linha de comando **inicial** em uma janela separada do prompt de comando maximizada, digite:
```
start /max start /?
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
