---
title: ksetup dumpstate
description: Artigo de referência para o comando dumpstate ksetup, que exibe o estado atual das configurações de realm para todos os territórios definidos no computador.
ms.topic: reference
ms.assetid: 3ef2f7b8-97af-4f42-9542-cff324840637
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 5f9d16a094dd3d3bbcdd5e9cb740e1bca5203a6d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639662"
---
# <a name="ksetup-dumpstate"></a>ksetup dumpstate

Exibe o estado atual das configurações de realm para todos os territórios definidos no computador. Esse comando exibe a mesma saída que o comando **ksetup** .

## <a name="syntax"></a>Sintaxe

```
ksetup /dumpstate
```

### <a name="remarks"></a>Comentários

- A saída desse comando inclui o realm padrão (o domínio do qual o computador é membro) e todos os territórios definidos neste computador. O seguinte está incluído para cada realm:

  - Todos os KDCs (centros de distribuição de chaves) associados a esse realm.

  - Todos os sinalizadores de **Realm definido** para esse realm.

  - A senha do KDC.

- Esse comando não exibe o nome de domínio especificado pela detecção de DNS ou pelo comando `ksetup /domain` .

- Esse comando não exibe a senha do computador definida usando o comando `ksetup /setcomputerpassword` .

## <a name="examples"></a>Exemplos

Para localizar as configurações de realm Kerberos em um computador, digite:

```
ksetup /dumpstate
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando ksetup](ksetup.md)