---
title: comparação de reg
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 177dc6a3-034e-4846-a394-330d03c14e0b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9f4a8c51e7add429aab326804af698fed7007999
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887217"
---
# <a name="reg-compare"></a>comparação de reg



Compara especificado subchaves do registro ou entradas.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
reg compare <KeyName1> <KeyName2> [{/v ValueName | /ve}] [{/oa | /od | /os | on}] [/s]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<KeyName1>|Especifica o caminho completo da primeira subchave a ser comparado. Para especificar um computador remoto, inclua o nome do computador (no formato \\ \\ComputerName\) como parte do *KeyName*. A omissão \\ \\ComputerName\ faz com que a operação padrão no computador local. O *KeyName* deve incluir uma chave de raiz válido. As chaves de raiz válido para o computador local são: HKLM, HKCU, HKCR, HKU e HKCC. Se um computador remoto for especificado, as chaves válidas são: HKLM e HKU.|
|\<KeyName2>|Especifica o caminho completo da segunda subchave a ser comparado. Para especificar um computador remoto, inclua o nome do computador (no formato \\ \\ComputerName\) como parte do *KeyName*. A omissão \\ \\ComputerName\ faz com que a operação padrão no computador local. Especificar somente o nome do computador no *KeyName2* faz com que a operação usar o caminho até a subchave especificada na *Nomedachave1*. O *KeyName* deve incluir uma chave de raiz válido. As chaves de raiz válido para o computador local são: HKLM, HKCU, HKCR, HKU e HKCC. Se um computador remoto for especificado, as chaves válidas são: HKLM e HKU.|
|/v \<ValueName>|Especifica o nome do valor a ser comparado à subchave.|
|/ve|Especifica que apenas as entradas que têm um nome de valor de null devem ser comparadas.|
|[{/oa | /od | /os | on}]|Especifica como exibir os resultados da operação de comparação. O padrão é **/od**. Consulte as opções específicas a seguir.|
|/oa|Especifica que todas as diferenças e as correspondências são exibidas. Por padrão, apenas as diferenças são listadas.|
|/od|Especifica que apenas as diferenças são exibidas. Esse é o comportamento padrão.|
|/os|Especifica que apenas as correspondências são exibidas. Por padrão, apenas as diferenças são listadas.|
|/on|Especifica que nada é exibido. Por padrão, apenas as diferenças são listadas.|
|/s|Compara todas as subchaves e entradas recursivamente.|
|/?|Exibe a Ajuda para **reg compare** no prompt de comando.|

## <a name="remarks"></a>Comentários

A tabela a seguir lista os valores de retornados para **reg compare**.

|Valor|Descrição|
|-----|-----------|
|0|A comparação for bem-sucedida e o resultado é idêntico.|
|1|Falha na comparação.|
|2|A comparação foi bem-sucedida e as diferenças foram encontradas.|

A tabela a seguir lista os símbolos exibidos nos resultados.

|Símbolo|Descrição|
|------|-----------|
|=|*Nomedachave1* dados são iguais a *KeyName2* dados.|
|<|*Nomedachave1* dados são menor que *KeyName2* dados.|
|>|*Nomedachave1* dados são maiores que *KeyName2* dados.|

## <a name="BKMK_examples"></a>Exemplos

Para comparar todos os valores na chave **MyApp** com todos os valores sob a chave **SaveMyApp**, tipo:

REG COMPARE HKLM\Software\MyCo\MyApp HKLM\Software\MyCo\SaveMyApp

Para comparar o valor para a versão na chave **MyCo** e o valor para a versão sob a chave **MyCo1**, tipo:

REG COMPARE HKLM\Software\MyCo HKLM\Software\MyCo1 /v versão

Para comparar todas as subchaves e valores em HKLM\Software\MyCo no computador chamado ZODÍACO com todas as subchaves e valores em HKLM\Software\MyCo no computador local, digite:

COMPARAR REG \\ \\ZODIAC\HKLM\Software\MyCo \\ \\. /s

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)