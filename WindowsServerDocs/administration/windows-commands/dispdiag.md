---
title: dispdiag
description: Artigo de referência para o comando dispdiag, que registra em log informações em um arquivo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5079e1dd-b57c-44ed-970f-e6b409369e03
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d95a53f60aa7e51450a640d5ec9c5a5e6ccb41a6
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931226"
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
| -atraso`<seconds>` | Atrasa a coleta de dados por tempo especificado em *segundos*. |
| -out`<filepath>`  | Especifica o caminho e o nome do arquivo para salvar os dados coletados. Esse deve ser o último parâmetro. |
| -? | Exibe os parâmetros de comando disponíveis e fornece ajuda para usá-los. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
