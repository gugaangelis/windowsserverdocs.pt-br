---
title: exportar reg
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0ad9526f-1e29-4fa5-9d2d-feaa92f12d7c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7fb3a779ffe5a4e7d513ca9a3afed8ee90901688
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384753"
---
# <a name="reg-export"></a>exportar reg



Copia as subchaves, entradas e valores especificados do computador local em um arquivo para transferência para outros servidores.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
Reg export KeyName FileName [/y]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<KeyName >|Especifica o caminho completo da subchave. A operação de exportação funciona apenas com o computador local. O KeyName deve incluir uma chave de raiz válida. As chaves raiz válidas são: HKLM, HKCU, HKCR, HKU e HKCC.|
|\<Nome de arquivo >|Especifica o nome e o caminho do arquivo a ser criado durante a operação. O arquivo deve ter uma extensão. reg.|
|/y|Substitui qualquer arquivo existente pelo *nome nome do arquivo sem solicitar* confirmação.|
|/?|Exibe a ajuda para o **reg export** no prompt de comando.|

## <a name="remarks"></a>Comentários

A tabela a seguir lista os valores de retorno para a operação de **exportação de reg** .

|Valor|Descrição|
|-----|-----------|
|0|Êxito|
|1|Falha|

## <a name="BKMK_examples"></a>Disso

Para exportar o conteúdo de todas as subchaves e valores da chave MyApp para o arquivo AppBkUp. reg, digite:
```
reg export HKLM\Software\MyCo\MyApp AppBkUp.reg
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)