---
title: Metadados de conjunto
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 67e6f60a-b42a-451a-95cf-b22ace7d50c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 82d2cd9ba447a0ea261f91dc01c11e45dfc0aa9b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835987"
---
# <a name="set-metadata"></a>Metadados de conjunto



Define o nome e local do arquivo de metadados de criação de sombra usado para transferir as cópias de sombra de um computador para outro. Se usado sem parâmetros, **definir metadados** exibe a Ajuda no prompt de comando.

## <a name="syntax"></a>Sintaxe

```
set metadata [<Drive>:][<Path>]<MetaData.cab>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[\<Drive>:][<Path>]|Especifica o local para criar o arquivo de metadados.|
|\<MetaData.cab>|Especifica o nome do arquivo cab para armazenar metadados de criação de sombra.|

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)