---
title: shift
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b56574e8-570a-4cc9-bbac-1b94fbf6a47a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e72b4be1b265d682d489cf372cdfe5ef54bb444d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441245"
---
# <a name="shift"></a>shift



Altera a posição de parâmetros em lotes em um arquivo em lotes.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
shift [/n <N>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/n \<N>|Especifica para iniciar a alternância na *N*argumento, onde *N* é qualquer valor de 0 a 8. Requer as extensões de comando, que são habilitadas por padrão.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

- O **shift** comando altera os valores dos parâmetros em lotes **%0** por meio do **%9** copiando cada parâmetro para o anterior — o valor de **%1** é copiado **%0**, o valor de **%2** é copiado para **%1**e assim por diante. Isso é útil para gravar um arquivo em lotes que executa a mesma operação em qualquer número de parâmetros.
- Se as extensões de comando estiverem habilitadas, o **shift** comando dá suporte a **/n** opção de linha de comando. O **/n** opção especifica para iniciar a alternância no enésimo argumento, onde **N** é qualquer valor de 0 a 8. Por exemplo, **e de deslocamento/2** mudaria **%3** para **%2**, **%4** para **%3**e assim por diante e deixe **%0** e **%1** afetados. As extensões de comando são habilitadas por padrão.
- Você pode usar o **shift** comando para criar um arquivo em lotes que pode aceitar parâmetros de lote mais de 10. Se você especificar mais de 10 parâmetros na linha de comando, aqueles que são exibidos após o décimo ( **%9**) será deslocada um por vez no **%9**.
- O **shift** comando não tem efeito sobre o **% \\** * parâmetro do lote.
- Não há não com versões anteriores **shift** comando. Depois de implementar o **shift** de comando, não é possível recuperar o parâmetro de lote ( **%0**) que existia antes do deslocamento.

## <a name="BKMK_examples"></a>Exemplos

As seguintes linhas de um arquivo em lotes de exemplo chamado Minhacop demonstram como usar **shift** com qualquer número de parâmetros do lote. Neste exemplo, Minhacop copia uma lista de arquivos em um diretório específico. Os parâmetros de lote são representados pelos argumentos de nome de arquivo e diretório.
```
@echo off 
rem MYCOPY.BAT copies any number of files
rem to a directory.
rem The command uses the following syntax:
rem mycopy dir file1 file2 ... 
set todir=%1
:getfile
shift
if "%1"=="" goto end
copy %1 %todir%
goto getfile
:end
set todir=
echo All done
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)