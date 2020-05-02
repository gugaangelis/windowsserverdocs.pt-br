---
title: dispdiag
description: Tópico de referência para dispdiag, que registra informações de exibição em um arquivo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5079e1dd-b57c-44ed-970f-e6b409369e03
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f9f44e261b9c46157fb3e6bb7f9105af2480a60b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719404"
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
|-atraso \<em segundos>|Atrasa a coleta de dados por tempo especificado em *segundos*.|
|-saída \<de FilePath>|Especifica o caminho e o nome do arquivo para salvar os dados coletados. Esse deve ser o último parâmetro.|
|-?|Exibe os parâmetros de comando disponíveis e fornece ajuda para usá-los.|