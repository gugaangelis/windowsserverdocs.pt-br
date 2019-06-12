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
ms.openlocfilehash: ca9f5f00bc92d7929f782be45562e80bba455d74
ms.sourcegitcommit: cd12ace92e7251daaa4e9fabf1d8418632879d38
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2019
ms.locfileid: "66501450"
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

O exemplo a seguir demonstra a criação e remoção de um link simbólico chamado MyFolder e MyFile.file do diretório raiz para o diretório \Users\User1\Documents e um example.file localizado dentro do diretório:
```
mklink /d \MyFolder \Users\User1\Documents
mklink /h \MyFile.file \User1\Documents\example.file
rd \MyFolder
del \MyFile.file
```
## <a name="additional-references"></a>Referências adicionais
-   [New-Item](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/new-item?view=powershell-6)
-   [del](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/del)
-   [rmdir](https://docs.microsoft.com/en-us/windows-server/administration/windows-commands/rd)
