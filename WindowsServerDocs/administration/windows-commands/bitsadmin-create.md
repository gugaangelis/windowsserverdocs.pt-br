---
title: bitsadmin create
description: Artigo de referência para o comando Bitsadmin Create, que cria um trabalho de transferência com o nome de exibição fornecido.
ms.topic: reference
ms.assetid: 9a8c53af-900b-4c24-9265-5b8b08213fac
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7d621f3c03f2b002c88792bc2cf2b8f2c70351c2
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632362"
---
# <a name="bitsadmin-create"></a>bitsadmin create

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria um trabalho de transferência com o nome de exibição fornecido.

> [!NOTE]
> Os **/Upload**   tipos de parâmetro/upload e **/upload-Reply** não têm suporte do bits 1,2 e anteriores.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /create [type] displayname
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| ------- | -------- |
| type | Há três tipos de trabalho:<ul><li>**Baixar.** Transfere dados de um servidor para um arquivo local.</li><li>**Carregar.** Transfere dados de um arquivo local para um servidor.</li><li>**/Upload-Reply.** Transfere dados de um arquivo local para um servidor e recebe um arquivo de resposta do servidor.</li></ul>O padrão desse parâmetro será **/Download** se não for especificado. |
| displayname | O nome de exibição atribuído ao trabalho recém-criado. |

## <a name="examples"></a>Exemplos

Para criar um trabalho de download chamado *myDownloadJob*:

```
bitsadmin /create myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando retomar Bitsadmin](bitsadmin-resume.md)

- [comando Bitsadmin](bitsadmin.md)
