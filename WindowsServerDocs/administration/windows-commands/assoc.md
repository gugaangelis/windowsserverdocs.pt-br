---
title: assoc
description: Tópico de comandos do Windows para **assoc** -exibe ou modifica associações de extensão de nome de arquivo.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 237bedda-b24c-4fec-a39c-9b7eacf96417
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d893167081b66c81366b59613c52182a4ddba370
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816637"
---
# <a name="assoc"></a>assoc



Exibe ou modifica associações de extensão de nome de arquivo. Se usado sem parâmetros, **assoc** exibe uma lista de todas as atuais nome extensão associações de arquivo.

> [!NOTE]
> Este comando só tem suporte no CMD. EXE e não está disponível no PowerShell.
>

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
assoc [<.ext>[=[<FileType>]]]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|<.ext>|Especifica a extensão de nome de arquivo.|
|\<FileType>|Especifica o tipo de arquivo a ser associado com a extensão de nome de arquivo especificado.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Para remover a associação de tipo de arquivo para uma extensão de nome de arquivo, adicione um espaço em branco após o sinal de igual, pressionando a barra de espaços.
-   Para exibir os tipos de arquivo atuais que têm cadeias de caracteres de comando aberto definidas, use o **ftype** comando.
-   Para redirecionar a saída de **assoc** em um arquivo de texto, use o **>** operador de redirecionamento.

## <a name="BKMK_examples"></a>Exemplos

Para exibir a associação de tipo de arquivo atual para a extensão de nome de arquivo. txt, digite:
```
assoc .txt
```
Para remover a associação de tipo de arquivo para a extensão de nome de arquivo. bak, digite:
```
assoc .bak= 
```

> [!NOTE]
> Certifique-se de adicionar um espaço após o sinal de igual.

Para exibir a saída de **assoc** uma tela por vez, tipo:
```
assoc | more
```
Para enviar a saída de **assoc** para assoc.txt o arquivo, digite:
```
assoc>assoc.txt
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
