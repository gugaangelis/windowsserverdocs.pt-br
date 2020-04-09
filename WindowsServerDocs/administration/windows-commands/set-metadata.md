---
title: Definir metadados
description: Tópico de comandos do Windows para definir metadados, que define o nome e o local do arquivo de metadados de criação de sombra usado para transferir cópias de sombra de um computador para outro.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 67e6f60a-b42a-451a-95cf-b22ace7d50c2
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b5bd728650cf163f98a82ff1e6f88755c4cc1aea
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834519"
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
|[\<drive >:] [<Path>]|Especifica o local para criar o arquivo de metadados.|
|\<MetaData. cab >|Especifica o nome do arquivo CAB para armazenar metadados de criação de sombra.|

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)