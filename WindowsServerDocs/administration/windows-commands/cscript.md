---
title: cscript
description: Artigo de referência para o comando cscript, que inicia um script para que ele seja executado em um ambiente de linha de comando.
ms.topic: reference
ms.assetid: fba3cbca-594e-4663-bb22-4ee0f63a1ac6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6b5c711d59f69267f8e2de51f34cb1c450e95fab
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033024"
---
# <a name="cscript"></a>cscript

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Inicia um script para ser executado em um ambiente de linha de comando.

>[!IMPORTANT]
> A execução desta tarefa não exige que você possua credenciais administrativas. Portanto, como uma prática recomendada de segurança, considere a execução desta tarefa como um usuário sem credenciais administrativas.

## <a name="syntax"></a>Sintaxe

```
cscript <scriptname.extension> [/b] [/d] [/e:<engine>] [{/h:cscript | /h:wscript}] [/i] [/job:<identifier>] [{/logo | /nologo}] [/s] [/t:<seconds>] [x] [/u] [/?] [<scriptarguments>]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| scriptname. extensão | Especifica o caminho e o nome do arquivo de script com extensão de nome de arquivo opcional. |
| /b | Especifica o modo de lote, que não exibe alertas, erros de script ou prompts de entrada. |
| /d | Inicia o depurador. |
| /e:`<engine>` | Especifica o mecanismo usado para executar o script. |
| /h: cscript | Registra cscript.exe como o host de script padrão para executar scripts. |
| /h: WScript | Registra wscript.exe como o host de script padrão para executar scripts. Esse é o padrão. |
| /i | Especifica o modo interativo, que exibe alertas, erros de script e prompts de entrada. Esse é o padrão e o oposto de `/b` . |
| /minuto<identifier> | Executa o trabalho identificado pelo *identificador* em um arquivo de script. wsf. |
| /logo | Especifica que a faixa do host de scripts do Windows é exibida no console do antes de o script ser executado. Esse é o padrão e o oposto de `/nologo` . |
| /nologo | Especifica que a faixa do host de scripts do Windows não é exibida antes de o script ser executado. |
| /s | Salva as opções de prompt de comando atuais do usuário atual. |
| /t:<seconds> | Especifica o tempo máximo que o script pode executar (em segundos). Você pode especificar até 32.767 segundos. O padrão é sem limite de tempo. |
| /u | Especifica o Unicode para entrada e saída que é redirecionado do console do. |
| /x | Inicia o script no depurador. |
| /? | Exibe os parâmetros de comando disponíveis e fornece ajuda para usá-los. Isso é o mesmo que digitar **cscript.exe** sem parâmetros e nenhum script. |
| scriptarguments | Especifica os argumentos passados para o script. Cada argumento de script deve ser precedido por uma barra ( **/** ). |

#### <a name="remarks"></a>Comentários

- Cada parâmetro é opcional; no entanto, você não pode especificar argumentos de script sem especificar um script. Se você não especificar um script ou argumentos de script, cscript.exe exibirá a sintaxe de cscript.exe e as opções de host válidas.

- O parâmetro **/t** impede a execução excessiva de scripts por meio da definição de um temporizador. Quando o tempo de execução excede o valor especificado, o cscript interrompe o mecanismo de script e encerra o processo.

- Os arquivos de script do Windows geralmente têm uma das seguintes extensões de nome de arquivo:. wsf,. vbs,. js. O Windows Script Host pode usar arquivos de script. wsf. Cada arquivo. wsf pode usar vários mecanismos de script e executar vários trabalhos.

- Se você clicar duas vezes em um arquivo de script com uma extensão que não tem associação, a caixa de diálogo **abrir com** será exibida. Selecione Wscript ou Cscript e, em seguida, selecione **sempre usar este programa para abrir este tipo de arquivo**. Isso registra wscript.exe ou Cscript como o host de script padrão para arquivos desse tipo de arquivo.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
