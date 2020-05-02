---
title: consulta reg
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0e6a0d7c-ed9b-4318-833d-33f265a81f39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ea66a7d96435309a3b30b67f45bc68200f30dd2f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722526"
---
# <a name="reg-query"></a>consulta reg



Retorna uma lista da próxima camada de subchaves e entradas localizadas em uma subchave especificada no registro.



## <a name="syntax"></a>Sintaxe

```
reg query <KeyName> [{/v <ValueName> | /ve}] [/s] [/se <Separator>] [/f <Data>] [{/k | /d}] [/c] [/e] [/t <Type>] [/z]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<KeyName>|Especifica o caminho completo da subchave. Para especificar computadores remotos, inclua o nome do computador (no \\ \\formato ComputerName\) como parte do *KeyName*. \\ \\Omitir computername \ faz com que a operação seja padronizada para o computador local. O *KeyName* deve incluir uma chave de raiz válida. As chaves de raiz válidas para o computador local são: HKLM, HKCU, HKCR, HKU e HKCC. Se um computador remoto for especificado, as chaves de raiz válidas serão: HKLM e HKU.|
|/v \<valor>|Especifica o nome do valor do registro que deve ser consultado. Se omitido, todos os nomes de valor para *KeyName* serão retornados. *ValueName* para esse parâmetro será opcional se a opção **/f** também for usada.|
|/ve|Executa uma consulta para nomes de valores vazios.|
|/s|Especifica para consultar todas as subchaves e nomes de valor recursivamente.|
|> \<separador de/se|Especifica o separador de valor único a ser procurado no tipo de nome de valor REG_MULTI_SZ. Se o *separador* não for especificado, será usado **\ 0** .|
|/f \<> de dados|Especifica os dados ou o padrão a ser pesquisado. Use aspas duplas se uma cadeia de caracteres contiver espaços. Se não for especificado, um curinga (**&#42;**) será usado como o padrão de pesquisa.|
|/k|Especifica a pesquisa somente em nomes de chave.|
|/d|Especifica a pesquisa somente em dados.|
|/c|Especifica que a consulta diferencia maiúsculas de minúsculas. Por padrão, as consultas não diferenciam maiúsculas de minúsculas.|
|/e|Especifica para retornar apenas correspondências exatas. Por padrão, todas as correspondências são retornadas.|
|/t \<tipo>|Especifica os tipos de registro a serem pesquisados. Os tipos válidos são: REG_SZ, REG_MULTI_SZ, REG_EXPAND_SZ, REG_DWORD, REG_BINARY, REG_NONE. Se não for especificado, todos os tipos serão pesquisados.|
|/z|Especifica para incluir o equivalente numérico para o tipo de registro nos resultados da pesquisa.|
|/?|Exibe a ajuda para a **consulta reg** no prompt de comando.|

## <a name="remarks-optional-section"></a>Comentários \<da seção opcional>

A tabela a seguir lista os valores de retorno para a operação **reg query** .

|Valor|Descrição|
|-----|-----------|
|0|Sucesso|
|1|Falha|

## <a name="examples"></a>Exemplos

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

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)