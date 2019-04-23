---
title: ksetup:dumpstate
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3ef2f7b8-97af-4f42-9542-cff324840637
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8e5e8f20188fc27cc08dfd37c5fdbd811925f476
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863117"
---
# <a name="ksetupdumpstate"></a>ksetup:dumpstate



Exibe o estado atual das configurações de realm para todos os realms que são definidos no computador. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /dumpstate
```

### <a name="parameters"></a>Parâmetros

Nenhuma

## <a name="remarks"></a>Comentários

A saída desse comando inclui o domínio padrão (o domínio que o computador é membro) e todos os realms que são definidos neste computador. A seguir está incluído para cada território:
-   Todas as chave centros de distribuição (KDCs) que estão associados esse realm
-   Todos os as **realm conjunto** sinalizadores para esse realm
-   A senha do KDC

Esse comando não exibe o nome de domínio que é especificado pela detecção de DNS ou pelo comando **ksetup /domain**.

Esse comando exibe a senha do computador que é definida usando o comando **ksetup /setcomputerpassword**.

**Ksetup** produz a mesma saída que **ksetup /dumpstate**.

## <a name="BKMK_Examples"></a>Exemplos

Encontre a maior parte das configurações de realm de Kerberos em um computador:
```
ksetup /dumpstate
```

#### <a name="additional-references"></a>Referências adicionais

-   [Ksetup](ksetup.md)
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)