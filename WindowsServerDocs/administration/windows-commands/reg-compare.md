---
title: reg compare
description: Artigo de referência do comando reg Compare, que compara as entradas ou as subchaves do registro especificadas.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 177dc6a3-034e-4846-a394-330d03c14e0b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3b508a52d0f110455b09002a1044fefcf1be048a
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85930728"
---
# <a name="reg-compare"></a>reg compare

Compara as entradas ou subchaves do registro especificadas.

## <a name="syntax"></a>Sintaxe

```
reg compare <keyname1> <keyname2> [{/v Valuename | /ve}] [{/oa | /od | /os | on}] [/s]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<keyname1>` | Especifica o caminho completo da subchave ou entrada a ser adicionada. Para especificar um computador remoto, inclua o nome do computador (no formato `\\<computername>\` ) como parte do *KeyName*. Omitir `\\<computername>\` faz com que a operação seja padronizada para o computador local. O *KeyName* deve incluir uma chave de raiz válida. As chaves de raiz válidas para o computador local são: **HKLM**, **HKCU**, **HKCR**, **HKU**e **HKCC**. Se um computador remoto for especificado, as chaves de raiz válidas serão: **HKLM** e **HKU**. Se o nome da chave do registro contiver um espaço, coloque o nome da chave entre aspas. |
| `<keyname2>` | Especifica o caminho completo da segunda subchave a ser comparada. Para especificar um computador remoto, inclua o nome do computador (no formato `\\<computername>\` ) como parte do *KeyName*. Omitir `\\<computername>\` faz com que a operação seja padronizada para o computador local. Especificar apenas o nome do computador em *KeyName2* faz com que a operação use o caminho para a subchave especificada em *KeyName1*. O *KeyName* deve incluir uma chave de raiz válida. As chaves de raiz válidas para o computador local são: **HKLM**, **HKCU**, **HKCR**, **HKU**e **HKCC**. Se um computador remoto for especificado, as chaves de raiz válidas serão: **HKLM** e **HKU**. Se o nome da chave do registro contiver um espaço, coloque o nome da chave entre aspas. |
| /v`<Valuename>` | Especifica o nome do valor a ser comparado sob a subchave. |
| /ve | Especifica que somente as entradas que têm um nome de valor nulo devem ser comparadas. |
| /oa | Especifica que todas as diferenças e correspondências são exibidas. Por padrão, somente as diferenças são listadas. |
| /OD | Especifica que apenas as diferenças são exibidas. Esse é o comportamento padrão. |
| /os | Especifica que somente as correspondências são exibidas. Por padrão, somente as diferenças são listadas. |
| /on | Especifica que nada é exibido. Por padrão, somente as diferenças são listadas. |
| /s | Compara todas as subchaves e entradas recursivamente. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Os valores de retorno para a operação **reg Compare** são:

    | Valor | Descrição |
    |--|--|
    | 0 | A comparação é bem-sucedida e o resultado é idêntico. |
    | 1 | Falha na comparação. |
    | 2 | A comparação foi bem-sucedida e foram encontradas diferenças. |

- Os símbolos exibidos nos resultados incluem:

    | Símbolo | Descrição |
    |--|--|
    | = | Os dados de *KeyName1* são iguais aos dados de *KeyName2* . |
    | < | Os dados de *KeyName1* são menores que os dados de *KeyName2* . |
    | > | Os dados de *KeyName1* são maiores que os dados de *KeyName2* . |

### <a name="examples"></a>Exemplos

Para comparar todos os valores na chave **MyApp** com todos os valores na chave **SaveMyApp**, digite:

```
reg compare HKLM\Software\MyCo\MyApp HKLM\Software\MyCo\SaveMyApp
```

Para comparar o valor da versão sob a chave **MYCO** e o valor da versão sob a chave **MyCo1**, digite:

```
reg compare HKLM\Software\MyCo HKLM\Software\MyCo1 /v Version
```

Para comparar todas as subchaves e valores em HKLM\Software\MyCo no computador chamado ZODIAC, com todas as subchaves e valores em HKLM\Software\MyCo no computador local, digite:

```
reg compare \\ZODIAC\HKLM\Software\MyCo \\. /s
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
