---
title: reg load
description: Artigo de referência para o comando reg Load, que grava subchaves e entradas salvas em uma subchave diferente no registro.
ms.topic: reference
ms.assetid: 3b0b2b1b-f510-4108-9e9d-7057e924aa6e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8647b417999459b210986187bd523b3953a8b2d6
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028024"
---
# <a name="reg-load"></a>reg load

Grava subchaves e entradas salvas em uma subchave diferente no registro. Este comando destina-se ao uso com arquivos temporários que são usados para solução de problemas ou edição de entradas do registro.

## <a name="syntax"></a>Sintaxe

```
reg load <keyname> <filename>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<keyname>` | Especifica o caminho completo da subchave a ser carregada. Para especificar um computador remoto, inclua o nome do computador (no formato `\\<computername>\` ) como parte do *KeyName*. Omitir `\\<computername>\` faz com que a operação seja padronizada para o computador local. O *KeyName* deve incluir uma chave de raiz válida. As chaves de raiz válidas para o computador local são: **HKLM**, **HKCU**, **HKCR**, **HKU**e **HKCC**. Se um computador remoto for especificado, as chaves de raiz válidas serão: **HKLM** e **HKU**. Se o nome da chave do registro contiver um espaço, coloque o nome da chave entre aspas.  |
| `<filename>` | Especifica o nome e o caminho do arquivo a ser carregado. Esse arquivo deve ser criado com antecedência usando o comando **reg Save** e deve ter uma extensão. HIV. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Os valores de retorno para a operação **reg Load** são:

    | Valor | Descrição |
    |--|--|
    | 0 | Êxito |
    | 1 | Falha |

### <a name="examples"></a>Exemplos

Para carregar o arquivo chamado TempHive. HIV na chave HKLM\TempHive, digite:

```
reg load HKLM\TempHive TempHive.hiv
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando reg Save](reg-save.md)
