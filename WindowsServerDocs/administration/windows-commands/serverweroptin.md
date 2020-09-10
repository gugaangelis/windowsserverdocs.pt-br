---
title: serverweroptin
description: Artigo de referência para * * * *-
ms.topic: reference
ms.assetid: f3c0b0af-cafb-4f09-8b36-5a357ddf392d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 73bdce2a4b18f55239b61132df0417b02cb683c2
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637788"
---
# <a name="serverweroptin"></a>serverweroptin

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Permite que você habilitar relatório de erros.
## <a name="syntax"></a>Sintaxe
```
serverweroptin [/query] [/detailed] [/summary]
```
#### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/Query|verifica a configuração atual.|
|/ detalhadas|Envia relatórios detalhados automaticamente.|
|Resumo|Envia automaticamente relatórios resumidos.|
|/?|Exibe a ajuda no prompt de comando.|
## <a name="examples"></a>Exemplos
Para verificar a configuração atual, digite:
```
serverweroptin /query
```
Para enviar automaticamente relatórios detalhados, digite:
```
serverweroptin /detailed
```
Para enviar automaticamente relatórios de resumo, digite
```
serverweroptin /summary
```
## <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

