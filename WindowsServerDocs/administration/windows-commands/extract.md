---
title: extract
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 20dab03e-f6e1-4eb8-b8a1-fd6f1d97ee83
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1cca89a356530e49fbf2b0610ff3ced1c5733847
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725651"
---
# <a name="extract"></a>extract



## <a name="syntax"></a>Sintaxe

```
EXTRACT [/Y] [/A] [/D | /E] [/L dir] cabinet [filename ...]
EXTRACT [/Y] source [newname]
EXTRACT [/Y] /C source destination
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|gabinete|O arquivo contém dois ou mais arquivos.|
|nome do arquivo|Nome do arquivo a ser extraído do gabinete. Caracteres curinga e vários nomes de (separados por espaços em branco) podem ser usados.|
|source|Arquivo compactado (um gabinete com apenas um arquivo).|
|newname|Novo filename para fornecer o arquivo extraído. Se não for fornecido, o nome original será usado.|
|/A|Processar todos os gabinetes. Segue a cadeia de gabinetes começando no primeiro gabinete mencionado.|
|/C|Copie o arquivo de origem para o destino (para copiar de discos de DMF).|
|/D|Exibir diretório de gabinete (use com nome de arquivo para evitar extração).|
|/E|Extração (uso em vez de *.* para extrair todos os arquivos).|
|/L dir|Local para colocação de arquivos extraídos (o padrão é o diretório atual).|
|/Y|Não solicitar antes de substituir um arquivo existente.|

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)