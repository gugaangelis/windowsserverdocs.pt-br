---
title: 'ksetup: dumpstate'
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 625d05b2fea9ae58681648c64e309aa8b2a201ed
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374997"
---
# <a name="ksetupdumpstate"></a>ksetup: dumpstate



Exibe o estado atual das configurações de realm para todos os territórios definidos no computador. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
ksetup /dumpstate
```

### <a name="parameters"></a>Parâmetros

Nenhuma

## <a name="remarks"></a>Comentários

A saída desse comando inclui o realm padrão (o domínio do qual o computador é membro) e todos os territórios definidos neste computador. O seguinte está incluído para cada realm:
-   Todos os KDCs (centros de distribuição de chaves) associados a este Realm
-   Todos os sinalizadores de **Realm definido** para este Realm
-   A senha do KDC

Esse comando não exibe o nome de domínio que é especificado pela detecção de DNS ou pelo comando **ksetup/Domain**.

Esse comando não exibe a senha do computador que é definida usando o comando **ksetup/setcomputerpassword**.

**Ksetup** produz a mesma saída que **Ksetup/dumpstate**.

## <a name="BKMK_Examples"></a>Disso

Encontre a maioria das configurações de realm do Kerberos em um computador:
```
ksetup /dumpstate
```

#### <a name="additional-references"></a>Referências adicionais

-   [Ksetup](ksetup.md)
-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)