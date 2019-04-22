---
title: bitsadmin create
description: Tópico de comandos do Windows para **bitsadmin criar** -cria um trabalho de transferência com o nome de exibição em questão.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d6ce5a4fdc21d879bf0a265e3c4185d83311464a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817187"
---
# <a name="bitsadmin-create"></a>bitsadmin create

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria um trabalho de transferência com o nome de exibição em questão. Baixe dados de transferência de trabalhos de um servidor em um arquivo local. Carrega dados de transferência de trabalhos de um arquivo local para um servidor. Trabalhos de resposta de upload transferir dados de um arquivo local para um servidor e recebem um arquivo de resposta do servidor.

Use o [bitsadmin retomar](bitsadmin-resume.md) switch para ativar o trabalho na fila de transferência.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /create [type] DisplayName
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------|--------|
|type|-   **/ Baixar** transfere os dados de um servidor em um arquivo local.<br />-   **/ Carregar** transfere os dados de um arquivo local para um servidor.<br />-   **/ Resposta de upload** transfere os dados de um arquivo local para um servidor e recebe um arquivo de resposta do servidor.<br />-Esse parâmetro assume como padrão **/baixar** quando não especificado na linha de comando.|
|DisplayName|O nome de exibição atribuído ao trabalho recém-criado.|

**BITS 1.2 e anteriores**: Os tipos de /Upload-Reply e /Upload não estão disponíveis.

## <a name="BKMK_examples"></a>Exemplos

Cria um trabalho de download denominado *myDownloadJob*.

```
C:\>bitsadmin /create myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
