---
title: mklink
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0ce4df22-2dbc-48fc-9c16-b721ae85f857
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e4bfa1c928b5bc5f4c5a885378f0f1d1c9b99cf5
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2020
ms.locfileid: "83437131"
---
# <a name="mklink"></a>mklink
Cria um link simbólico.



## <a name="syntax"></a>Sintaxe

```
mklink [[/d] | [/h] | [/j]] <Link> <Target>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/d|Cria um link simbólico de diretório. Por padrão, o **MKLINK** cria um link simbólico de arquivo.|
|/h|Cria um link físico em vez de um link simbólico.|
|/j|Cria uma junção de diretório.|
|\<> de link|Especifica o nome do link simbólico que está sendo criado.|
|\<Target>|Especifica o caminho (relativo ou absoluto) ao qual o novo link simbólico se refere.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="examples"></a>Exemplos

Demonstra a criação e a remoção de um link simbólico chamado MyFolder e MyFile. File do diretório raiz para o diretório \Users\User1\Documents e um exemplo. File localizado no diretório:
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
