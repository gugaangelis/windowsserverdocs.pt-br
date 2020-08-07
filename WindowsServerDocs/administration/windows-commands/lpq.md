---
title: lpq
description: Artigo de referência para o comando lpq, que exibe o status de uma fila de impressão em um computador que executa o LPD (daemon de impressora de linha).
ms.topic: article
ms.assetid: bb6abcc4-310a-4fa4-927b-4084b62ca02e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 550e94455ed7c57e723edb6608c42820e81fba0b
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87887066"
---
# <a name="lpq"></a>lpq

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe o status de uma fila de impressão em um computador que executa o LPD (daemon de impressora de linha).

## <a name="syntax"></a>Sintaxe

```
lpq -S <servername> -P <printername> [-l]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| -S`<servername>` | Especifica (por nome ou endereço IP) o computador ou o dispositivo de compartilhamento de impressora que hospeda a fila de impressão LPD com um status que você deseja exibir. Esse parâmetro é necessário e deve estar em letras maiúsculas. |
| -P`<Printername>` | Especifica (por nome) a impressora para a fila de impressão com um status que você deseja exibir. Esse parâmetro é necessário e deve estar em letras maiúsculas. |
| -l | Especifica que você deseja exibir detalhes sobre o status da fila de impressão. |
| /? | Exibe a ajuda no prompt de comando. |

### <a name="examples"></a>Exemplos

Para exibir o status da fila de impressora *Laserprinter1* em um host LPD em *10.0.0.45*, digite:

```
lpq -S 10.0.0.45 -P Laserprinter1
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Referência aos comandos de impressão](print-command-reference.md)
