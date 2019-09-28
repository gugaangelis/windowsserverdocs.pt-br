---
title: bitsadmin create
description: O tópico de comandos do Windows para **Bitsadmin Create** -cria um trabalho de transferência com o nome de exibição fornecido.
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9a8c53af-900b-4c24-9265-5b8b08213fac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9f6d641d44c56ea4ff11f48a725367de7dcf472a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381814"
---
# <a name="bitsadmin-create"></a>bitsadmin create

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria um trabalho de transferência com o nome de exibição fornecido. Baixar trabalhos transferir dados de um servidor para um arquivo local. Carregar trabalhos de transferência de dados de um arquivo local para um servidor. Upload – os trabalhos de resposta transferem dados de um arquivo local para um servidor e recebem um arquivo de resposta do servidor.

Use a opção [Bitsadmin resume](bitsadmin-resume.md) para ativar o trabalho na fila de transferência.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /create [type] DisplayName
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------|--------|
|type|-    **/Download** transfere dados de um servidor para um arquivo local.<br />-    **/upload** transfere dados de um arquivo local para um servidor.<br />-    **/upload-Reply** transfere dados de um arquivo local para um servidor e recebe um arquivo de resposta do servidor.<br />-Esse parâmetro assume o padrão de **/Download** quando não especificado na linha de comando.|
|DisplayName|O nome de exibição atribuído ao trabalho recém-criado.|

**BITS 1,2 e anteriores**: Os tipos/upload e/Upload-Reply não estão disponíveis.

## <a name="BKMK_examples"></a>Disso

Cria um trabalho de download chamado *myDownloadJob*.

```
C:\>bitsadmin /create myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)
