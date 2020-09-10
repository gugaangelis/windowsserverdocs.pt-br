---
title: ksetup removerealm
description: Artigo de referência para o comando ksetup removerealm, que exclui todas as informações do realm especificado do registro.
ms.topic: reference
ms.assetid: 39f0c6f0-4c50-4781-941e-0893495405e8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 324eb79fee80e53da4dd1bc331e5af49c64c1574
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89622761"
---
# <a name="ksetup-removerealm"></a>ksetup removerealm

Exclui todas as informações do realm especificado do registro.

O nome do realm é armazenado no registro em `HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001` e `\CurrentControlSet\Control\Lsa\Kerberos` . Por padrão, essa entrada não existe no registro. Você pode usar o comando [ksetup addrealmflags](ksetup-addrealmflags.md) para popular o registro.

> [!IMPORTANT]
> Não é possível remover o nome de realm padrão do controlador de domínio porque isso redefine suas informações de DNS e removê-lo pode tornar o controlador de domínio inutilizável.

## <a name="syntax"></a>Sintaxe

```
ksetup /removerealm <realmname>
```
### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<realmname>` | Especifica o nome DNS em maiúsculas, como CORP. CONTOSO.COM e é listado como o realm ou **Realm padrão =** quando **ksetup** é executado. |

### <a name="examples"></a>Exemplos

Para remover um nome de realm errado (. CON em vez de. COM) do computador local, digite:
```
ksetup /removerealm CORP.CONTOSO.CON
```

Para verificar a remoção, você pode executar o comando **ksetup** e examinar a saída.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando ksetup](ksetup.md)

- [comando ksetup setreale](ksetup-setrealm.md)
