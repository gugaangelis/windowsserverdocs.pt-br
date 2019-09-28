---
title: cscript
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 32cfd70266abdcdc247bf79b34ae220a73729837
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378803"
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
| Scriptname. extensão |                                 Especifica o caminho e o nome do arquivo de script com extensão de nome de arquivo opcional.                                 |
|          /B          |                                Especifica o modo de lote, que não exibe alertas, erros de script ou prompts de entrada.                                |
|          /D          |                                                                  Inicia o depurador.                                                                  |
|     /E: <Engine>      |                                                  Especifica o mecanismo usado para executar o script.                                                  |
|      /H: cscript      |                                         Registra o cscript. exe como o host de script padrão para executar scripts.                                          |
|      /H: WScript      |                               Registra o WScript. exe como o host de script padrão para executar scripts. Esse é o padrão.                               |
|          /I          |        Especifica o modo interativo, que exibe alertas, erros de script e prompts de entrada. Esse é o padrão e o oposto de **/b**.         |
|  /Minuto: <Identifier>   |                                             Executa o trabalho identificado pelo *identificador* em um arquivo de script. wsf.                                             |
|        /Logo         | Especifica que a faixa do host de scripts do Windows é exibida no console do antes de o script ser executado. Esse é o padrão e o oposto de **/nologo**. |
|       /Nologo        |                                 Especifica que a faixa do host de scripts do Windows não é exibida antes de o script ser executado.                                 |
|          /S          |                                             Salva as opções de prompt de comando atuais do usuário atual.                                             |
|     /T: <Seconds>     |            Especifica o tempo máximo que o script pode executar (em segundos). Você pode especificar até 32.767 segundos. O padrão é sem limite de tempo.             |
|          /U          |                                      Especifica o Unicode para entrada e saída que é redirecionado do console do.                                       |
|          /X          |                                                           Inicia o script no depurador.                                                           |
|          /?          |  Exibe os parâmetros de comando disponíveis e fornece ajuda para usá-los. Isso é o mesmo que digitar **cscript. exe** sem parâmetros e nenhum script.  |
|   ScriptArguments    |                        Especifica os argumentos passados para o script. Cada argumento de script deve ser precedido por uma barra ( **/** ).                         |

### <a name="remarks"></a>Comentários
-   A execução dessa tarefa não requer que você tenha credenciais administrativas. Portanto, como a prática recomendada de segurança, considere executar essa tarefa como um usuário sem credenciais administrativas.
-   Para abrir um prompt de comando, na tela **Iniciar** , digite **cmd**e clique em **prompt de comando**.
-   Cada parâmetro é opcional; no entanto, você não pode especificar argumentos de script sem especificar um script. Se você não especificar um script ou argumentos de script, o cscript. exe exibirá a sintaxe de cscript. exe e as opções de host válidas.
-   O parâmetro **/t** impede a execução excessiva de scripts por meio da definição de um temporizador. Quando o tempo de execução excede o valor especificado, o cscript interrompe o mecanismo de script e encerra o processo.
-   Os arquivos de script do Windows geralmente têm uma das seguintes extensões de nome de arquivo:. wsf,. vbs,. js.
-   Você pode definir propriedades para scripts individuais. Consulte os tópicos relacionados para obter mais informações.
-   O Windows Script Host pode usar arquivos de script. wsf. Cada arquivo. wsf pode usar vários mecanismos de script e executar vários trabalhos.
-   Se você clicar duas vezes em um arquivo de script com uma extensão que não tem associação, a caixa de diálogo **abrir com** será exibida. Selecione Wscript ou Cscript e, em seguida, selecione **sempre usar este programa para abrir este tipo de arquivo**. Isso registra o WScript. exe ou o Cscript como o host de script padrão para arquivos desse tipo de arquivo.
-   Você pode definir propriedades para scripts individuais. Consulte [referências adicionais](#BKMK_references) para obter mais informações.
-   O Windows Script Host pode usar arquivos de script. wsf. Cada arquivo. wsf pode usar vários mecanismos de script e executar vários trabalhos.

#### <a name="BKMK_references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
