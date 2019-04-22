---
title: mklink
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0ce4df22-2dbc-48fc-9c16-b721ae85f857
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eabbf159a64ab5df7f45ece390d0c2fdb9956b80
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826327"
---
# <a name="mklink"></a>mklink
Cria um link simbólico.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
mklink [[/d] | [/h] | [/j]] <Link> <Target>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/d|Cria um link simbólico do diretório. Por padrão, **mklink** cria um link simbólico do arquivo.|
|/h|Cria um link físico, em vez de um link simbólico.|
|/j|Cria uma junção de diretório.|
|\<Link>|Especifica o nome do link simbólico que está sendo criado.|
|\<Destino >|Especifica o caminho (relativo ou absoluto) que o novo link simbólico refere-se a.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="BKMK_examples"></a>Exemplos

Para criar um link simbólico chamado MyDocs do diretório raiz para o diretório \Users\User1\Documents, digite:
```
mklink /d \MyDocs \Users\User1\Documents
```
## <a name="additional-references"></a>Referências adicionais
-   [New-Item](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/new-item?view=powershell-6)
