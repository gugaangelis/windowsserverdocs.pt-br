---
title: carga reg
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: db661e311e3fe8c393750716de5dab375e7817f4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384702"
---
# <a name="reg-load"></a>carga reg



Grava subchaves e entradas salvas em uma subchave diferente no registro. Destinado a uso com arquivos temporários que são usados para solução de problemas ou edição de entradas do registro.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
reg load KeyName FileName
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<KeyName >|Especifica o caminho completo da subchave a ser carregada. Para especificar computadores remotos, inclua o nome do computador (no formato \\ @ no__t-1ComputerName @ no__t-2 como parte do *KeyName*. Omitir \\ @ no__t-1ComputerName \ faz com que a operação seja padronizada para o computador local. O *KeyName* deve incluir uma chave de raiz válida. As chaves de raiz válidas para o computador local são: HKLM, HKCU, HKCR, HKU e HKCC. Se um computador remoto for especificado, as chaves de raiz válidas serão: HKLM e HKU.|
|\<Nome de arquivo >|Especifica o nome e o caminho do arquivo a ser carregado. Esse arquivo deve ser criado com antecedência usando a operação **reg Save** e uma extensão. HIV.|
|/?|Exibe a ajuda para a **carga reg** no prompt de comando.|

## <a name="remarks"></a>Comentários

A tabela a seguir lista os valores de retorno para a operação **reg Load** .

|Valor|Descrição|
|-----|-----------|
|0|Êxito|
|1|Falha|

## <a name="BKMK_examples"></a>Disso

Para carregar o arquivo chamado TempHive. HIV na chave HKLM\TempHive, digite:
```
REG LOAD HKLM\TempHive TempHive.hiv
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)