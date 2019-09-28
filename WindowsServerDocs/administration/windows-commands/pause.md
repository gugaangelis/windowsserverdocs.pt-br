---
title: pause
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 6501859eacf30dd6c1e64f34eee29ff81bd78ec9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372373"
---
# <a name="pause"></a>pause



Suspende o processamento de um programa em lotes e exibe o seguinte prompt:
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

- Quando você executa o comando **Pause** , a seguinte mensagem é exibida:  
  ```
  Press any key to continue . . .
  ```  
- Se você pressionar CTRL + C para interromper um programa em lotes, a seguinte mensagem será exibida:  
  ```
  Terminate batch job (Y/N)?
  ```  
  Se você pressionar s (para Sim) em resposta a essa mensagem, o programa do lote terminará e controlará o retorno ao sistema operacional.
- Você pode inserir o comando **Pause** antes de uma seção do arquivo em lotes que você talvez não queira processar. Quando a **pausa** suspende o processamento do programa em lotes, você pode pressionar CTRL + C e, em seguida, pressionar Y para interromper o programa em lotes.

## <a name="BKMK_examples"></a>Disso

Para criar um programa em lotes que solicita que o usuário altere os discos em uma das unidades, digite:
```
@echo off 
:Begin 
copy a:*.* 
echo Put a new disk into drive A 
pause 
goto begin
```
Neste exemplo, todos os arquivos no disco na unidade A são copiados para o diretório atual. Depois que a mensagem solicita que você coloque um novo disco na unidade A, o comando **Pause** suspende o processamento para que você possa alterar os discos e, em seguida, pressionar qualquer tecla para retomar o processamento. Esse programa em lotes é executado em um loop interminável — o comando **goto Begin** envia o interpretador de comandos para o rótulo inicial do arquivo em lotes. Para interromper este programa em lotes, pressione CTRL + C e, em seguida, pressione Y.

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)