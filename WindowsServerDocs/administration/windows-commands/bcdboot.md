---
title: bcdboot
description: Tópico de comandos do Windows para **bcdboot** – rapidamente configurar uma partição de sistema ou reparar o ambiente de inicialização localizado na partição do sistema.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 78838dd6567ad886948df8ac21425a8f9b596d5f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825877"
---
# <a name="bcdboot"></a>bcdboot



Permite configurar rapidamente uma partição de sistema ou para reparar o ambiente de inicialização localizado na partição do sistema. A partição do sistema é configurada por meio da cópia de um conjunto simple de arquivos de dados de configuração da inicialização (BCD) para uma partição vazia.

Para obter mais informações sobre BCDboot, incluindo informações sobre onde localizar BCDboot e exemplos de como usar esse comando, consulte o [opções de linha de comando BCDboot](https://technet.microsoft.com/library/hh824874.aspx) tópico.

## <a name="syntax"></a>Sintaxe

```
bcdboot <source> [/l] [/s]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Código-fonte|Especifica o local do diretório Windows a ser usado como a origem para copiar arquivos do ambiente de inicialização.|
|/l|Especifica a localidade. A localidade padrão é inglês (EUA).|
|/s|Especifica a letra de volume da partição do sistema. O padrão é a partição do sistema identificada pelo firmware.|

## <a name="BKMK_examples"></a>Exemplos

Para obter mais exemplos de como usar esse comando, consulte o [opções de linha de comando BCDboot](https://technet.microsoft.com/library/hh824874.aspx) tópico.

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)