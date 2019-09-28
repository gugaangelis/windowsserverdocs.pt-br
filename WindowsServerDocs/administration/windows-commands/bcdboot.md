---
title: bcdboot
description: Tópico de comandos do Windows para o **BCDboot** – configure rapidamente uma partição do sistema ou repare o ambiente de inicialização localizado na partição do sistema.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e62a250e-08e9-47f6-89d1-e6002560ab57
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a1c0f505180a503617335cc9575fea3d346bbe02
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383442"
---
# <a name="bcdboot"></a>bcdboot



Permite que você configure rapidamente uma partição do sistema ou repare o ambiente de inicialização localizado na partição do sistema. A partição do sistema é configurada copiando um conjunto simples de arquivos de Dados de Configuração da Inicialização (BCD) para uma partição vazia existente.

Para obter mais informações sobre o BCDboot, incluindo informações sobre onde encontrar o BCDboot e exemplos de como usar esse comando, consulte o tópico [Opções de linha de comando do BCDboot](https://technet.microsoft.com/library/hh824874.aspx) .

## <a name="syntax"></a>Sintaxe

```
bcdboot <source> [/l] [/s]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|source|Especifica o local do diretório do Windows a ser usado como a origem para copiar os arquivos do ambiente de inicialização.|
|/l|Especifica a localidade. A localidade padrão é inglês americano.|
|/s|Especifica a letra de volume da partição do sistema. O padrão é a partição do sistema identificada pelo firmware.|

## <a name="BKMK_examples"></a>Disso

Para obter mais exemplos de como usar esse comando, consulte o tópico [Opções de linha de comando do BCDboot](https://technet.microsoft.com/library/hh824874.aspx) .

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)