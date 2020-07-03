---
title: Definir metadados
description: Artigo de referência para definir metadados, que define o nome e o local do arquivo de metadados de criação de sombra usado para transferir cópias de sombra de um computador para outro.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 67e6f60a-b42a-451a-95cf-b22ace7d50c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 50c9ceebf072db2e7cefada1601accc97b5d0f7f
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85937095"
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
|[\<Drive>:][<Path>]|Especifica o local para criar o arquivo de metadados.|
|\<MetaData.cab>|Especifica o nome do arquivo CAB para armazenar metadados de criação de sombra.|

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)