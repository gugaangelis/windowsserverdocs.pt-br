---
title: Carga de reg
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3b0b2b1b-f510-4108-9e9d-7057e924aa6e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ebc75ad78b7334f4d48a085f6870a443b31fa2a9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852187"
---
# <a name="reg-load"></a>Carga de reg



Gravações salva subchaves e entradas em uma subchave diferente no registro. Deve ser usada com arquivos temporários que são usados para solução de problemas ou editar entradas do registro.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
reg load KeyName FileName
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<KeyName>|Especifica o caminho completo da subchave a ser carregado. Para especificar computadores remotos, inclua o nome do computador (no formato \\ \\ComputerName\) como parte do *KeyName*. A omissão \\ \\ComputerName\ faz com que a operação padrão no computador local. O *KeyName* deve incluir uma chave de raiz válido. As chaves de raiz válido para o computador local são: HKLM, HKCU, HKCR, HKU e HKCC. Se um computador remoto for especificado, as chaves válidas são: HKLM e HKU.|
|\<FileName>|Especifica o nome e caminho do arquivo a ser carregado. Esse arquivo deve ser criado com antecedência, usando o **reg salvar** operação e uma extensão de HIV.|
|/?|Exibe a Ajuda para **reg carga** no prompt de comando.|

## <a name="remarks"></a>Comentários

A tabela a seguir lista os valores retornados para o **reg carga** operação.

|Valor|Descrição|
|-----|-----------|
|0|Êxito|
|1|Falha|

## <a name="BKMK_examples"></a>Exemplos

Para carregar o arquivo chamado TempHive à chave HKLM\TempHive, digite:
```
REG LOAD HKLM\TempHive TempHive.hiv
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)