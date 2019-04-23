---
title: pause
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cab3afc3-d046-432f-a0bf-6282f0099032
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 109d162e8d5c4bdd59871a21f16b6f568df4fbd6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861657"
---
# <a name="pause"></a>pause



Suspende o processamento de um arquivo em lotes e exibe o prompt a seguir:
```
Press any key to continue . . .
```
Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
pause
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Quando você executa o **pausar** de comando, a seguinte mensagem aparecerá:  
    ```
    Press any key to continue . . .
    ```  
-   Se você pressionar CTRL + C para interromper um programa em lote, a seguinte mensagem será exibida:  
    ```
    Terminate batch job (Y/N)?
    ```  
    Se você pressionar S (para Sim) em resposta a essa mensagem, o programa de lote será encerrado e o controle retorna para o sistema operacional.
-   Você pode inserir o **pausar** comando antes de uma seção do arquivo em lotes que você não deseja processar. Quando **pausar** suspende o processamento de lotes, você pode pressionar CTRL + C e, em seguida, pressione Y para interromper o programa.

## <a name="BKMK_examples"></a>Exemplos

Para criar um arquivo em lotes que solicita que o usuário altere os discos em uma das unidades, digite:
```
@echo off 
:Begin 
copy a:*.* 
echo Put a new disk into drive A 
pause 
goto begin
```
Neste exemplo, todos os arquivos no disco na unidade A são copiados para o diretório atual. Depois que a mensagem solicita que você coloque um novo disco na unidade A, o **pausar** comando suspende o processamento de modo que você pode alterar os discos e, em seguida, pressione qualquer tecla para continuar o processamento. Esse arquivo em lotes é executado em um loop infinito — o **goto começar** comando envia o interpretador de comandos para o início do arquivo em lotes. Para interromper esse arquivo em lotes, pressione CTRL + C e, em seguida, pressione Y.

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)