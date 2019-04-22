---
title: chcp
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 622d4b64128c7e39cc761e4f5e9d69cf54383760
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819197"
---
# <a name="chcp"></a>chcp



Altera a página de código do console do Active Directory. Se usado sem parâmetros, **chcp** exibe o número da página de código ativa do console.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
chcp [<NNN>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<NNN>|Especifica a página de código.|
|/?|Exibe a ajuda no prompt de comando.|

A tabela a seguir lista cada código com suporte de página e seu país/região ou idioma:

|Página de código|País/região ou idioma|
|---------|--------------------------|
|437|Estados Unidos|
|850|Multilíngue (Latino I)|
|852|Eslavo (Latim II)|
|855|Cirílico (Russo)|
|857|Turco|
|860|Português|
|861|islandês|
|863|Francês (Canadá)|
|865|Nórdico|
|866|Russo|
|869|Grego moderno|
|936|Chinês|

## <a name="remarks"></a>Comentários

-   Somente a página de código do fabricante original do equipamento (OEM) é instalada com o Windows é exibida corretamente em uma janela de Prompt de comando que usa fontes de varredura. Outras páginas de código são exibidas corretamente no modo de tela inteira ou nas janelas de Prompt de comando que usam fontes TrueType.
-   Não é necessário preparar as páginas de código (como no MS-DOS).
-   Programas iniciados depois que você atribuem uma nova página de código usam a nova página de código. No entanto, os programas (exceto Cmd.exe) que você iniciar antes de você atribuir a nova página de código usam a página de código original.

## <a name="BKMK_examples"></a>Exemplos

Para exibir a configuração de página de código ativo, digite:
```
chcp
```
Será exibida uma mensagem semelhante à seguinte:

`Active code page: 437`

Para alterar a página de código ativa para 850 (multilíngue), digite:
```
chcp 850
```
Se a página de código especificada é inválida, a seguinte mensagem de erro será exibida:

`Invalid code page`

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
