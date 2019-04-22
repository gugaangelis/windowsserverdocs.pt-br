---
title: wscript
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2fbaf193-cdbd-414c-84c9-bb5720f84c29
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 08/21/2018
ms.openlocfilehash: 771c1231ee5379ec797f535505839de8671e32a8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821297"
---
# <a name="wscript"></a>wscript



Host de Script do Windows fornece um ambiente no qual os usuários podem executar scripts em uma variedade de linguagens que usam uma variedade de modelos de objeto para executar tarefas.

## <a name="syntax"></a>Sintaxe

```
wscript [<scriptname>] [/b] [/d] [/e:<engine>] [{/h:cscript|/h:wscript}] [/i] [/job:<identifier>] [{/logo|/nologo}] [/s] [/t:<number>] [/x] [/?] [<ScriptArguments>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|ScriptName|Especifica o nome de arquivo e caminho do arquivo de script.|
|/b|Especifica o modo de lote, que não exibe alertas, erros de script ou prompts de entrada. Isso é o oposto da **/i**.|
|/d|Inicia o depurador.|
|/e|Especifica o mecanismo usado para executar o script. Isso permite executar scripts que usam uma extensão de nome de arquivo personalizado. Sem o parâmetro /e, você pode executar somente scripts que usam extensões de nome de arquivo registrados. Por exemplo, se você tentar executar este comando:<br>```cscript test.admin```<br>Você receberá essa mensagem de erro: Erro de entrada: Não há nenhum mecanismo de script para a extensão de arquivo ". Admin."<br>Uma vantagem de usar extensões de nome de arquivo não padrão é que ele protege contra acidentalmente clicar duas vezes em um script e executar algo que você realmente não queria executar. <br>Isso não cria uma associação permanente entre a extensão de nome de arquivo .admin e VBScript. Sempre que você executar um script que usa uma extensão de nome de arquivo .admin, você precisará usar o parâmetro /e.|
|/h:cscript|Registra **cscript.exe** como o host de script padrão para a execução de scripts.|
|/h:wscript|Registra **wscript.exe** como o host de script padrão para a execução de scripts. Esse é o padrão quando o **/h** opção for omitida.|
|/i|Especifica o modo interativo, que exibe alertas, erros de script e prompts de entrada.</br>Este é o padrão e o oposto da **/b**.|
|/job:\<identifier>|Executa o trabalho identificado pelo *identificador* em um **. wsf** arquivo de script.|
|/logo|Especifica que a faixa do Windows Script Host é exibida no console antes do script é executado.</br>Este é o padrão e o oposto da **/nologo**.|
|/nologo|Especifica que a faixa do Windows Script Host não é exibida antes do script é executado. Isso é o oposto da **/logo**.|
|/s|Salva as opções de prompt de comando atual para o usuário atual.|
|/t:\<number>|Especifica o tempo máximo que o script pode ser executado (em segundos). Você pode especificar até 32.767 segundos.</br>O padrão é sem limite de tempo.|
|/x|Inicia o script no depurador.|
|ScriptArguments|Especifica os argumentos passados para o script. Cada argumento de script deve ser precedido por uma barra (/).|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   A execução dessa tarefa não requer que você tenha credenciais administrativas. Portanto, como a prática recomendada de segurança, considere executar essa tarefa como um usuário sem credenciais administrativas.
-   Para abrir um prompt de comando, na tela **Inicial**, digite **cmd** e clique em **prompt de comando**.
-   Cada parâmetro é opcional. No entanto, você não pode especificar argumentos de script sem especificar um script. Se você não especificar um script ou argumentos de script **wscript.exe** exibe os **configurações do Windows Script Host** caixa de diálogo que você pode usar para definir propriedades de script globais para todos os scripts que **wscript.exe** é executado no computador local.
-   O **/t** parâmetro evita a execução excessiva de scripts definindo um timer. Quando o tempo excede o valor especificado, **wscript** interrompe o mecanismo de script e finaliza o processo.
-   Arquivos de script do Windows geralmente têm uma das seguintes extensões de nome de arquivo: **. wsf**, **. vbs**, **. js**.
-   Se você clicar duas vezes em um arquivo de script com uma extensão que não tem associação, o **abrir com** caixa de diálogo é exibida. Selecione **wscript** ou **cscript**e, em seguida, selecione **sempre usar esse programa para abrir este tipo de arquivo**. Isso registra **wscript.exe** ou **cscript.exe** como o host de script padrão para arquivos desse tipo de arquivo.
-   Você pode definir propriedades para scripts individuais. Ver [visão geral do Windows Script Host](https://technet.microsoft.com/library/cc738350(v=ws.10).aspx) para obter mais informações.
-   Pode usar o Windows Script Host **. wsf** arquivos de script. Cada **. wsf** arquivo pode usar vários mecanismos de script e executar vários trabalhos.

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
