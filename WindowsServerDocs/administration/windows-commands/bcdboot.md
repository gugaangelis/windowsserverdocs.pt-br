---
title: bcdboot
description: Tópico de referência para o comando do BCDboot, que configura rapidamente uma partição do sistema ou repara o ambiente de inicialização localizado na partição do sistema.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e62a250e-08e9-47f6-89d1-e6002560ab57
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cde91738f524350f72f0278495e4bd46a3960e6f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718707"
---
# <a name="bcdboot"></a>bcdboot

Permite que você configure rapidamente uma partição do sistema ou repare o ambiente de inicialização localizado na partição do sistema. A partição do sistema é configurada copiando um conjunto simples de arquivos de Dados de Configuração da Inicialização (BCD) para uma partição vazia existente.

## <a name="syntax"></a>Sintaxe

```
bcdboot <source> [/l] [/s]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| source | Especifica o local do diretório do Windows a ser usado como a origem para copiar os arquivos do ambiente de inicialização. |
| /l | Especifica a localidade. A localidade padrão é inglês americano. |
| /s | Especifica a letra de volume da partição do sistema. O padrão é a partição do sistema identificada pelo firmware. |

## <a name="examples"></a>Exemplos

Para obter informações sobre onde encontrar o BCDboot e exemplos de como usar esse comando, consulte o tópico [Opções de linha de comando do BCDboot](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh824874(v=win.10)x) .

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
