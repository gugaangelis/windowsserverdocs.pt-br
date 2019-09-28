---
title: ftype
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6fb53cee-9bed-44dd-af5d-bc7cec1dd114
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ce3f4c360269eb9cabd2cbef8abb89935923a595
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71375817"
---
# <a name="ftype"></a>ftype



Exibe ou modifica os tipos de arquivo que são usados em associações de extensão de nome de arquivo. Se usado sem um operador de atribuição ( **=** ), **ftype** exibirá a cadeia de caracteres de comando aberta atual para o tipo de arquivo especificado. Se usado sem parâmetros, **ftype** exibirá os tipos de arquivo que têm cadeias de caracteres de comando abertas definidas.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
ftype [<FileType>[=[<OpenCommandString>]]]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<FileType >|Especifica o tipo de arquivo a ser exibido ou alterado.|
|\<OpenCommandString >|Especifica a cadeia de caracteres do comando Open a ser usada ao abrir arquivos do tipo de arquivo especificado.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

A tabela a seguir descreve como o **ftype** substitui variáveis dentro de uma cadeia de caracteres de comando Open:

|Variável|Valor de substituição|
|--------|-----------------|
|% 0 ou% 1|É substituído pelo nome do arquivo que está sendo iniciado por meio da associação.|
|%*|Obtém todos os parâmetros.|
|% 2,% 3,...|Obtém o primeiro parâmetro (% 2), o segundo parâmetro (% 3) e assim por diante.|
|%~ @ NO__T-1N >|Obtém todos os parâmetros restantes começando com o *n*-ésimo parâmetro, em que *n* pode ser qualquer número de 2 a 9.|

## <a name="BKMK_examples"></a>Disso

Para exibir os tipos de arquivo atuais que têm cadeias de comando abertas definidas, digite:
```
ftype
```
Para exibir a cadeia de comando Open atual para o tipo de arquivo *txtfile* , digite:
```
ftype txtfile
```
Esse comando produz uma saída semelhante à seguinte:
```
txtfile=%SystemRoot%\system32\NOTEPAD.EXE %1
```
Para excluir a cadeia de comando Open para um tipo de arquivo chamado *example*, digite:
```
ftype example=
```
Para associar a extensão de nome de arquivo. pl ao tipo de arquivo PerlScript e habilitar o tipo de arquivo PerlScript para executar o PERL. EXE, digite os seguintes comandos:
```
assoc .pl=PerlScript 
ftype PerlScript=perl.exe %1 %*
```
Para eliminar a necessidade de digitar a extensão de nome de arquivo. pl ao invocar um script Perl, digite:
```
set PATHEXT=.pl;%PATHEXT%
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)