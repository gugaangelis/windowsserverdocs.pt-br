---
title: serverweroptin
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f3c0b0af-cafb-4f09-8b36-5a357ddf392d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 18b4a56888b3f23bf3bac4a12b2dba7079b50923
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834629"
---
# <a name="serverweroptin"></a>serverweroptin

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

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
## <a name="examples"></a><a name=BKMK_Examples></a>Disso
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
-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

