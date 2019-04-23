---
title: set_2
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: acf24663-1a50-4321-b48d-1717655e9476
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3f5523646fddbfec31cb3900fc09230efc1c7813
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862607"
---
# <a name="set2"></a>set_2



Define o contexto, opções, o modo detalhado e arquivo de metadados para criação de cópias de sombra. Se usado sem parâmetros, **definir** lista todas as configurações atuais.

## <a name="syntax"></a>Sintaxe

```
set
set context {clientaccessible | persistent [nowriters] | volatile [nowriters]}
set option {[differential | plex] [transportable] [[rollbackrecover] [txfrecover] | [noautorecover]]}
set verbose {on|off}
set metadata <MetaData.cab>
```

## <a name="set-sub-commands"></a>Conjunto de subcomandos

|Subcomando|Descrição|
|-----------|-----------|
|context|Define o contexto para a criação de cópias de sombra. Ver [definir o contexto de](set-context.md) para sintaxe e parâmetros.|
|Opção|Define opções para criação de cópias de sombra. Ver [defina a opção](set-option.md) para sintaxe e parâmetros.|
|modo detalhado|Ativa ou desativa o modo de saída detalhada. Ver [definido detalhado](set-verbose.md) para sintaxe e parâmetros.|
|metadata|Define o nome e local do arquivo de metadados de criação de sombra. Ver [definir metadados](set-metadata.md) para sintaxe e parâmetros.|
|/?|Exibe a ajuda no prompt de comando.|

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)