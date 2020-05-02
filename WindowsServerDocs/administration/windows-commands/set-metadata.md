---
title: Definir metadados
description: Tópico de referência para definir metadados, que define o nome e o local do arquivo de metadados de criação de sombra usado para transferir cópias de sombra de um computador para outro.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 67e6f60a-b42a-451a-95cf-b22ace7d50c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 683e54a7efc072d8709d6257771ba6bc5bde206e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721909"
---
# <a name="set-metadata"></a>Definir metadados

Define o nome e o local do arquivo de metadados de criação de sombra usado para transferir cópias de sombra de um computador para outro. Se usado sem parâmetros, **definir metadados** exibirá a ajuda no prompt de comando.

## <a name="syntax"></a>Sintaxe

```
set metadata [<Drive>:][<Path>]<MetaData.cab>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[\<Unidade>:] [<Path>]|Especifica o local para criar o arquivo de metadados.|
|\<> de MetaData. cab|Especifica o nome do arquivo CAB para armazenar metadados de criação de sombra.|

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)