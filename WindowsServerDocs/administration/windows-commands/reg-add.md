---
title: Adicionar reg
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d9ad143e-dc10-4e2e-a229-408393c40079
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d46fc2df23391a1dbb782014addc68d9522d603a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441909"
---
# <a name="reg-add"></a>Adicionar reg


Adiciona uma nova subchave ou entrada no registro.

## <a name="syntax"></a>Sintaxe

```
reg add <KeyName> [{/v ValueName | /ve}] [/t DataType] [/s Separator] [/d Data] [/f]
```
Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="parameters"></a>Parâmetros

|      Parâmetro      |                                                                                                                                                                                                                                                                   Descrição                                                                                                                                                                                                                                                                   |
|---------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| \<KeyName<em>></em> | Especifica o caminho completo da subchave ou entrada a ser adicionada. Para especificar um computador remoto, inclua o nome do computador (no formato \\ \\ \<ComputerName >\) como parte do *KeyName*. A omissão \\ \\ComputerName\ faz com que a operação padrão no computador local. O *KeyName* deve incluir uma chave de raiz válido. As chaves de raiz válido para o computador local são: HKLM, HKCU, HKCR, HKU e HKCC. Se um computador remoto for especificado, as chaves válidas são: HKLM e HKU. Se o nome da chave do registro contém um espaço, coloque o nome da chave entre aspas. |
|   /v \<ValueName>   |                                                                                                                                                                                                                                Especifica o nome da entrada do registro a ser adicionada sob a subchave especificada.                                                                                                                                                                                                                                 |
|         /ve         |                                                                                                                                                                                                                                Especifica que a entrada do registro que é adicionada ao registro tem um valor nulo.                                                                                                                                                                                                                                |
|     /t \<Type>      |                                                                                                                                          Especifica o tipo da entrada do registro. *Tipo* deve ser um dos seguintes:</br>REG_SZ</br>REG_MULTI_SZ</br>REG_DWORD_BIG_ENDIAN</br>REG_DWORD</br>REG_BINARY</br>REG_DWORD_LITTLE_ENDIAN</br>REG_LINK</br>REG_FULL_RESOURCE_DESCRIPTOR</br>REG_EXPAND_SZ                                                                                                                                          |
|   /s \<separador >   |                                                                                                                                                              Especifica o caractere a ser usado para separar várias instâncias de dados quando o tipo de dados REG_MULTI_SZ for especificado e mais de uma entrada precisa ser listado. Se não for especificado, o separador padrão é **\0**.                                                                                                                                                              |
|     /d \<Data>      |                                                                                                                                                                                                                                                 Especifica os dados para a nova entrada de registro.                                                                                                                                                                                                                                                  |
|         /f          |                                                                                                                                                                                                                                           Adiciona a entrada do registro sem solicitar confirmação.                                                                                                                                                                                                                                           |
|         /?          |                                                                                                                                                                                                                                              Exibe a Ajuda para **reg adicionar** no prompt de comando.                                                                                                                                                                                                                                               |

## <a name="remarks"></a>Comentários

-   Não não possível adicionar subárvores com esta operação. Esta versão do **reg** não peça confirmação quando você estiver adicionando uma subchave.
-   A tabela a seguir lista os valores retornados para o **reg adicionar** operação.

| Valor | Descrição |
|-------|-------------|
|   0   |   Êxito   |
|   1   |   Falha   |

-   O tipo de chave REG_EXPAND_SZ, use o símbolo de acento circunflexo ( **^** ) com **%** "dentro do parâmetro /d.

## <a name="BKMK_examples"></a>Exemplos

Para adicionar a chave HKLM\Software\MyCo no ABC do computador remoto, digite:
```
REG ADD \\ABC\HKLM\Software\MyCo
```
Para adicionar uma entrada de registro ao HKLM\Software\MyCo com um valor denominado **dados** do tipo REG_BINARY e os dados de **fe340ead**, tipo:
```
REG ADD HKLM\Software\MyCo /v Data /t REG_BINARY /d fe340ead
```
Para adicionar uma entrada de registro com vários valores ao HKLM\Software\MyCo com um nome de valor de **MRU** de tipo REG_MULTI_SZ e os dados de **fax\0mail\0\0**, tipo:
```
REG ADD HKLM\Software\MyCo /v MRU /t REG_MULTI_SZ /d fax\0mail\0\0
```
Para adicionar uma entrada de registro expandida ao HKLM\Software\MyCo com um nome de valor de **caminho** de tipo REG_EXPAND_SZ e os dados de **% systemroot %** , tipo:
```
REG ADD HKLM\Software\MyCo /v Path /t REG_EXPAND_SZ /d ^%systemroot^%
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
