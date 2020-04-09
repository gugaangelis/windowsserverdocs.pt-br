---
title: BdeHdCfg silencioso
description: O tópico de comandos do Windows para **BdeHdCfg Quiet**, que informa BdeHdCfg para não exibir todas as ações e erros.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7f75b702-890b-4ff9-805c-edf5cadd8822
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e9c24d8861476e6c1578af8245236d699b6ef6db
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851039"
---
# <a name="bdehdcfg-quiet"></a>BdeHdCfg: Quiet

Informa a ferramenta de linha de comando BdeHdCfg de que todas as ações e erros não devem ser exibidos na interface de linha de comando. Para obter um exemplo de como esse comando pode ser usado, consulte [exemplos](#BKMK_Examples).

## <a name="syntax"></a>Sintaxe

```
bdehdcfg -target {default|unallocated|<DriveLetter> shrink|<DriveLetter> merge} -quiet
```

#### <a name="parameters"></a>Parâmetros

Este comando não usa parâmetros adicionais.

## <a name="remarks"></a>Comentários

Se algum prompt Sim/Não (S/N) tiver sido exibido durante a preparação da unidade, uma resposta "Sim" será presumida. Para exibir os erros ocorridos durante a preparação da unidade, examine o log de eventos do sistema no provedor de eventos **Microsoft-Windows-BitLocker-DrivePreparationTool**.

## <a name="examples"></a><a name="BKMK_Examples"></a>Disso

O exemplo a seguir ilustra o uso do comando **Quiet** .

```
bdehdcfg -target default -quiet
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [BdeHdCfg](bdehdcfg.md)