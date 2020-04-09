---
title: bcdboot
description: Tópico de comandos do Windows para o **BCDboot**, que configura rapidamente uma partição do sistema ou repara o ambiente de inicialização localizado na partição do sistema.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e62a250e-08e9-47f6-89d1-e6002560ab57
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 637170cbd8ee4f3c11ee1dc77a0cd49b5dfa3038
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851079"
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
| {1&gt;source&lt;1} | Especifica o local do diretório do Windows a ser usado como a origem para copiar os arquivos do ambiente de inicialização. |
| /l | Especifica a localidade. A localidade padrão é inglês americano. |
| /s | Especifica a letra de volume da partição do sistema. O padrão é a partição do sistema identificada pelo firmware. |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para obter informações sobre onde encontrar o BCDboot e exemplos de como usar esse comando, consulte o tópico [Opções de linha de comando do BCDboot](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh824874(v=win.10)x) .

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)