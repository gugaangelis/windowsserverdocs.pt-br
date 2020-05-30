---
title: lpq
description: Tópico de referência para o comando lpq, que exibe o status de uma fila de impressão em um computador que executa o LPD (daemon de impressora de linha).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: bb6abcc4-310a-4fa4-927b-4084b62ca02e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1ecce9b1b4255e5e769fe76b0f753226d61fa916
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222727"
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
