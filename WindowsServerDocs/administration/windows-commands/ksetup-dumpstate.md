---
title: 'ksetup: dumpstate'
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3ef2f7b8-97af-4f42-9542-cff324840637
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 27a7e3154b9dfa663b88b04857ea7650995613c6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724648"
---
# <a name="ksetupdumpstate"></a>ksetup: dumpstate



Exibe o estado atual das configurações de realm para todos os territórios definidos no computador.

## <a name="syntax"></a>Sintaxe

```
ksetup /dumpstate
```

#### <a name="parameters"></a>Parâmetros

Nenhum

## <a name="remarks"></a>Comentários

A saída desse comando inclui o realm padrão (o domínio do qual o computador é membro) e todos os territórios definidos neste computador. O seguinte está incluído para cada realm:
-   Todos os KDCs (centros de distribuição de chaves) associados a este Realm
-   Todos os sinalizadores de **Realm definido** para este Realm
-   A senha do KDC

Esse comando não exibe o nome de domínio que é especificado pela detecção de DNS ou pelo comando **ksetup/Domain**.

Esse comando não exibe a senha do computador que é definida usando o comando **ksetup/setcomputerpassword**.

**Ksetup** produz a mesma saída que **Ksetup/dumpstate**.

## <a name="examples"></a>Exemplos

Encontre a maioria das configurações de realm do Kerberos em um computador:
```
ksetup /dumpstate
```

## <a name="additional-references"></a>Referências adicionais

-   [Ksetup](ksetup.md)
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)