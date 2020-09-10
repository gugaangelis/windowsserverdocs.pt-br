---
title: setlocal
description: Artigo de referência para setlocal, que inicia a localização de variáveis de ambiente em um arquivo em lotes.
ms.topic: reference
ms.assetid: e4e4b6d3-3f1a-4851-a782-25ee2470e16e
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 807bcb1d5694617f9632e88a4bc200a714048cc9
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639462"
---
# <a name="setlocal"></a>setlocal

Inicia a localização de variáveis de ambiente em um arquivo em lotes. A localização continua até que um comando **ENDLOCAL** correspondente seja encontrado ou o final do arquivo em lotes seja atingido.



## <a name="syntax"></a>Sintaxe

```
setlocal [enableextensions | disableextensions] [enabledelayedexpansion | disabledelayedexpansion]
```

## <a name="arguments"></a>Argumentos

|Argumento|Descrição|
|--------|-----------|
|enableextensions|Habilita as extensões de comando até que o comando **ENDLOCAL** correspondente seja encontrado, independentemente da configuração antes da execução do comando **setlocal** .|
|disableextensions|Desabilita as extensões de comando até que o comando **ENDLOCAL** correspondente seja encontrado, independentemente da configuração antes da execução do comando **setlocal** .|
|enabledelayedexpansion|Habilita a expansão da variável de ambiente atrasada até que o comando **ENDLOCAL** correspondente seja encontrado, independentemente da configuração antes da execução do comando **setlocal** .|
|disabledelayedexpansion|Desabilita a expansão da variável de ambiente atrasada até que o comando **ENDLOCAL** correspondente seja encontrado, independentemente da configuração antes da execução do comando **setlocal** .|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Usando **setlocal**

    Quando você usa **setlocal** fora de um script ou arquivo em lotes, ele não tem nenhum efeito.
-   Alterando variáveis ambientais

    Use **setlocal** para alterar variáveis de ambiente ao executar um arquivo em lotes. As alterações de ambiente feitas após a execução de **setlocal** são locais para o arquivo em lotes. O programa de Cmd.exe restaura as configurações anteriores quando encontra um comando **ENDLOCAL** ou atinge o final do arquivo em lotes.
-   Comandos de aninhamento

    Você pode ter mais de um comando **setlocal** ou **ENDLOCAL** em um programa em lotes (ou seja, comandos aninhados).
-   Testando extensões de comando em arquivos em lotes

    O comando **setlocal** define a variável ERRORLEVEL. Se você passar {**ENABLEEXTENSIONS**  |  **DISABLEEXTENSIONS**} ou {**enabledelayedexpansion**  |  **disabledelayedexpansion**}, a variável ERRORLEVEL será definida como **0** (zero). Caso contrário, ele será definido como **1**. Você pode usar essas informações em scripts do lote para determinar se as extensões estão disponíveis, conforme mostrado no exemplo a seguir:
    ```
    setlocal enableextensions
    verify other 2>nul
    if errorlevel 1 echo Unable to enable extensions
    ```
    Como o **cmd** não define a variável ERRORLEVEL quando as extensões de comando são desabilitadas, o comando **Verify** Inicializa a variável ERRORLEVEL para um valor diferente de zero quando você a usa com um argumento inválido. Além disso, se você usar o comando **setlocal** com argumentos {**ENABLEEXTENSIONS**  |  **DISABLEEXTENSIONS**} ou {**enabledelayedexpansion**  |  **disabledelayedexpansion**} e não definir a variável ERRORLEVEL como **1**, as extensões de comando não estarão disponíveis.

## <a name="examples"></a>Exemplos

Você pode localizar variáveis de ambiente em um arquivo em lotes, conforme mostrado no seguinte script de exemplo:
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

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)