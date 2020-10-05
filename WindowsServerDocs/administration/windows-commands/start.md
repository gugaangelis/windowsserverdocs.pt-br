---
title: start
description: Artigo de referência para o comando Start, que inicia uma janela de prompt de comando separada para executar um programa ou comando especificado.
ms.topic: reference
ms.assetid: 0173f9b3-5cd7-4edb-b01e-d02193b4fadc
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 6c154cd0c3cab4520f64c77d2ecc920dfc8bdcc9
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91718263"
---
# <a name="start"></a>start

Inicia uma janela de prompt de comando separada para executar um programa ou comando especificado.

## <a name="syntax"></a>Sintaxe

```
start [<title>] [/d <path>] [/i] [{/min | /max}] [{/separate | /shared}] [{/low | /normal | /high | /realtime | /abovenormal | belownormal}] [/affinity <hexaffinity>] [/wait] [/elevate] [/b] [<command> [<parameter>... ] | <program> [<parameter>... ]]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<title>` | Especifica o título a ser exibido na barra de título da janela do **prompt de comando** . |
| /d `<path>` | Especifica o diretório de inicialização. |
| /i | Passa o ambiente de inicialização Cmd.exe para a nova janela do **prompt de comando** . Se **/i** não for especificado, o ambiente atual será usado. |
| `{/min | /max}` | Especifica para minimizar (**/min**) ou maximizar (**/Max**) a nova janela do **prompt de comando** . |
| `{/separate | /shared}` | Inicia programas de 16 bits em um espaço de memória separado (**/separate**) ou espaço de memória compartilhada (**/Shared**). Essas opções não têm suporte em plataformas de 64 bits. |
| `{/low | /normal | /high | /realtime | /abovenormal | belownormal}` | Inicia um aplicativo na classe de prioridade especificada. |
| /affinity `<hexaffinity>` | Aplica a máscara de afinidade de processador especificada (expressa como um número hexadecimal) ao novo aplicativo. |
| /Wait | Inicia um aplicativo e aguarda sua finalização. |
| /elevate | Executa o aplicativo como administrador. |
| /b | Inicia um aplicativo sem abrir uma nova janela de **prompt de comando** . A manipulação CTRL + C é ignorada, a menos que o aplicativo permita o processamento de CTRL + C. Use CTRL + BREAK para interromper o aplicativo. |
| `[<command> [<parameter>... ] | <program> [<parameter>... ]]` | Especifica o comando ou programa a ser iniciado. |
| `<parameter>` | Especifica os parâmetros a serem passados para o comando ou o programa. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Você pode executar arquivos não executáveis por meio de sua associação de arquivo digitando o nome do arquivo como um comando.

- Se você executar um comando que contém a cadeia de caracteres CMD como o primeiro token sem um qualificador de extensão ou caminho, o CMD será substituído pelo valor da variável COMSPEC. Isso impede que os usuários escolham **cmd** do diretório atual.

- Se você executar um aplicativo de GUI (interface gráfica do usuário) de 32 bits, o **cmd** não aguardará que o aplicativo saia antes de retornar ao prompt de comando. Esse comportamento não ocorrerá se você executar o aplicativo a partir de um script de comando.

- Se você executar um comando que usa um primeiro token que não contém uma extensão, Cmd.exe usará o valor da variável de ambiente PATHEXT para determinar quais extensões procurar e em qual ordem. O valor padrão para a variável PATHEXT é:

  ```
  .COM;.EXE;.BAT;.CMD;.VBS;.VBE;.JS;.JSE;.WSF;.WSH;.MSC
  ```

  Observe que a sintaxe é a mesma que a variável PATH, com ponto-e-vírgula (;) separando cada extensão.

- Ao procurar um arquivo executável, se não houver nenhuma correspondência em nenhuma extensão, **Iniciar** verificará se o nome corresponde a um nome de diretório. Se tiver, **Iniciar** abrirá Explorer.exe nesse caminho.

## <a name="examples"></a>Exemplos

Para iniciar o programa *MyApp* no prompt de comando e manter o uso da janela do **prompt de comando** atual, digite:

```
start Myapp
```

Para exibir o tópico da ajuda da linha de comando **inicial** em uma janela separada do **prompt de comando** maximizada, digite:

```
start /max start /?
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
