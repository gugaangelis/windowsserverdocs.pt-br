---
title: chcp
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc7b1c71-7b80-443d-9cf1-9bcf305aa1fd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d5784b052ff1d7084d68cca0589caf518b8e44a8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379532"
---
# <a name="chcp"></a>chcp



Altera a página de código do console ativo. Se usado sem parâmetros, **chcp** exibe o número da página de código do console ativo.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
chcp [<NNN>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<NNN >|Especifica a página de código.|
|/?|Exibe a ajuda no prompt de comando.|

A tabela a seguir lista cada página de código com suporte e seu país/região ou idioma:

|Página de código|País/região ou idioma|
|---------|--------------------------|
|437|Estados Unidos|
|850|Multilíngue (Latino I)|
|852|Eslavo (latino II)|
|855|Cirílico (russo)|
|857|Turco|
|860|Português|
|861|islandês|
|863|Canadá-francês|
|865|Nórdico|
|866|Russo|
|869|Grego moderno|
|936|Chinês|

## <a name="remarks"></a>Comentários

-   Somente a página de código OEM (fabricante original do equipamento) instalada com o Windows aparece corretamente em uma janela de prompt de comando que usa fontes de varredura. Outras páginas de código aparecem corretamente no modo de tela inteira ou em janelas de prompt de comando que usam fontes TrueType.
-   Você não precisa preparar as páginas de código (como no MS-DOS).
-   Os programas que você inicia depois de atribuir uma nova página de código usam a nova página de código. No entanto, os programas (exceto cmd. exe) que você inicia antes de atribuir a nova página de código usam a página de código original.

## <a name="BKMK_examples"></a>Disso

Para exibir a configuração da página de código ativa, digite:
```
chcp
```
É exibida uma mensagem semelhante à seguinte:

`Active code page: 437`

Para alterar a página de código ativa para 850 (multilíngue), digite:
```
chcp 850
```
Se a página de código especificada for inválida, a seguinte mensagem de erro será exibida:

`Invalid code page`

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
