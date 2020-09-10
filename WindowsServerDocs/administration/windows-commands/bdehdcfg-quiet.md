---
title: bdehdcfg quiet
description: Artigo de referência para o comando BdeHdCfg Quiet, que informa BdeHdCfg para não exibir todas as ações e erros.
ms.topic: reference
ms.assetid: 7f75b702-890b-4ff9-805c-edf5cadd8822
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 463dd8d558c4704f0997e867af73c5775c3b9f07
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632873"
---
# <a name="bdehdcfg-quiet"></a>BdeHdCfg: Quiet

Informa a ferramenta de linha de comando BdeHdCfg de que todas as ações e erros não devem ser exibidos na interface de linha de comando. Qualquer prompt Sim/não (Y/N) exibido durante a preparação da unidade assumirá uma resposta "Sim". Para exibir os erros ocorridos durante a preparação da unidade, examine o log de eventos do sistema no provedor de eventos **Microsoft-Windows-BitLocker-DrivePreparationTool**.

## <a name="syntax"></a>Sintaxe

```
bdehdcfg -target {default|unallocated|<drive_letter> shrink|<drive_letter> merge} -quiet
```

#### <a name="parameters"></a>Parâmetros

Este comando não tem parâmetros adicionais.

## <a name="examples"></a>Exemplos

Para usar o comando **silencioso** :

```
bdehdcfg -target default -quiet
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [bdehdcfg](bdehdcfg.md)
