---
title: consulta de reg
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0e6a0d7c-ed9b-4318-833d-33f265a81f39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1e239184cc5d118a858d012528fd8135f0b834e5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827747"
---
# <a name="reg-query"></a>consulta de reg



Retorna uma lista da próxima camada de subchaves e entradas que estão localizadas em uma subchave especificada no registro.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
reg query <KeyName> [{/v <ValueName> | /ve}] [/s] [/se <Separator>] [/f <Data>] [{/k | /d}] [/c] [/e] [/t <Type>] [/z]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<KeyName>|Especifica o caminho completo da subchave. Para especificar computadores remotos, inclua o nome do computador (no formato \\ \\ComputerName\) como parte do *KeyName*. A omissão \\ \\ComputerName\ faz com que a operação padrão no computador local. O *KeyName* deve incluir uma chave de raiz válido. As chaves de raiz válido para o computador local são: HKLM, HKCU, HKCR, HKU e HKCC. Se um computador remoto for especificado, as chaves válidas são: HKLM e HKU.|
|/v \<ValueName>|Especifica o nome do valor do registro a ser consultado. Se omitido, o valor de todos os nomes para *KeyName* são retornados. *ValueName* para esse parâmetro é opcional se o **/f** opção também é usada.|
|/ve|Executa uma consulta para nomes de valor estão vazios.|
|/s|Especifica para consultar todas as subchaves e nomes de valores recursivamente.|
|/Se \<separador >|Especifica o separador de valor único a ser pesquisado no tipo de nome de valor REG_MULTI_SZ. Se *separador* não for especificado, **\0** é usado.|
|/f \<dados >|Especifica os dados ou o padrão a ser pesquisada. Se uma cadeia de caracteres contiver espaços, use aspas duplas. Se não for especificado, um caractere curinga (**&#42;**) é usado como o padrão de pesquisa.|
|/k|Especifica que somente os nomes de chave de pesquisa.|
|/d|Especifica a pesquisa apenas nos dados.|
|/c|Especifica que a consulta é diferencia maiusculas de minúsculas. Por padrão, as consultas não diferenciam maiusculas de minúsculas.|
|/e|Especifica para retornar apenas correspondências exatas. Por padrão, todas as correspondências são retornadas.|
|/t \<Type>|Especifica os tipos de registro para pesquisar. Os tipos válidos são: REG_SZ, REG_MULTI_SZ, REG_EXPAND_SZ, REG_DWORD, REG_BINARY, REG_NONE. Se não for especificado, todos os tipos são pesquisados.|
|/z|Especifica para incluir o equivalente numérico do tipo de registro nos resultados da pesquisa.|
|/?|Exibe a Ajuda para **reg query** no prompt de comando.|

## <a name="remarks-optional-section"></a>Comentários \<seção opcional >

A tabela a seguir lista os valores retornados para o **reg query** operação.

|Valor|Descrição|
|-----|-----------|
|0|Êxito|
|1|Falha|

## <a name="BKMK_examples"></a>Exemplos

Para exibir o valor do nome da versão na chave HKLM\Software\Microsoft\ResKit, digite:
```
REG QUERY HKLM\Software\Microsoft\ResKit /v Version
```
Para exibir todas as subchaves e valores na chave HKLM\Software\Microsoft\ResKit\Nt\Setup em um computador remoto denominado ABC, digite:
```
REG QUERY \\ABC\HKLM\Software\Microsoft\ResKit\Nt\Setup /s
```
Para exibir todas as subchaves e valores de tipo REG_MULTI_SZ usando **#** como o separador, digite:
```
REG QUERY HKLM\Software\Microsoft\ResKit\Nt\Setup /se #
```
Para exibir a chave, valor e os dados exatos e diferencia maiusculas de minúsculas corresponde do sistema sob a raiz HKLM do tipo de dados REG_SZ, digite:
```
REG QUERY HKLM /f SYSTEM /t REG_SZ /c /e
```
Para exibir a chave, valor e os dados que correspondem ao **0F** nos dados na chave de raiz de HKCU dos dados de tipo REG_BINARY.
```
REG QUERY HKCU /f 0F /d /t REG_BINARY
```
Para exibir o valor e os dados para nomes de valores nulos (padrão) em HKLM\SOFTWARE, digite:
```
REG QUERY HKLM\SOFTWARE /ve
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)