---
title: set_2
description: Artigo de referência para set_2, que define o contexto, as opções, o modo detalhado e o arquivo de metadados para a criação da cópia de sombra.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: acf24663-1a50-4321-b48d-1717655e9476
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1b25b30ad729eb4e1cbf455f02cdacc76c0a3ab3
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85934633"
---
# <a name="set_2"></a>set_2

Define o contexto, as opções, o modo detalhado e o arquivo de metadados para a criação da cópia de sombra. Se usado sem parâmetros, **set** listará todas as configurações atuais.

## <a name="syntax"></a>Syntax

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
|contexto|Define o contexto para a criação da cópia de sombra. Consulte [Definir contexto](set-context.md) para sintaxe e parâmetros.|
|Opção|Define opções para a criação da cópia de sombra. Consulte [definir opção](set-option.md) para sintaxe e parâmetros.|
|verbose|Ativa ou desativa o modo de saída detalhado. Consulte [definir detalhado](set-verbose.md) para sintaxe e parâmetros.|
|metadata|Define o nome e o local do arquivo de metadados de criação de sombra. Consulte [definir metadados](set-metadata.md) para sintaxe e parâmetros.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)