---
title: importação de reg
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0be103de-08fc-4f02-b590-361782680b3e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7d1dd1b61848671b528c62fd22fe656e14fda7b1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861837"
---
# <a name="reg-import"></a>importação de reg



Copia o conteúdo de um arquivo que contém exportados subchaves do registro, entradas e valores no registro do computador local.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
Reg import FileName
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<FileName>|Especifica o nome e caminho do arquivo que tem o conteúdo a ser copiado para o registro do computador local. Esse arquivo deve ser criado com antecedência, usando **reg export**.|
|/?|Exibe a Ajuda para **importação reg** no prompt de comando.|

## <a name="remarks"></a>Comentários

A tabela a seguir lista os valores retornados para o **importação reg** operação.

|Valor|Descrição|
|-----|-----------|
|0|Êxito|
|1|Falha|

## <a name="BKMK_examples"></a>Exemplos

Para importar as entradas do registro do arquivo denominado AppBkUp. reg, digite:
```
reg import AppBkUp.reg
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)