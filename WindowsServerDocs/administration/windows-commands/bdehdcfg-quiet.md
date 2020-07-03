---
title: bdehdcfg quiet
description: Artigo de referência para o comando BdeHdCfg Quiet, que informa BdeHdCfg para não exibir todas as ações e erros.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7f75b702-890b-4ff9-805c-edf5cadd8822
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5b6a62314109d1c299187d87fba23b8e59669d66
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85923453"
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
