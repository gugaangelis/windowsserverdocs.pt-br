---
title: carga reg
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3b0b2b1b-f510-4108-9e9d-7057e924aa6e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 140c6b51b9f88081a8686ebebbc9400f241b5ef6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836379"
---
# <a name="reg-load"></a>carga reg



Grava subchaves e entradas salvas em uma subchave diferente no registro. Destinado a uso com arquivos temporários que são usados para solução de problemas ou edição de entradas do registro.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
reg load KeyName FileName
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<KeyName >|Especifica o caminho completo da subchave a ser carregada. Para especificar computadores remotos, inclua o nome do computador (no formato \\\\ComputerName\) como parte do *KeyName*. Omitir \\\\computername \ faz com que a operação seja padronizada para o computador local. O *KeyName* deve incluir uma chave de raiz válida. As chaves de raiz válidas para o computador local são: HKLM, HKCU, HKCR, HKU e HKCC. Se um computador remoto for especificado, as chaves de raiz válidas serão: HKLM e HKU.|
|\<nome de arquivo >|Especifica o nome e o caminho do arquivo a ser carregado. Esse arquivo deve ser criado com antecedência usando a operação **reg Save** e uma extensão. HIV.|
|/?|Exibe a ajuda para a **carga reg** no prompt de comando.|

## <a name="remarks"></a>Comentários

A tabela a seguir lista os valores de retorno para a operação **reg Load** .

|{1&gt;Valor&lt;1}|Descrição|
|-----|-----------|
|0|Êxito|
|1|Falha|

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para carregar o arquivo chamado TempHive. HIV na chave HKLM\TempHive, digite:
```
REG LOAD HKLM\TempHive TempHive.hiv
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)