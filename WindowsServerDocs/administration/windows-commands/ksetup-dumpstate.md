---
title: 'ksetup: dumpstate'
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3ef2f7b8-97af-4f42-9542-cff324840637
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 46f827d26d867392db4cbef92cf5be496aee8d74
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80841509"
---
# <a name="ksetupdumpstate"></a>ksetup: dumpstate



Exibe o estado atual das configurações de realm para todos os territórios definidos no computador. Para obter exemplos de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

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

## <a name="examples"></a><a name=BKMK_Examples></a>Disso

Encontre a maioria das configurações de realm do Kerberos em um computador:
```
ksetup /dumpstate
```

## <a name="additional-references"></a>Referências adicionais

-   [Ksetup](ksetup.md)
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)