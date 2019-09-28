---
title: bitsadmin addfileset
description: O tópico de comandos do Windows para **Bitsadmin addfileset** -adiciona um ou mais arquivos ao trabalho especificado.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 75466994-262f-4724-b14d-f813c5397675
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 464f2da151d5a7bfffde286e52d9158560d48dcc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381994"
---
# <a name="bitsadmin-addfileset"></a>bitsadmin addfileset

Adiciona um ou mais arquivos ao trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /addfileset <Job> <TextFile>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|O nome de exibição ou o GUID do trabalho|
|TextFile|Um arquivo de texto, cada linha que contém um nome de arquivo remoto e local.</br>Observação: Os nomes são delimitados por espaço. As linhas que começam com um caractere # são tratadas como um comentário.|

## <a name="BKMK_examples"></a>Disso

```
C:\>bitsadmin /addfileset files.txt
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)