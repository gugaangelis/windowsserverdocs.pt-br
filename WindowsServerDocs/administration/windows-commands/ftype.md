---
title: ftype
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: c29ca0aa027d11fa8f981134e5367021227d3096
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881677"
---
# <a name="ftype"></a>ftype



Exibe ou modifica tipos de arquivos que são usados em associações de extensão de nome de arquivo. Se usado sem um operador de atribuição (**=**), **ftype** exibe a cadeia de caracteres de comando aberta atual para o tipo de arquivo especificado. Se usado sem parâmetros, **ftype** exibe os tipos de arquivos que têm cadeias de caracteres de comando aberto definidas.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
ftype [<FileType>[=[<OpenCommandString>]]]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<FileType>|Especifica o tipo de arquivo para exibir ou alterar.|
|\<OpenCommandString>|Especifica a cadeia de caracteres de comando aberto para usar ao abrir arquivos do tipo de arquivo especificado.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

A tabela a seguir descreve como **ftype** substitui variáveis em uma cadeia de caracteres de comando aberta:

|Variável|Valor de substituição|
|--------|-----------------|
|%0 ou %1|Obtém substituída com o nome de arquivo que está sendo iniciado por meio da associação.|
|%*|Obtém todos os parâmetros.|
|%2, %3, ...|Obtém o primeiro parâmetro (%2), o segundo parâmetro (%3) e assim por diante.|
|%~\<N>|Obtém todos os parâmetros restantes, começando com o *N*º parâmetro, onde *N* pode ser qualquer número de 2 a 9.|

## <a name="BKMK_examples"></a>Exemplos

Para exibir os tipos de arquivo atual que têm cadeias de caracteres de comando aberto definidas, digite:
```
ftype
```
Para exibir a cadeia de caracteres de comando aberta atual para o *txtfile* tipo de arquivo, tipo:
```
ftype txtfile
```
Esse comando gera uma saída semelhante à seguinte:
```
txtfile=%SystemRoot%\system32\NOTEPAD.EXE %1
```
Para excluir a cadeia de caracteres de comando aberto para um tipo de arquivo chamado *exemplo*, tipo:
```
ftype example=
```
Para associar a extensão de nome de arquivo. pl com o tipo de arquivo PerlScript e habilitar o tipo de arquivo PerlScript executar o PERL. EXE, digite os seguintes comandos:
```
assoc .pl=PerlScript 
ftype PerlScript=perl.exe %1 %*
```
Para eliminar a necessidade de digitar a extensão de nome de arquivo. pl ao invocar um script Perl, digite:
```
set PATHEXT=.pl;%PATHEXT%
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)