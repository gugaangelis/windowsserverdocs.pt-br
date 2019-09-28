---
title: consulta reg
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: d2f616fb33974df4327c7b2536b3143b75d116be
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371722"
---
# <a name="reg-query"></a>consulta reg



Retorna uma lista da próxima camada de subchaves e entradas localizadas em uma subchave especificada no registro.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
reg query <KeyName> [{/v <ValueName> | /ve}] [/s] [/se <Separator>] [/f <Data>] [{/k | /d}] [/c] [/e] [/t <Type>] [/z]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<KeyName >|Especifica o caminho completo da subchave. Para especificar computadores remotos, inclua o nome do computador (no formato \\ @ no__t-1ComputerName @ no__t-2 como parte do *KeyName*. Omitir \\ @ no__t-1ComputerName \ faz com que a operação seja padronizada para o computador local. O *KeyName* deve incluir uma chave de raiz válida. As chaves de raiz válidas para o computador local são: HKLM, HKCU, HKCR, HKU e HKCC. Se um computador remoto for especificado, as chaves de raiz válidas serão: HKLM e HKU.|
|/v \<ValueName >|Especifica o nome do valor do registro que deve ser consultado. Se omitido, todos os nomes de valor para *KeyName* serão retornados. *ValueName* para esse parâmetro será opcional se a opção **/f** também for usada.|
|/ve|Executa uma consulta para nomes de valores vazios.|
|/s|Especifica para consultar todas as subchaves e nomes de valor recursivamente.|
|/se \<Separator >|Especifica o separador de valor único a ser pesquisado no nome do valor tipo REG_MULTI_SZ. Se o *separador* não for especificado, será usado **\ 0** .|
|/f \<Data >|Especifica os dados ou o padrão a ser pesquisado. Use aspas duplas se uma cadeia de caracteres contiver espaços. Se não for especificado, um curinga **&#42;** () será usado como o padrão de pesquisa.|
|/k|Especifica a pesquisa somente em nomes de chave.|
|/d|Especifica a pesquisa somente em dados.|
|/c|Especifica que a consulta diferencia maiúsculas de minúsculas. Por padrão, as consultas não diferenciam maiúsculas de minúsculas.|
|/e|Especifica para retornar apenas correspondências exatas. Por padrão, todas as correspondências são retornadas.|
|/t \<Type >|Especifica os tipos de registro a serem pesquisados. Os tipos válidos são: REG_SZ, REG_MULTI_SZ, REG_EXPAND_SZ, REG_DWORD, REG_BINARY, REG_NONE. Se não for especificado, todos os tipos serão pesquisados.|
|/z|Especifica para incluir o equivalente numérico para o tipo de registro nos resultados da pesquisa.|
|/?|Exibe a ajuda para a **consulta reg** no prompt de comando.|

## <a name="remarks-optional-section"></a>Comentários da seção \<Optional >

A tabela a seguir lista os valores de retorno para a operação **reg query** .

|Valor|Descrição|
|-----|-----------|
|0|Êxito|
|1|Falha|

## <a name="BKMK_examples"></a>Disso

Para exibir o valor da versão de valor de nome na chave HKLM\Software\Microsoft\ResKit, digite:
```
REG QUERY HKLM\Software\Microsoft\ResKit /v Version
```
Para exibir todas as subchaves e valores sob a chave HKLM\Software\Microsoft\ResKit\Nt\Setup em um computador remoto chamado ABC, digite:
```
REG QUERY \\ABC\HKLM\Software\Microsoft\ResKit\Nt\Setup /s
```
Para exibir todas as subchaves e valores do tipo REG_MULTI_SZ usando **#** como separador, digite:
```
REG QUERY HKLM\Software\Microsoft\ResKit\Nt\Setup /se #
```
Para exibir a chave, o valor e os dados para correspondências exatas e diferenciadas entre maiúsculas e minúsculas do sistema na raiz HKLM do tipo de dados REG_SZ, digite:
```
REG QUERY HKLM /f SYSTEM /t REG_SZ /c /e
```
Para exibir a chave, o valor e os dados que correspondem a **0f** nos dados sob a chave raiz HKCU do tipo de dados REG_BINARY.
```
REG QUERY HKCU /f 0F /d /t REG_BINARY
```
Para exibir o valor e os dados para nomes de valores nulos (padrão) em HKLM\SOFTWARE, digite:
```
REG QUERY HKLM\SOFTWARE /ve
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)