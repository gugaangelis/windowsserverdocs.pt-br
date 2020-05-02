---
title: reinicialização BdeHdCfg
description: Tópico de referência para o comando BdeHdCfg restart, que informa ao BdeHdCfg que o computador deve ser reiniciado depois que a preparação da unidade for concluída.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a98b76bb-36f1-4790-b337-7dc35f606bc6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 684a6a24fe78c0a23ba954981121c7bd99ac56fb
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718630"
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
