---
title: set_2
description: O tópico de comandos do Windows para set_2, que define o contexto, as opções, o modo detalhado e o arquivo de metadados para a criação da cópia de sombra.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: acf24663-1a50-4321-b48d-1717655e9476
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fa467625997824a11b2303572a063d591f59bdd6
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834379"
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
|contexto|Define o contexto para a criação da cópia de sombra. Consulte [Definir contexto](set-context.md) para sintaxe e parâmetros.|
|opção|Define opções para a criação da cópia de sombra. Consulte [definir opção](set-option.md) para sintaxe e parâmetros.|
|modo detalhado|Ativa ou desativa o modo de saída detalhado. Consulte [definir detalhado](set-verbose.md) para sintaxe e parâmetros.|
|los|Define o nome e o local do arquivo de metadados de criação de sombra. Consulte [definir metadados](set-metadata.md) para sintaxe e parâmetros.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)