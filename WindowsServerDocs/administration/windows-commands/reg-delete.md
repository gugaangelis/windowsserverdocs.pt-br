---
title: reg delete
description: Artigo de referência do comando reg Delete, que exclui uma subchave ou entradas do registro.
ms.topic: article
ms.assetid: cee05071-1607-4ab1-b8ab-65caebeb85c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: da541f55117e287df81b53a45c923ed2ed3ae028
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87884156"
---
# <a name="reg-delete"></a>reg delete

Exclui uma subchave ou entradas do registro.

## <a name="syntax"></a>Sintaxe

```
reg delete <keyname> [{/v Valuename | /ve | /va}] [/f]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<keyname1>` | Especifica o caminho completo da subchave ou entrada a ser adicionada. Para especificar um computador remoto, inclua o nome do computador (no formato `\\<computername>\` ) como parte do *KeyName*. Omitir `\\<computername>\` faz com que a operação seja padronizada para o computador local. O *KeyName* deve incluir uma chave de raiz válida. As chaves de raiz válidas para o computador local são: **HKLM**, **HKCU**, **HKCR**, **HKU**e **HKCC**. Se um computador remoto for especificado, as chaves de raiz válidas serão: **HKLM** e **HKU**. Se o nome da chave do registro contiver um espaço, coloque o nome da chave entre aspas. |
| /v`<Valuename>` | Exclui uma entrada específica sob a subchave. Se nenhuma entrada for especificada, todas as entradas e subchaves na subchave serão excluídas. |
| /ve | Especifica que somente as entradas que não têm nenhum valor serão excluídas. |
| /va | Exclui todas as entradas na subchave especificada. As subchaves na subchave especificada não são excluídas. |
| /f | Exclui a subchave ou entrada do registro existente sem solicitar confirmação. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Os valores de retorno para a operação **reg Delete** são:

    | Valor | Descrição |
    |--|--|
    | 0 | Êxito |
    | 1 | Falha |

### <a name="examples"></a>Exemplos

Para excluir o tempo limite da chave do registro e suas subchaves e valores, digite:

```
reg delete HKLM\Software\MyCo\MyApp\Timeout
```

Para excluir o MTU do valor do registro em HKLM\Software\MyCo no computador chamado ZODIAC, digite:

```
reg delete \\ZODIAC\HKLM\Software\MyCo /v MTU
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
