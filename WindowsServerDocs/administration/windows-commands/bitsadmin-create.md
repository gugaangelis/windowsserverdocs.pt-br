---
title: bitsadmin create
description: Tópico de referência para o comando Bitsadmin Create, que cria um trabalho de transferência com o nome de exibição fornecido.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9a8c53af-900b-4c24-9265-5b8b08213fac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 728027eb4680805c1f9a2afc32d8d37a14239597
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718213"
---
# <a name="bitsadmin-create"></a>bitsadmin create

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria um trabalho de transferência com o nome de exibição fornecido.

> [!NOTE]
> Os tipos de parâmetro **/upload** e **/upload-Reply** não têm suporte do bits 1,2 e anteriores.

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
