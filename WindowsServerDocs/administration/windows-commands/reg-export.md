---
title: reg export
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d7aeddb4b069b1baf5b8f7aaea2730a2b25bdad7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889647"
---
# <a name="reg-export"></a>reg export



Copia as subchaves especificadas, entradas e valores do computador local em um arquivo para transferência para outros servidores.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
Reg export KeyName FileName [/y]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<KeyName>|Especifica o caminho completo da subchave. A operação de exportação funciona somente com o computador local. O KeyName deve incluir uma chave de raiz válido. As chaves válidas são: HKLM, HKCU, HKCR, HKU e HKCC.|
|\<FileName>|Especifica o nome e caminho do arquivo a ser criado durante a operação. O arquivo deve ter uma extensão. reg.|
|/y|Substitui arquivos existentes com o nome *FileName* sem solicitar confirmação.|
|/?|Exibe a Ajuda para **reg export** no prompt de comando.|

## <a name="remarks"></a>Comentários

A tabela a seguir lista os valores retornados para o **reg export** operação.

|Valor|Descrição|
|-----|-----------|
|0|Êxito|
|1|Falha|

## <a name="BKMK_examples"></a>Exemplos

Para exportar o conteúdo de todas as subchaves e valores da chave MyApp para o arquivo AppBkUp. reg, digite:
```
reg export HKLM\Software\MyCo\MyApp AppBkUp.reg
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)