---
title: editar
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4e0ff2e8-3518-47c1-8c69-5e93f895fa0e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4dfffefbe113bc5df242a00357c769aaa9c1c787
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80845219"
---
# <a name="edit"></a>editar



Inicia o editor do MS-DOS, que cria e altera arquivos de texto ASCII.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
edit [/b] [/h] [/r] [/s] [/<NNN>] [[<Drive>:][<Path>]<FileName> [<FileName2> [...]]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[\<drive >:] [<Path>]<FileName> [<FileName2> [...]]|Especifica o local e o nome de um ou mais arquivos de texto ASCII. Se o arquivo não existir, o editor do MS-DOS o criará. Se o arquivo existir, o editor do MS-DOS o abrirá e exibirá seu conteúdo na tela. O *nome do arquivo* pode conter **&#42;** caracteres curinga (e **?** ). Separe vários nomes de arquivo com espaços.|
|/b|Força o modo monocromático, para que o editor do MS-DOS seja exibido em preto e branco.|
|/h|Exibe o número máximo de linhas possíveis para o monitor atual.|
|/r|Carrega arquivo (s) no modo somente leitura.|
|/s|Força o uso de nomes de FileName curtos.|
|\<NNN >|Carrega arquivo (s) binários, encapsulando linhas em *nnn* caracteres de largura.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Para obter ajuda adicional, abra o editor do MS-DOS e pressione a tecla F1.
-   Alguns monitores não dão suporte à exibição de teclas de atalho por padrão. Se o monitor não exibir as teclas de atalho, use **/b**.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para abrir o editor do MS-DOS, digite:
```
edit
```
Para criar e editar um arquivo chamado newtextfile. txt no diretório atual, digite:
```
edit newtextfile.txt
```