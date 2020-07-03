---
title: bdehdcfg restart
description: Artigo de referência para o comando BdeHdCfg restart, que informa BdeHdCfg de que o computador deve ser reiniciado depois que a preparação da unidade for concluída.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a98b76bb-36f1-4790-b337-7dc35f606bc6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 02d57eaef9b2f6e705a5bb30d7d52a09579d27c9
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923468"
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
