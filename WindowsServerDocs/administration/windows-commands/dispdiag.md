---
title: dispdiag
description: Tópico de referência para o comando dispdiag, que logs exibem informações para um arquivo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5079e1dd-b57c-44ed-970f-e6b409369e03
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ff4e3690ec3b2c9d473f05027d5637eda124d0ba
ms.sourcegitcommit: aed942d11f1a361fc1d17553a4cf190a864d1268
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83235215"
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
