---
title: Adicionar Reg
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d9ad143e-dc10-4e2e-a229-408393c40079
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 85880a1a0fd92dca1f203d3b29df5300fab4eb00
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722598"
---
# <a name="reg-add"></a>Adicionar Reg


Adiciona uma nova subchave ou entrada ao registro.

## <a name="syntax"></a>Sintaxe

```
reg add <KeyName> [{/v ValueName | /ve}] [/t DataType] [/s Separator] [/d Data] [/f]
```

### <a name="parameters"></a>Parâmetros

|      Parâmetro      |                                                                                                                                                                                                                                                                   Descrição                                                                                                                                                                                                                                                                   |
|---------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| \<KeyName<em>></em> | Especifica o caminho completo da subchave ou entrada a ser adicionada. Para especificar um computador remoto, inclua o nome do computador (no formato \\ \\ \<ComputerName>\) como parte do *KeyName*. \\ \\Omitir computername \ faz com que a operação seja padronizada para o computador local. O *KeyName* deve incluir uma chave de raiz válida. As chaves de raiz válidas para o computador local são: HKLM, HKCU, HKCR, HKU e HKCC. Se um computador remoto for especificado, as chaves de raiz válidas serão: HKLM e HKU. Se o nome da chave do registro contiver um espaço, coloque o nome da chave entre aspas. |
|   /v \<valor>   |                                                                                                                                                                                                                                Especifica o nome da entrada do registro a ser adicionada na subchave especificada.                                                                                                                                                                                                                                 |
|         /ve         |                                                                                                                                                                                                                                Especifica que a entrada do registro que é adicionada ao registro tem um valor nulo.                                                                                                                                                                                                                                |
|     /t \<tipo>      |                                                                                                                                          Especifica o tipo para a entrada do registro. O *tipo* deve ser um dos seguintes:</br>REG_SZ</br>REG_MULTI_SZ</br>REG_DWORD_BIG_ENDIAN</br>REG_DWORD</br>REG_BINARY</br>REG_DWORD_LITTLE_ENDIAN</br>REG_LINK</br>REG_FULL_RESOURCE_DESCRIPTOR</br>REG_EXPAND_SZ                                                                                                                                          |
|   /s \<separador>   |                                                                                                                                                              Especifica o caractere a ser usado para separar várias instâncias de dados quando o tipo de dados REG_MULTI_SZ é especificado e mais de uma entrada precisa ser listada. Se não for especificado, o separador padrão será **\ 0**.                                                                                                                                                              |
|     /d \<dados>      |                                                                                                                                                                                                                                                 Especifica os dados para a nova entrada do registro.                                                                                                                                                                                                                                                  |
|         /f          |                                                                                                                                                                                                                                           Adiciona a entrada do registro sem solicitar confirmação.                                                                                                                                                                                                                                           |
|         /?          |                                                                                                                                                                                                                                              Exibe a ajuda para o **reg Add** no prompt de comando.                                                                                                                                                                                                                                               |

## <a name="remarks"></a>Comentários

-   Não é possível adicionar subárvores com esta operação. Esta versão do **reg** não solicita confirmação ao adicionar uma subchave.
-   A tabela a seguir lista os valores de retorno para a operação **reg Add** .

| Valor | Descrição |
|-------|-------------|
|   0   |   Sucesso   |
|   1   |   Falha   |

-   Para o tipo de chave REG_EXPAND_SZ, use o símbolo de **^** cursor ( **%** ) com dentro do parâmetro/d

## <a name="examples"></a>Exemplos

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
Para adicionar uma entrada de registro expandida ao HKLM\Software\MyCo com um nome de valor de **caminho** do tipo REG_EXPAND_SZ e dados de **% systemroot%**, digite:
```
REG ADD HKLM\Software\MyCo /v Path /t REG_EXPAND_SZ /d ^%systemroot^%
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
