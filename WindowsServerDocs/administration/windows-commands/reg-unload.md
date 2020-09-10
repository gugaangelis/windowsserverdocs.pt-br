---
title: reg unload
description: Artigo de referência para o comando reg Unload, que remove uma seção do registro carregado usando a operação reg Load.
ms.topic: reference
ms.assetid: 1d07791d-ca27-454e-9797-27d7e84c5048
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 5a94f4c3193a7096bd99d83a927ccb0b694d7faa
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640304"
---
# <a name="reg-unload"></a>reg unload

Remove uma seção do registro que foi carregado usando a operação **reg Load** .

## <a name="syntax"></a>Sintaxe

```
reg unload <keyname>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<keyname>` | Especifica o caminho completo da subchave. Para especificar um computador remoto, inclua o nome do computador (no formato `\\<computername>\` ) como parte do *KeyName*. Omitir `\\<computername>\` faz com que a operação seja padronizada para o computador local. O *KeyName* deve incluir uma chave de raiz válida. As chaves de raiz válidas para o computador local são: **HKLM**, **HKCU**, **HKCR**, **HKU**e **HKCC**. Se um computador remoto for especificado, as chaves de raiz válidas serão: **HKLM** e **HKU**. Se o nome da chave do registro contiver um espaço, coloque o nome da chave entre aspas. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Os valores de retorno para a operação **reg Unload** são:

    | Valor | Descrição |
    |--|--|
    | 0 | Êxito |
    | 1 | Falha |

## <a name="examples"></a>Exemplos

Para descarregar o hive TempHive no arquivo HKLM, digite:

```
reg unload HKLM\TempHive
```

> [!CAUTION]
> Não edite o registro diretamente, a menos que você não tenha nenhuma alternativa. O editor do registro ignora as proteções padrão, permitindo que as configurações possam prejudicar o desempenho, danificar o sistema ou até mesmo exigir que você reinstale o Windows. Você pode alterar com segurança a maioria das configurações do registro usando os programas no painel de controle ou no console de gerenciamento Microsoft (MMC). Se você precisar editar o registro diretamente, faça o backup primeiro.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando reg Load](reg-load.md)
