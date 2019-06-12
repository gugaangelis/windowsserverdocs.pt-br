---
title: cscript
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fba3cbca-594e-4663-bb22-4ee0f63a1ac6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ef98a98088e345f267aa852318cee6e237604aa4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433995"
---
# <a name="cscript"></a>cscript

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

inicia um script para que ele seja executado em um ambiente de linha de comando.
## <a name="syntax"></a>Sintaxe
```
cscript <Scriptname.extension> [/B] [/D] [/E:<Engine>] [{/H:cscript|/H:wscript}] [/I] [/Job:<Identifier>] [{/Logo|/NoLogo}] [/S] [/T:<Seconds>] [/X] [/U] [/?] [<ScriptArguments>]
```
### <a name="parameters"></a>Parâmetros

|      Parâmetro       |                                                                      Descrição                                                                       |
|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------|
| Scriptname.extension |                                 Especifica o nome de arquivo e caminho do arquivo de script com a extensão de nome de arquivo opcional.                                 |
|          /B          |                                Especifica o modo de lote, que não exibe alertas, erros de script ou prompts de entrada.                                |
|          /D          |                                                                  Inicia o depurador.                                                                  |
|     /E:<Engine>      |                                                  Especifica o mecanismo usado para executar o script.                                                  |
|      /H:cscript      |                                         Registra cscript.exe como host de script padrão para a execução de scripts.                                          |
|      /H:wscript      |                               Registra wscript.exe como host de script padrão para a execução de scripts. Este é o padrão.                               |
|          /I          |        Especifica o modo interativo, que exibe alertas, erros de script e prompts de entrada. Este é o padrão e o oposto da **/b**.         |
|  /Job:<Identifier>   |                                             Executa o trabalho identificado pelo *identificador* em um arquivo de script. wsf.                                             |
|        / Logotipo         | Especifica que a faixa do Windows Script Host é exibida no console antes do script é executado. Este é o padrão e o oposto da **/Nologo**. |
|       /Nologo        |                                 Especifica que a faixa do Windows Script Host não é exibida antes do script é executado.                                 |
|          /S          |                                             Salva as opções de prompt de comando atuais para o usuário atual.                                             |
|     /T:<Seconds>     |            Especifica o tempo máximo que o script pode ser executado (em segundos). Você pode especificar até 32.767 segundos. O padrão é sem limite de tempo.             |
|          /U          |                                      Especifica o Unicode para entrada e saída é redirecionada do console.                                       |
|          /X          |                                                           Inicia o script no depurador.                                                           |
|          /?          |  Exibe os parâmetros de comando disponíveis e fornece ajuda para usá-los. Isso é o mesmo que digitar **cscript.exe** sem parâmetros e nenhum script.  |
|   ScriptArguments    |                        Especifica os argumentos passados para o script. Cada argumento de script deve ser precedido por uma barra ( **/** ).                         |

### <a name="remarks"></a>Comentários
-   A execução dessa tarefa não requer que você tenha credenciais administrativas. Portanto, como a prática recomendada de segurança, considere executar essa tarefa como um usuário sem credenciais administrativas.
-   Para abrir um prompt de comando na **inicie** tela, digite **cmd**e, em seguida, clique em **prompt de comando**.
-   Cada parâmetro é opcional. No entanto, você não pode especificar argumentos de script sem especificar um script. Se você não especificar um script ou argumentos de script, cscript.exe exibe a sintaxe cscript.exe e as opções de host válido.
-   O **/T** parâmetro evita a execução excessiva de scripts definindo um timer. Quando o tempo de execução excede o valor especificado, o cscript interrompe o mecanismo de script e finaliza o processo.
-   Arquivos de script do Windows geralmente têm uma das seguintes extensões de nome de arquivo:. wsf,. vbs,. js.
-   Você pode definir propriedades para scripts individuais. Consulte os tópicos relacionados para obter mais informações.
-   Host de Script do Windows pode usar arquivos de script. wsf. Cada arquivo. wsf pode usar vários mecanismos de script e executar vários trabalhos.
-   Se você clicar duas vezes em um arquivo de script com uma extensão que não tem associação, o **abrir com** caixa de diálogo é exibida. Selecione wscript ou cscript e, em seguida, selecione **sempre usar esse programa para abrir este tipo de arquivo**. Isso registra wscript.exe ou cscript como o host de script padrão para arquivos desse tipo de arquivo.
-   Você pode definir propriedades para scripts individuais. Ver [referências adicionais](#BKMK_references) para obter mais informações.
-   Host de Script do Windows pode usar arquivos de script. wsf. Cada arquivo. wsf pode usar vários mecanismos de script e executar vários trabalhos.

#### <a name="BKMK_references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
