---
ms.assetid: 6c6ff819-f349-4aea-b0be-1f637f631736
title: Wim fsutil
ms.prod: windows-server-threshold
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: c9186721ce4d3a549964e420cbc16d4893a1859d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826037"
---
# <a name="fsutil-wim"></a>Wim fsutil
>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows 10

Fornece funções para descobrir e gerenciar arquivos WIM de imagem do Windows com suporte.

## <a name="syntax"></a>Sintaxe

```
fsutil wim [enumfiles] <drive name> <data source>
fsutil wim [enumwims] <drive name>
fsutil wim [queryfile] <filename>
fsutil wim [removewim] <drive name> <data source>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------------|---------------|
|enumfiles|Enumera os arquivos WIM com suporte.|
|\<drive name>|Especifica o nome da unidade.|
|\<fonte de dados >|Especifica a fonte de dados.|
|enumwims|Enumera os arquivos WIM de backup.|
|queryfile|Consultas se o arquivo é feito por WIM e, em caso afirmativo, exibe detalhes sobre o arquivo WIM.|
|\<filename>|Especifica o nome do arquivo.|
|removewim|Remove um WIM de arquivos de backup.|




### <a name="examples"></a>Exemplos

Para enumerar os arquivos para a unidade c: da fonte de dados 0, digite:

```
fsutil wim enumfiles C: 0
```

Para enumerar arquivos WIM de backup para a unidade c:, digite:

```
fsutil wim enumwims C:
```

Para ver se um arquivo é feito por WIM, digite:

```
fsutil wim C:\Windows\Notepad.exe
```

Para remover o WIM de fazendo backup de arquivos para a fonte de dados 2 e o volume c:, digite:

```
fsutil wim removewims C: 2
```

### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](Command-Line-Syntax-Key.md)

[Fsutil](Fsutil.md)