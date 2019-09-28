---
title: extract
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 967f08e271019cc33970419179c9ddbf902b1882
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71377264"
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
|gabinete|O arquivo contém dois ou mais arquivos.|
|filename|Nome do arquivo a ser extraído do gabinete. Caracteres curinga e vários nomes de (separados por espaços em branco) podem ser usados.|
|source|Arquivo compactado (um gabinete com apenas um arquivo).|
|NewName|Novo filename para fornecer o arquivo extraído. Se não for fornecido, o nome original será usado.|
|/A|Processar todos os gabinetes. Segue a cadeia de gabinetes começando no primeiro gabinete mencionado.|
|/C|Copie o arquivo de origem para o destino (para copiar de discos de DMF).|
|/D|Exibir diretório de gabinete (use com nome de arquivo para evitar extração).|
|/E|Extração (uso em vez de *.* para extrair todos os arquivos).|
|/L dir|Local para colocação de arquivos extraídos (o padrão é o diretório atual).|
|/Y|Não solicitar antes de substituir um arquivo existente.|

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)