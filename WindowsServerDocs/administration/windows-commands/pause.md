---
title: pause
description: Tópico de referência para o comando Pause, que suspende o processamento de programas em lotes.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cab3afc3-d046-432f-a0bf-6282f0099032
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f603802926d0f9418a82e1f4981181889fc573ef
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85472512"
---
# <a name="pause"></a>pause

Suspende o processamento de um programa em lotes, exibindo o prompt,`Press any key to continue . . .`

## <a name="syntax"></a>Sintaxe

```
pause
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /? | Exibe a ajuda no prompt de comando. |

## <a name="remarks"></a>Comentários

- Se você pressionar CTRL + C para interromper um programa em lotes, a seguinte mensagem será exibida, `Terminate batch job (Y/N)?` . Se você pressionar **s** (para Sim) em resposta a essa mensagem, o programa do lote terminará e controlará o retorno ao sistema operacional.

- Você pode inserir o comando **Pause** antes de uma seção do arquivo em lotes que você talvez não queira processar. Quando a **pausa** suspende o processamento do programa em lotes, você pode pressionar CTRL + C e, em seguida, pressionar **Y** para interromper o programa em lotes.

## <a name="examples"></a>Exemplos

Para criar um programa em lotes que solicita que o usuário altere os discos em uma das unidades, digite:

```
@echo off
:Begin
copy a:*.*
echo Put a new disk into Drive A
pause
goto begin
```

Neste exemplo, todos os arquivos no disco na unidade A são copiados para o diretório atual. Depois que a mensagem solicita que você coloque um novo disco na unidade A, o comando **Pause** suspende o processamento para que você possa alterar os discos e, em seguida, pressionar qualquer tecla para retomar o processamento. Esse programa em lotes é executado em um loop interminável — o comando **goto Begin** envia o interpretador de comandos para o rótulo inicial do arquivo em lotes.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)