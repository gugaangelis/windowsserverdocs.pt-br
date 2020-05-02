---
title: bitsadmin listfiles
description: Tópico de referência para o comando Bitsadmin ListFiles, que lista os arquivos no trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ad0d1eaa-3bd8-45e5-8f72-4da7366f0d59
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6826c1ec2f624a06d11fedcb8ca9f14d86b7ec27
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717412"
---
# <a name="bitsadmin-listfiles"></a>bitsadmin listfiles

Lista os arquivos no trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /listfiles <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar a lista de arquivos para o trabalho chamado *myDownloadJob*:

```
bitsadmin /listfiles myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
