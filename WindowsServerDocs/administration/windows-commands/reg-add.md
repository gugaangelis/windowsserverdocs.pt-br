---
title: Adicionar Reg
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d9ad143e-dc10-4e2e-a229-408393c40079
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: df59477c980169699dac897e36836e5226b6a0fa
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836589"
---
# <a name="reg-add"></a>Adicionar Reg


Adiciona uma nova subchave ou entrada ao registro.

## <a name="syntax"></a>Sintaxe

```
reg add <KeyName> [{/v ValueName | /ve}] [/t DataType] [/s Separator] [/d Data] [/f]
```
Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

### <a name="parameters"></a>Parâmetros

|      Parâmetro      |                                                                                                                                                                                                                                                                   Descrição                                                                                                                                                                                                                                                                   |
|---------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| \<KeyName<em>></em> | Especifica o caminho completo da subchave ou entrada a ser adicionada. Para especificar um computador remoto, inclua o nome do computador (no formato \\\\\<ComputerName >\) como parte do *KeyName*. Omitir \\\\computername \ faz com que a operação seja padronizada para o computador local. O *KeyName* deve incluir uma chave de raiz válida. As chaves de raiz válidas para o computador local são: HKLM, HKCU, HKCR, HKU e HKCC. Se um computador remoto for especificado, as chaves de raiz válidas serão: HKLM e HKU. Se o nome da chave do registro contiver um espaço, coloque o nome da chave entre aspas. |
|   /v \<valueName >   |                                                                                                                                                                                                                                Especifica o nome da entrada do registro a ser adicionada na subchave especificada.                                                                                                                                                                                                                                 |
|         /ve         |                                                                                                                                                                                                                                Especifica que a entrada do registro que é adicionada ao registro tem um valor nulo.                                                                                                                                                                                                                                |
|     /t \<tipo >      |                                                                                                                                          Especifica o tipo para a entrada do registro. O *tipo* deve ser um dos seguintes:</br>REG_SZ</br>REG_MULTI_SZ</br>REG_DWORD_BIG_ENDIAN</br>REG_DWORD</br>REG_BINARY</br>REG_DWORD_LITTLE_ENDIAN</br>REG_LINK</br>REG_FULL_RESOURCE_DESCRIPTOR</br>REG_EXPAND_SZ                                                                                                                                          |
|   /s separador de \<>   |                                                                                                                                                              Especifica o caractere a ser usado para separar várias instâncias de dados quando o tipo de dados REG_MULTI_SZ é especificado e mais de uma entrada precisa ser listada. Se não for especificado, o separador padrão será **\ 0**.                                                                                                                                                              |
|     /d \<> de dados      |                                                                                                                                                                                                                                                 Especifica os dados para a nova entrada do registro.                                                                                                                                                                                                                                                  |
|         /f          |                                                                                                                                                                                                                                           Adiciona a entrada do registro sem solicitar confirmação.                                                                                                                                                                                                                                           |
|         /?          |                                                                                                                                                                                                                                              Exibe a ajuda para o **reg Add** no prompt de comando.                                                                                                                                                                                                                                               |

## <a name="remarks"></a>Comentários

-   Não é possível adicionar subárvores com esta operação. Esta versão do **reg** não solicita confirmação ao adicionar uma subchave.
-   A tabela a seguir lista os valores de retorno para a operação **reg Add** .

| {1&gt;Valor&lt;1} | Descrição |
|-------|-------------|
|   0   |   Êxito   |
|   1   |   Falha   |

-   Para o tipo de chave REG_EXPAND_SZ, use o símbolo de cursor ( **^** ) com **%** dentro do parâmetro/d

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para adicionar a chave HKLM\Software\MyCo no computador remoto ABC, digite:
```
REG ADD \\ABC\HKLM\Software\MyCo
```
Para adicionar uma entrada de registro ao HKLM\Software\MyCo com um valor denominado **dados** do tipo REG_BINARY e dados de **fe340ead**, digite:
```
REG ADD HKLM\Software\MyCo /v Data /t REG_BINARY /d fe340ead
```
Para adicionar uma entrada de registro de valores multivalors ao HKLM\Software\MyCo com um nome de valor de **MRU** do tipo REG_MULTI_SZ e dados de **fax\0mail\0\0**, digite:
```
REG ADD HKLM\Software\MyCo /v MRU /t REG_MULTI_SZ /d fax\0mail\0\0
```
Para adicionar uma entrada de registro expandida ao HKLM\Software\MyCo com um nome de valor de **caminho** do tipo REG_EXPAND_SZ e dados de **% systemroot%** , digite:
```
REG ADD HKLM\Software\MyCo /v Path /t REG_EXPAND_SZ /d ^%systemroot^%
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
