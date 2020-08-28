---
title: reg query
description: Artigo de referência do comando reg query, que retorna uma lista da próxima camada de subchaves e entradas localizadas em uma subchave especificada no registro.
ms.topic: reference
ms.assetid: 0e6a0d7c-ed9b-4318-833d-33f265a81f39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 633928d89059e1b9a9677141011b391ee0d34757
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025080"
---
# <a name="reg-query"></a>reg query

Retorna uma lista da próxima camada de subchaves e entradas localizadas em uma subchave especificada no registro.

## <a name="syntax"></a>Sintaxe

```
reg query <keyname> [{/v <Valuename> | /ve}] [/s] [/se <separator>] [/f <data>] [{/k | /d}] [/c] [/e] [/t <Type>] [/z]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<keyname>` | Especifica o caminho completo da subchave. Para especificar um computador remoto, inclua o nome do computador (no formato `\\<computername>\` ) como parte do *KeyName*. Omitir `\\<computername>\` faz com que a operação seja padronizada para o computador local. O *KeyName* deve incluir uma chave de raiz válida. As chaves de raiz válidas para o computador local são: **HKLM**, **HKCU**, **HKCR**, **HKU**e **HKCC**. Se um computador remoto for especificado, as chaves de raiz válidas serão: **HKLM** e **HKU**. Se o nome da chave do registro contiver um espaço, coloque o nome da chave entre aspas. |
| /v `<Valuename>` | Especifica o nome do valor do registro que deve ser consultado. Se omitido, todos os nomes de valor para *KeyName* serão retornados. *ValueName* para esse parâmetro será opcional se a opção **/f** também for usada. |
| /ve | Executa uma consulta para nomes de valores vazios. |
| /s | Especifica para consultar todas as subchaves e nomes de valor recursivamente. |
| /se `<separator>` | Especifica o separador de valor único a ser procurado no tipo de nome de valor **REG_MULTI_SZ**. Se o *separador* não for especificado, será usado **\ 0** . |
| /f `<data>` | Especifica os dados ou o padrão a ser pesquisado. Use aspas duplas se uma cadeia de caracteres contiver espaços. Se não for especificado, um curinga (**&#42;**) será usado como o padrão de pesquisa. |
| /k | Especifica a pesquisa somente em nomes de chave. |
| /d | Especifica a pesquisa somente em dados. |
| /c | Especifica que a consulta diferencia maiúsculas de minúsculas. Por padrão, as consultas não diferenciam maiúsculas de minúsculas. |
| /e | Especifica para retornar apenas correspondências exatas. Por padrão, todas as correspondências são retornadas. |
| /t `<Type>` | Especifica os tipos de registro a serem pesquisados. Os tipos válidos são: **REG_SZ**, **REG_MULTI_SZ**, **REG_EXPAND_SZ**, **REG_DWORD**, **REG_BINARY**, **REG_NONE**. Se não for especificado, todos os tipos serão pesquisados. |
| /z | Especifica para incluir o equivalente numérico para o tipo de registro nos resultados da pesquisa. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Os valores de retorno para a operação de **consulta reg** são:

    | Valor | Descrição |
    |--|--|
    | 0 | Êxito |
    | 1 | Falha |

### <a name="examples"></a>Exemplos

Para exibir o valor da versão de valor de nome na chave HKLM\Software\Microsoft\ResKit, digite:

```
reg query HKLM\Software\Microsoft\ResKit /v Version
```

Para exibir todas as subchaves e valores sob a chave HKLM\Software\Microsoft\ResKit\Nt\Setup em um computador remoto chamado ABC, digite:

```
reg query \\ABC\HKLM\Software\Microsoft\ResKit\Nt\Setup /s
```

Para exibir todas as subchaves e valores do tipo REG_MULTI_SZ usando **#** como separador, digite:

```
reg query HKLM\Software\Microsoft\ResKit\Nt\Setup /se #
```

Para exibir a chave, o valor e os dados para correspondências exatas e diferenciadas entre maiúsculas e minúsculas do sistema na raiz HKLM do tipo de dados REG_SZ, digite:

```
reg query HKLM /f SYSTEM /t REG_SZ /c /e
```

Para exibir a chave, o valor e os dados que correspondem a **0f** nos dados sob a chave raiz HKCU do tipo de dados REG_BINARY, digite:

```
reg query HKCU /f 0F /d /t REG_BINARY
```

Para exibir o valor e os dados para nomes de valores nulos (padrão) em HKLM\SOFTWARE, digite:

```
reg query HKLM\SOFTWARE /ve
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
