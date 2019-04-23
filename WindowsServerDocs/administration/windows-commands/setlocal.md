---
title: setlocal
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e4e4b6d3-3f1a-4851-a782-25ee2470e16e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 70e58e3c3a7c3de594c620f7530816b57727d4c3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868857"
---
# <a name="setlocal"></a>setlocal



Inicia a localização de variáveis de ambiente em um arquivo em lotes. Localização continua até encontrar uma correspondência **endlocal** comando é encontrado ou o final do arquivo em lotes for atingido.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
setlocal [enableextensions | disableextensions] [enabledelayedexpansion | disabledelayedexpansion]
```

## <a name="arguments"></a>Argumentos

|Argumento|Descrição|
|--------|-----------|
|enableextensions|Permite que as extensões de comando até que a correspondência **endlocal** comando seja encontrado, independentemente da configuração antes do **setlocal** comando foi executado.|
|disableextensions|Desabilita as extensões de comando até que a correspondência **endlocal** comando seja encontrado, independentemente da configuração antes do **setlocal** comando foi executado.|
|enabledelayedexpansion|Habilita a expansão de variáveis de ambiente atrasada até que a correspondência **endlocal** comando seja encontrado, independentemente da configuração antes do **setlocal** comando foi executado.|
|disabledelayedexpansion|Desabilita a expansão de variáveis de ambiente atrasada até que a correspondência **endlocal** comando seja encontrado, independentemente da configuração antes do **setlocal** comando foi executado.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Usando **setlocal**

    Quando você usa **setlocal** fora de um arquivo de script ou lote, ele não tem nenhum efeito.
-   Alterar variáveis de ambiente

    Use **setlocal** para alterar as variáveis de ambiente quando você executa um arquivo em lotes. As alterações de ambiente feitas após a execução **setlocal** são locais para o arquivo em lotes. O programa Cmd.exe restaura as configurações anteriores quando encontra uma **endlocal** de comando ou atinge o final do arquivo em lotes.
-   Comandos de aninhamento

    Você pode ter mais de um **setlocal** ou **endlocal** comando em um arquivo em lotes (ou seja, comandos aninhados).
-   Testando extensões de comando em arquivos em lotes

    O **setlocal** comando define a variável ERRORLEVEL. Se você passar {**enableextensions** | **disableextensions**} ou {**enabledelayedexpansion**  |   **DISABLEDELAYEDEXPANSION**}, a variável ERRORLEVEL é definida como **0** (zero). Caso contrário, ele é definido como **1**. Você pode usar essas informações em scripts em lotes para determinar se as extensões estão disponíveis, conforme mostrado no exemplo a seguir:  
    ```
    setlocal enableextensions
    verify other 2>nul
    if errorlevel 1 echo Unable to enable extensions
    ```  
    Porque **cmd** não define a variável ERRORLEVEL quando as extensões de comando estão desabilitadas, o **verificar** comando inicializa a variável ERRORLEVEL como um valor diferente de zero quando usá-lo com um inválido argumento. Além disso, se você usar o **setlocal** comando com argumentos {**enableextensions** | **disableextensions**} ou {**enabledelayedexpansion**   |  **disabledelayedexpansion**} e não define a variável ERRORLEVEL com **1**, as extensões de comando não estão disponíveis.

## <a name="BKMK_examples"></a>Exemplos

Você pode localizar as variáveis de ambiente em um arquivo em lotes, conforme mostrado no script de exemplo a seguir:
```
rem *******Begin Comment**************
rem This program starts the superapp batch program on the network,
rem directs the output to a file, and displays the file
rem in Notepad.
rem *******End Comment**************
@echo off
setlocal
path=g:\programs\superapp;%path%
call superapp>c:\superapp.out
endlocal
start notepad c:\superapp.out
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)