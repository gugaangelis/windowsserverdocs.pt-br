---
title: editar
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4e0ff2e8-3518-47c1-8c69-5e93f895fa0e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 096632005b3e42dd941ccc7c72c08ead1d291b53
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873147"
---
# <a name="edit"></a>editar



Inicia o Editor do MS-DOS, que cria e altera os arquivos de texto ASCII.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
edit [/b] [/h] [/r] [/s] [/<NNN>] [[<Drive>:][<Path>]<FileName> [<FileName2> [...]]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[\<Drive>:][<Path>]<FileName> [<FileName2> [...]]|Especifica o local e o nome de um ou mais arquivos de texto ASCII. Se o arquivo não existir, o Editor do MS-DOS o criará. Se o arquivo existir, o Editor do MS-DOS abre e exibe seu conteúdo na tela. *Nome do arquivo* pode conter caracteres curinga (**&#42;** e **?**). Separe vários nomes de arquivos com espaços.|
|/b|Força modo monocromático, para que esse Editor do MS-DOS exibido em preto e branco.|
|/h|Exibe o número máximo de linhas possíveis para o monitor atual.|
|/r|Carrega o arquivo (s) no modo somente leitura.|
|/s|Força o uso de nomes de arquivo curtos.|
|\<NNN>|Carrega arquivos binários, quebra automática de linhas a serem *NNN* caracteres largos.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Para obter ajuda adicional, abra o Editor do MS-DOS e, em seguida, pressione a tecla F1.
-   Alguns monitores não dão suporte à exibição das teclas de atalho por padrão. Se o monitor não exibir teclas de atalho, use **/b**.

## <a name="BKMK_examples"></a>Exemplos

Para abrir o Editor do MS-DOS, digite:
```
edit
```
Para criar e editar um arquivo chamado newtextfile.txt no diretório atual, digite:
```
edit newtextfile.txt
```