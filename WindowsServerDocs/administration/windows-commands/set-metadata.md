---
title: Definir metadados
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: ac73a4131d3f4065cd1aeae873734b079ad664e2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370947"
---
# <a name="set-metadata"></a>Definir metadados



Define o nome e o local do arquivo de metadados de criação de sombra usado para transferir cópias de sombra de um computador para outro. Se usado sem parâmetros, **definir metadados** exibirá a ajuda no prompt de comando.

## <a name="syntax"></a>Sintaxe

```
set metadata [<Drive>:][<Path>]<MetaData.cab>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[\<Drive >:] [<Path>]|Especifica o local para criar o arquivo de metadados.|
|@no__t -0MetaData. cab >|Especifica o nome do arquivo CAB para armazenar metadados de criação de sombra.|

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)