---
title: extract
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 20dab03e-f6e1-4eb8-b8a1-fd6f1d97ee83
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9113c34b61b98fb738bc0aff03193ab73b1abbd7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882307"
---
# <a name="extract"></a>extract



## <a name="syntax"></a>Sintaxe

```
EXTRACT [/Y] [/A] [/D | /E] [/L dir] cabinet [filename ...]
EXTRACT [/Y] source [newname]
EXTRACT [/Y] /C source destination
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|gabinete|Arquivo contém dois ou mais arquivos.|
|filename|Nome do arquivo para extrair o gabinete. Curingas e vários nomes de arquivos (separados por espaços em branco) podem ser usados.|
|Código-fonte|Arquivo compactado (um gabinete com apenas um arquivo).|
|newname|Novo nome de arquivo para fornecer o arquivo extraído. Se não for fornecido, o nome original é usado.|
|/A|Processe todos os gabinetes. Segue a cadeia de gabinete começando no primeiro gabinete mencionado.|
|/C|Copie o arquivo de origem para destino (para copiar de discos DMF).|
|/D|Exibir o diretório de gabinete (uso com o nome de arquivo para evitar a extração).|
|/E|Extrair (use em vez de *.* para extrair todos os arquivos).|
|/L dir|Local para colocar os arquivos extraídos (o padrão é o diretório atual).|
|/Y|Não avisar antes de substituir um arquivo existente.|

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)