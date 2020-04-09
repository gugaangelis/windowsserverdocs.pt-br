---
ms.assetid: 6c6ff819-f349-4aea-b0be-1f637f631736
title: Fsutil wim
ms.prod: windows-server
manager: dmoss
ms.author: toklima
author: toklima
ms.technology: storage
audience: IT Pro
ms.topic: article
ms.date: 10/16/2017
ms.openlocfilehash: d4a8f2c008c1a28e498edb7726a8c209e91f41af
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80843919"
---
# <a name="fsutil-wim"></a>Fsutil wim
>Aplicável a: Windows Server (Canal Semestral), Windows Server 2016, Windows 10

Fornece funções para descobrir e gerenciar arquivos com suporte de imagem do Windows (WIM).

## <a name="syntax"></a>Sintaxe

```
fsutil wim [enumfiles] <drive name> <data source>
fsutil wim [enumwims] <drive name>
fsutil wim [queryfile] <filename>
fsutil wim [removewim] <drive name> <data source>
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------------|---------------|
|enumfiles|Enumera arquivos de backup do WIM.|
|nome da unidade de \<>|Especifica o nome da unidade.|
|\<> da fonte de dados|Especifica a fonte de dados.|
|enumwims|Enumera os arquivos WIM de backup.|
|queryfile|Consulta se o arquivo tem o suporte do WIM e, em caso afirmativo, exibe detalhes sobre o arquivo WIM.|
|\<nome de arquivo >|Especifica o nome do arquivo.|
|removewim|Remove um WIM do backup de arquivos.|




### <a name="examples"></a>Exemplos

Para enumerar os arquivos da unidade C: da fonte de dados 0, digite:

```
fsutil wim enumfiles C: 0
```

Para enumerar arquivos WIM de backup para a unidade C:, digite:

```
fsutil wim enumwims C:
```

Para ver se um arquivo tem o suporte do WIM, digite:

```
fsutil wim C:\Windows\Notepad.exe
```

Para remover o WIM de arquivos de backup para o volume C: e a fonte de dados 2, digite:

```
fsutil wim removewims C: 2
```

### <a name="additional-references"></a>Referências adicionais
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

[Fsutil](Fsutil.md)