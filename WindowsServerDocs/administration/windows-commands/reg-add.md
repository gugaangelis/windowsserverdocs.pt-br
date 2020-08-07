---
title: reg add
description: Artigo de referência para o comando reg Add, que adiciona uma nova subchave ou entrada ao registro.
ms.topic: article
ms.assetid: d9ad143e-dc10-4e2e-a229-408393c40079
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 549c9e4ff0eb09e051debdee12003031a8443e18
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87884199"
---
# <a name="reg-add"></a>reg add

Adiciona uma nova subchave ou entrada ao registro.

## <a name="syntax"></a>Sintaxe

```
reg add <keyname> [{/v Valuename | /ve}] [/t datatype] [/s Separator] [/d Data] [/f]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<keyname>` | Especifica o caminho completo da subchave ou entrada a ser adicionada. Para especificar um computador remoto, inclua o nome do computador (no formato `\\<computername>\` ) como parte do *KeyName*. Omitir `\\<computername>\` faz com que a operação seja padronizada para o computador local. O *KeyName* deve incluir uma chave de raiz válida. As chaves de raiz válidas para o computador local são: **HKLM**, **HKCU**, **HKCR**, **HKU**e **HKCC**. Se um computador remoto for especificado, as chaves de raiz válidas serão: **HKLM** e **HKU**. Se o nome da chave do registro contiver um espaço, coloque o nome da chave entre aspas. |
| /v`<Valuename>` | Especifica o nome da entrada de registro de adição. |
| /ve | Especifica que a entrada de registro adicionada tem um valor nulo. |
| /t`<Type>` | Especifica o tipo para a entrada do registro. O *tipo* deve ser um dos seguintes:<ul><li>REG_SZ</li><li>REG_MULTI_SZ</li><li>REG_DWORD_BIG_ENDIAN</li><li>REG_DWORD</li><li>REG_BINARY</li><li>REG_DWORD_LITTLE_ENDIAN</li><li>REG_LINK</li><li>REG_FULL_RESOURCE_DESCRIPTOR</li><li>REG_EXPAND_SZ</li></ul> |
| /s`<Separator>` | Especifica o caractere a ser usado para separar várias instâncias de dados quando o tipo de dados **REG_MULTI_SZ** é especificado e mais de uma entrada é listada. Se não for especificado, o separador padrão será **\ 0**. |
| /d`<Data>` | Especifica os dados para a nova entrada do registro. |
| /f | Adiciona a entrada do registro sem solicitar confirmação. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Não é possível adicionar subárvores com esta operação. Esta versão do **reg** não solicita confirmação ao adicionar uma subchave.

- Os valores de retorno para a operação **reg Add** são:

| Valor | Descrição |
|--|--|
| 0 | Êxito |
| 1 | Falha |

- Para o tipo de chave **REG_EXPAND_SZ** , use o símbolo de cursor ( **^** ) com **%** dentro do parâmetro/d.

### <a name="examples"></a>Exemplos

Para adicionar a chave *HKLM\Software\MyCo* no computador remoto *ABC*, digite:

```
reg add \\ABC\HKLM\Software\MyCo
```

Para adicionar uma entrada de registro ao *HKLM\Software\MyCo* com um valor chamado *Data*, o tipo *REG_BINARY*e os dados de *fe340ead*, digite:

```
reg add HKLM\Software\MyCo /v Data /t REG_BINARY /d fe340ead
```

Para adicionar uma entrada de registro de valores múltiplos ao *HKLM\Software\MyCo* com um valor chamado *MRU*, o tipo *REG_MULTI_SZ*e os dados de *fax\0mail\0\0*, digite:

```
reg add HKLM\Software\MyCo /v MRU /t REG_MULTI_SZ /d fax\0mail\0\0
```

Para adicionar uma entrada de registro expandida ao *HKLM\Software\MyCo* com um valor chamado *Path*, o tipo *REG_EXPAND_SZ*e os dados de *% systemroot%*, digite:

```
reg add HKLM\Software\MyCo /v Path /t REG_EXPAND_SZ /d ^%systemroot^%
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
