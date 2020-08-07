---
title: shift
description: Artigo de referência para Shift, que altera a posição de parâmetros de lote em um arquivo em lotes.
ms.topic: article
ms.assetid: b56574e8-570a-4cc9-bbac-1b94fbf6a47a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 985c1271a1da8f486e2313ba9aeb266803664b93
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87882389"
---
# <a name="shift"></a>shift

Altera a posição de parâmetros de lote em um arquivo em lotes.



## <a name="syntax"></a>Sintaxe

```
shift [/n <N>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|opção\<N>|Especifica o início da mudança no argumento *N*-ésimo, em que *N* é qualquer valor de 0 a 8. Requer extensões de comando, que são habilitadas por padrão.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

- O comando **Shift** altera os valores dos parâmetros de lote **%0** a **%9** copiando cada parâmetro para o anterior — o valor de **%1** é copiado para **%0**, o valor de **%2** é copiado para **%1**e assim por diante. Isso é útil para gravar um arquivo em lotes que executa a mesma operação em qualquer número de parâmetros.
- Se as extensões de comando estiverem habilitadas, o comando **Shift** dará suporte à opção de linha de comando **/n** . A opção **/n** especifica o início da mudança no argumento enésimo, em que **n** é qualquer valor de 0 a 8. Por exemplo, **Shift/2** mudaria **%3** para **%2**, **%4** para **%3**e assim por diante e deixará **%0** e **%1** não afetado. As extensões de comando são habilitadas por padrão.
- Você pode usar o comando **Shift** para criar um arquivo em lotes que pode aceitar mais de 10 parâmetros de lote. Se você especificar mais de 10 parâmetros na linha de comando, aqueles que aparecerem após o décimo (**%9**) serão deslocados um de cada vez em **%9**.
- O comando **Shift** não tem nenhum efeito sobre o **%\*** parâmetro Batch.
- Não há nenhum comando de **deslocamento** para trás. Depois de implementar o comando **Shift** , você não pode recuperar o parâmetro de lote (**%0**) que existia antes da mudança.

## <a name="examples"></a>Exemplos

As linhas a seguir de um arquivo em lotes de exemplo chamado Mycopy.bat demonstram como usar **Shift** com qualquer número de parâmetros de lote. Neste exemplo, Mycopy.bat copia uma lista de arquivos para um diretório específico. Os parâmetros de lote são representados pelos argumentos de nome de arquivo e diretório.
```
@echo off
rem MYCOPY.BAT copies any number of files
rem to a directory.
rem The command uses the following syntax:
rem mycopy dir file1 file2 ...
set todir=%1
:getfile
shift
if %1== goto end
copy %1 %todir%
goto getfile
:end
set todir=
echo All done
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
