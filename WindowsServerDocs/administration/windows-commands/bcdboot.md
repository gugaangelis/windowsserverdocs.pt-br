---
title: bcdboot
description: Artigo de referência para o comando do BCDboot, que configura rapidamente uma partição do sistema ou repara o ambiente de inicialização localizado na partição do sistema.
ms.topic: article
ms.assetid: e62a250e-08e9-47f6-89d1-e6002560ab57
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ccdacd1254b290160c81123dd419d1816ffccaf5
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87895183"
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

Para obter informações sobre onde encontrar o BCDboot e exemplos de como usar esse comando, consulte o tópico [Opções de linha de comando do BCDboot](/previous-versions/windows/it-pro/windows-8.1-and-8/hh824874(v=win.10)) .

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
