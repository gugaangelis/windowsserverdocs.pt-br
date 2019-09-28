---
title: mklink
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 9d930cbf7acbfceab16f2fa619aaaac6e789c131
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373645"
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
|/d|Cria um link simbólico de diretório. Por padrão, o **MKLINK** cria um link simbólico de arquivo.|
|/h|Cria um link físico em vez de um link simbólico.|
|/j|Cria uma junção de diretório.|
|\<Link >|Especifica o nome do link simbólico que está sendo criado.|
|\<Target >|Especifica o caminho (relativo ou absoluto) ao qual o novo link simbólico se refere.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="BKMK_examples"></a>Disso

O exemplo seguintes demonstra a criação e a remoção de um link simbólico chamado MyFolder e MyFile. File do diretório raiz para o diretório \Users\User1\Documents e um exemplo. File localizado no diretório:
```
mklink /d \MyFolder \Users\User1\Documents
mklink /h \MyFile.file \User1\Documents\example.file
rd \MyFolder
del \MyFile.file
```
## <a name="additional-references"></a>Referências adicionais
-   [Novo item](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/new-item?view=powershell-6)
-   [del](https://docs.microsoft.com/windows-server/administration/windows-commands/del)
-   [rmdir](https://docs.microsoft.com/windows-server/administration/windows-commands/rd)
