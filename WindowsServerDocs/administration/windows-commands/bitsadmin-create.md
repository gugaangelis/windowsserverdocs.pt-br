---
title: bitsadmin create
description: Artigo de referência para o comando Bitsadmin Create, que cria um trabalho de transferência com o nome de exibição fornecido.
ms.topic: reference
ms.assetid: 9a8c53af-900b-4c24-9265-5b8b08213fac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 516f751109724688579f084fd0ccdb5f5bda85a3
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89026674"
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
