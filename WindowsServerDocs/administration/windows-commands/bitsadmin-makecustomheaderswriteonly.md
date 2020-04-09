---
title: bitsadmin makecustomheaderswriteonly
description: Tópico de comandos do Windows para **Bitsadmin makecustomheaderswriteonly**, que tornam os cabeçalhos HTTP personalizados de um trabalho somente gravação.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 03/01/2019
ms.openlocfilehash: 9183b1b5de51020c5c6d2efad2c0a788d158a183
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850239"
---
# <a name="bitsadmin-makecustomheaderswriteonly"></a>bitsadmin makecustomheaderswriteonly

Faça com que os cabeçalhos HTTP personalizados de um trabalho somente sejam gravados.

> [!Important]
> Esta ação não pode ser desfeita.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /makecustomheaderswriteonly <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir torna os cabeçalhos HTTP personalizados somente gravação para o trabalho chamado *myDownloadJob*.

```
C:\>bitsadmin /makecustomheaderswriteonly myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)