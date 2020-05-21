---
title: extract
description: Tópico de referência para o comando Extract, que extrai arquivos de um local de origem.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 20dab03e-f6e1-4eb8-b8a1-fd6f1d97ee83
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dbadcc555fc9bb0b02e568b1126a317a9d59d336
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2020
ms.locfileid: "83437181"
---
# <a name="extract"></a>extract

Extrai arquivos de um gabinete ou origem.

## <a name="syntax"></a>Sintaxe

```
extract [/y] [/a] [/d | /e] [/l dir] cabinet [filename ...]
extract [/y] source [newname]
extract [/y] /c source destination
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| gabinete | Use se você quiser extrair dois ou mais arquivos. |
| nome do arquivo | Nome do arquivo a ser extraído do gabinete. Caracteres curinga e vários nomes de (separados por espaços em branco) podem ser usados. |
| origem | Arquivo compactado (um gabinete com apenas um arquivo). |
| newname | Novo filename para fornecer o arquivo extraído. Se não for fornecido, o nome original será usado. |
| /a | Processar todos os gabinetes. Segue a cadeia de gabinetes começando no primeiro gabinete mencionado. |
| /c | Copie o arquivo de origem para o destino (para copiar de discos de DMF). |
| /d | Exibir diretório de gabinete (use com nome de arquivo para evitar extração). |
| /e | Extração (uso em vez de *.* para extrair todos os arquivos). |
| /l dir | Local para colocação de arquivos extraídos (o padrão é o diretório atual). |
| /y | Não avisar antes de substituir um arquivo existente. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
