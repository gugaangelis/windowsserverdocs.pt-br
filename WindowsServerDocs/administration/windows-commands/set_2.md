---
title: set_2
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: e91bee5f0d351e461d16ccd22478d67f26887728
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370906"
---
# <a name="set_2"></a>set_2



Define o contexto, as opções, o modo detalhado e o arquivo de metadados para a criação da cópia de sombra. Se usado sem parâmetros, **set** listará todas as configurações atuais.

## <a name="syntax"></a>Sintaxe

```
set
set context {clientaccessible | persistent [nowriters] | volatile [nowriters]}
set option {[differential | plex] [transportable] [[rollbackrecover] [txfrecover] | [noautorecover]]}
set verbose {on|off}
set metadata <MetaData.cab>
```

## <a name="set-sub-commands"></a>Definir subcomandos

|Subcomando|Descrição|
|-----------|-----------|
|Noticioso|Define o contexto para a criação da cópia de sombra. Consulte [Definir contexto](set-context.md) para sintaxe e parâmetros.|
|Option|Define opções para a criação da cópia de sombra. Consulte [definir opção](set-option.md) para sintaxe e parâmetros.|
|modo detalhado|Ativa ou desativa o modo de saída detalhado. Consulte [definir detalhado](set-verbose.md) para sintaxe e parâmetros.|
|Los|Define o nome e o local do arquivo de metadados de criação de sombra. Consulte [definir metadados](set-metadata.md) para sintaxe e parâmetros.|
|/?|Exibe a ajuda no prompt de comando.|

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)