---
title: dispdiag
description: Tópico de comandos do Windows para dispdiag, que registra em log informações em um arquivo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5079e1dd-b57c-44ed-970f-e6b409369e03
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 498294aa6678cc4904880128bca55d7900c91db5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80845379"
---
# <a name="dispdiag"></a>dispdiag

Os logs exibem informações em um arquivo.

## <a name="syntax"></a>Sintaxe

```
dispdiag [-testacpi] [-d] [-delay <Seconds>] [-out <FilePath>]
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|- testacpi|Executa o teste de diagnóstico de tecla de atalho. Exibe o nome da chave, código e código de verificação para qualquer tecla pressionada durante o teste.|
|-d|Gera um arquivo de despejo com resultados de teste.|
|-atraso \<segundos >|Atrasa a coleta de dados por tempo especificado em *segundos*.|
|-out \<FilePath >|Especifica o caminho e o nome do arquivo para salvar os dados coletados. Esse deve ser o último parâmetro.|
|-?|Exibe os parâmetros de comando disponíveis e fornece ajuda para usá-los.|