---
title: dispdiag
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5079e1dd-b57c-44ed-970f-e6b409369e03
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9c96c70aac1b3329e050fa8b02743e61fed44d15
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831457"
---
# <a name="dispdiag"></a>dispdiag



Logs de exibem informações para um arquivo.

## <a name="syntax"></a>Sintaxe

```
dispdiag [-testacpi] [-d] [-delay <Seconds>] [-out <FilePath>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|- testacpi|Executa o teste de diagnóstico de tecla de acesso. Exibe o nome da chave, código e Scanner do código para qualquer tecla pressionada durante o teste.|
|-d|Gera um arquivo de despejo com resultados de teste.|
|-delay \<Seconds>|Atrasa a coleta de dados pelo tempo especificado na *segundos*.|
|-out \<FilePath >|Especifica o caminho e nome de arquivo para salvar os dados coletados. Isso deve ser o último parâmetro.|
|-?|Exibe os parâmetros de comando disponíveis e fornece ajuda para usá-los.|