---
title: bdehdcfg restart
description: Artigo de referência para o comando BdeHdCfg restart, que informa BdeHdCfg de que o computador deve ser reiniciado depois que a preparação da unidade for concluída.
ms.topic: reference
ms.assetid: a98b76bb-36f1-4790-b337-7dc35f606bc6
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 0839f22bed8dee63fb25f9b254e36b0c02c88399
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632832"
---
# <a name="bdehdcfg-restart"></a>BdeHdCfg: reiniciar

Informa a ferramenta de linha de comando BdeHdCfg de que o computador deve ser reiniciado depois que a preparação da unidade for concluída. Se outros usuários estiverem conectados ao computador e o comando **silencioso** não for especificado, um prompt será exibido para confirmar que o computador deve ser reiniciado.

## <a name="syntax"></a>Sintaxe

```
bdehdcfg -target {default|unallocated|<drive_letter> shrink|<drive_letter> merge} -restart
```

#### <a name="parameters"></a>Parâmetros

Este comando não tem parâmetros adicionais.

## <a name="examples"></a>Exemplos

Para usar o comando de **reinicialização** :

```
bdehdcfg -target default -restart
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
