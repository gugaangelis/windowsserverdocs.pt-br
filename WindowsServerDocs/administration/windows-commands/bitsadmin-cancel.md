---
title: bitsadmin cancel
description: O tópico de comandos do Windows para **Bitsadmin Cancel**, que remove o trabalho da fila de transferência e exclui todos os arquivos temporários associados ao trabalho.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7374b544-6a16-4d3e-872c-dcf4c02ad89d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5c2bdeef824bc269671cc5ae926fb77cd5726c58
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850829"
---
# <a name="bitsadmin-cancel"></a>bitsadmin cancel

Remove o trabalho da fila de transferência e exclui todos os arquivos temporários associados ao trabalho.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /cancel <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir remove o trabalho *myDownloadJob* da fila de transferência.

```
C:\>bitsadmin /cancel myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)