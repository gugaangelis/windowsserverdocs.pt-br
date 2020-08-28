---
title: dispdiag
description: Artigo de referência para o comando dispdiag, que registra em log informações em um arquivo.
ms.topic: reference
ms.assetid: 5079e1dd-b57c-44ed-970f-e6b409369e03
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 92dd0b49d8907f3ec934fd59d61b0504b622e80b
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89030834"
---
# <a name="dispdiag"></a>dispdiag

Os logs exibem informações em um arquivo.

## <a name="syntax"></a>Sintaxe

```
dispdiag [-testacpi] [-d] [-delay <seconds>] [-out <filepath>]
```

#### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| - testacpi | Executa o teste de diagnóstico de tecla de atalho. Exibe o nome da chave, código e código de verificação para qualquer tecla pressionada durante o teste. |
| -d | Gera um arquivo de despejo com resultados de teste. |
| -atraso `<seconds>` | Atrasa a coleta de dados por tempo especificado em *segundos*. |
| -out `<filepath>`  | Especifica o caminho e o nome do arquivo para salvar os dados coletados. Esse deve ser o último parâmetro. |
| -? | Exibe os parâmetros de comando disponíveis e fornece ajuda para usá-los. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
