---
title: goto
description: Artigo de referência para o comando goto, que direciona cmd.exe para uma linha rotulada em um programa em lotes.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e0de1458-1f78-48ff-a746-c285a945a510
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: afc77f7837ddaeb0552052538537285f0d652682
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85924684"
---
# <a name="goto"></a>goto

Direciona cmd.exe para uma linha rotulada em um programa em lotes. Em um programa em lotes, esse comando direciona o processamento de comandos para uma linha identificada por um rótulo. Quando o rótulo é encontrado, o processamento continua começando com os comandos que começam na próxima linha.

## <a name="syntax"></a>Sintaxe

```
goto <label>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<label>` | Especifica uma cadeia de texto que é usada como um rótulo no programa em lotes. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

-  Se as extensões de comando estiverem habilitadas (o padrão) e você usar o comando **goto** com um rótulo de destino de **: EOF**, você transfere o controle para o final do arquivo de script do lote atual e sai do arquivo de script do lote sem definir um rótulo. Ao usar esse comando com o rótulo **: EOF** , você deve inserir dois-pontos antes do rótulo. Por exemplo: `goto:EOF`.

- Você pode usar espaços no parâmetro de *rótulo* , mas não pode incluir outros separadores (por exemplo, pontos-e-vírgulas (;) ou sinais de igual (=)).

- O valor de *rótulo* especificado deve corresponder a um rótulo no programa em lotes. O rótulo dentro do programa do lote deve começar com dois-pontos (:). Se uma linha começar com dois-pontos, ela será tratada como um rótulo e todos os comandos nessa linha serão ignorados. Se o programa do lote não contiver o rótulo que você especificar no parâmetro *Label* , o programa em lotes será interrompido e exibirá a seguinte mensagem: `Label not found` .

- Você pode usar **goto** com outros comandos para executar operações condicionais. Para obter mais informações sobre como usar **goto** para operações condicionais, consulte o [comando IF](if.md).

## <a name="examples"></a>Exemplos

O programa em lotes a seguir formata um disco na unidade A como um disco do sistema. Se a operação for bem-sucedida, o comando **goto** direcionará o processamento para o rótulo **: End** :

```
echo off
format a: /s
if not errorlevel 1 goto end
echo An error occurred during formatting.
:end
echo End of batch program.
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando cmd](cmd.md)

- [comando if](if.md)