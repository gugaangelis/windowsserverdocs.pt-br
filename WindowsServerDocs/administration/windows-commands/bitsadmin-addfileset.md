---
title: bitsadmin addfileset
description: Tópico de comandos do Windows para **addfileset bitsadmin** -adiciona um ou mais arquivos para o trabalho especificado.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: f8f6ff32dfa6042272c68647477d77183ce9cb76
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889437"
---
# <a name="bitsadmin-addfileset"></a>bitsadmin addfileset

Adiciona um ou mais arquivos para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /addfileset <Job> <TextFile>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|Job|Nome de exibição ou o GUID do trabalho|
|Arquivo de texto|Um arquivo de texto, cada linha que contém um controle remoto e um nome de arquivo local.</br>Observação: Os nomes são delimitados por espaço. As linhas que começam com um caractere # são tratadas como um comentário.|

## <a name="BKMK_examples"></a>Exemplos

```
C:\>bitsadmin /addfileset files.txt
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)