---
title: nslookup set search
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 064ac660-8b04-4af9-8b2c-e4e0549771b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d9da08a296d61789dbafeccde5d46c8a220d874c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372782"
---
# <a name="nslookup-set-search"></a>nslookup set search



Acrescenta os nomes de domínio DNS (sistema de nomes de domínio) na lista de pesquisa de domínio DNS à solicitação até que uma resposta seja recebida. Isso se aplica quando o conjunto e a solicitação de pesquisa contêm pelo menos um ponto, mas não terminam com um ponto à direita.

## <a name="syntax"></a>Sintaxe

```
set [no]search
```

## <a name="parameters"></a>Parâmetros

|  Parâmetro   |                                                                          Descrição                                                                          |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nosearch** |                            Interrompe a anexação dos nomes de domínio DNS (sistema de nomes de domínio) na lista de pesquisa de domínio DNS à solicitação.                            |
|  **procurando**  | Acrescenta os nomes de domínio DNS (sistema de nomes de domínio) na lista de pesquisa de domínio DNS à solicitação até que uma resposta seja recebida. A sintaxe padrão é **Search**. |
|    {ajuda     |                                                                              ?}                                                                               |

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)