---
title: cor
description: Artigo de referência para o comando Color, que altera as cores de primeiro e segundo plano na janela do prompt de comando da sessão atual.
ms.topic: reference
ms.assetid: f5b67131-d196-45ec-a3f9-b5d9f091fd86
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3ce7aa8e927e3796917d2720495f394636d9c240
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028454"
---
# <a name="color"></a>cor

Altera as cores de primeiro e segundo plano na janela do prompt de comando para a sessão atual. Se usado sem parâmetros, **Color** restaura as cores de primeiro plano e tela de fundo da janela de prompt de comando padrão.

## <a name="syntax"></a>Sintaxe

```
color [[<b>]<f>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<b>` | Especifica a cor do plano de fundo. |
| `<f>` | Especifica a cor do primeiro plano. |
| /? | Exibe a ajuda no prompt de comando. |

Em que:

A tabela a seguir lista os dígitos hexadecimais válidos que você pode usar como valores para `<b>` e `<f>` :

| Valor | Cor |
| ----- | ----- |
| 0 | Preto |
| 1 | Azul |
| 2 | Verde |
| 3 | Aqua |
| 4 | Vermelho |
| 5 | Roxo |
| 6 | Amarelo |
| 7 | Branco |
| 8 | Cinza |
| 9 | Azul-claro |
| um | verde-claro |
| b | Azul-claro |
| c | Vermelho-claro |
| d | Roxo-claro |
| e | Amarelo-claro |
| f | Branco brilhante |

#### <a name="remarks"></a>Comentários

- Não use caracteres de espaço entre `<b>` e `<f>` .

- Se você especificar apenas um dígito hexadecimal, a cor correspondente será usada como a cor de primeiro plano e a cor do plano de fundo será definida como a cor padrão.

- Para definir a cor da janela do prompt de comando padrão, selecione o canto superior esquerdo da janela do **prompt de comando** , selecione **padrões**, selecione a guia **cores** e, em seguida, selecione as cores que deseja usar para o **texto da tela** e o plano de **fundo da tela**.

- Se `<b>` e `<f>` for o mesmo valor de cor, ERRORLEVEL será definido como `1` , e nenhuma alteração será feita na cor de primeiro plano ou segundo plano.

## <a name="examples"></a>Exemplos

Para alterar a cor do plano de fundo da janela do prompt de comando para cinza e a cor do primeiro plano para vermelho, digite:

```
color 84
```

Para alterar a cor do primeiro plano da janela do prompt de comando para amarelo claro, digite:

```
color e
```

> [!NOTE]
> Neste exemplo, o plano de fundo é definido como a cor padrão porque apenas um dígito hexadecimal é especificado.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
