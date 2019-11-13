---
title: shift
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: f74e0f1f9041a4a7b95d83772ea79376c82876de
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371254"
---
# <a name="shift"></a>shift



Altera a posição de parâmetros de lote em um arquivo em lotes.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
shift [/n <N>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/n \<N >|Especifica o início da mudança no argumento *N*-ésimo, em que *N* é qualquer valor de 0 a 8. Requer extensões de comando, que são habilitadas por padrão.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

- O comando **Shift** altera os valores dos parâmetros de lote **%0** a **%9** copiando cada parâmetro para o anterior — o valor de **%1** é copiado para **%0**, o valor de **%2** é copiado para **%1**e assim por diante. Isso é útil para gravar um arquivo em lotes que executa a mesma operação em qualquer número de parâmetros.
- Se as extensões de comando estiverem habilitadas, o comando **Shift** dará suporte à opção de linha de comando **/n** . A opção **/n** especifica o início da mudança no argumento enésimo, em que **n** é qualquer valor de 0 a 8. Por exemplo, **Shift/2** mudaria **%3** para **%2**, **%4** para **%3**e assim por diante e deixará **%0** e **%1** não afetado. As extensões de comando são habilitadas por padrão.
- Você pode usar o comando **Shift** para criar um arquivo em lotes que pode aceitar mais de 10 parâmetros de lote. Se você especificar mais de 10 parâmetros na linha de comando, aqueles que aparecerem após o décimo ( **%9**) serão deslocados um de cada vez em **%9**.
- O comando **Shift** não tem nenhum efeito sobre o **%\\** * parâmetro de lote.
- Não há nenhum comando de **deslocamento** para trás. Depois de implementar o comando **Shift** , você não pode recuperar o parâmetro de lote ( **%0**) que existia antes da mudança.

## <a name="BKMK_examples"></a>Disso

As linhas a seguir de um arquivo em lotes de exemplo chamado mycopy. bat demonstram como usar **Shift** com qualquer número de parâmetros de lote. Neste exemplo, mycopy. bat copia uma lista de arquivos para um diretório específico. Os parâmetros de lote são representados pelos argumentos de nome de arquivo e diretório.
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