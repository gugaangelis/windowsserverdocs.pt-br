---
title: bitsadmin create
description: O tópico de comandos do Windows para **Bitsadmin Create**, que cria um trabalho de transferência com o nome de exibição fornecido.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9a8c53af-900b-4c24-9265-5b8b08213fac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4a922d9f15aff0a9bd064a7e987920adf3a9107d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850809"
---
# <a name="bitsadmin-create"></a>bitsadmin create

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria um trabalho de transferência com o nome de exibição fornecido. Baixar trabalhos transferir dados de um servidor para um arquivo local. Carregar trabalhos de transferência de dados de um arquivo local para um servidor. Upload – os trabalhos de resposta transferem dados de um arquivo local para um servidor e recebem um arquivo de resposta do servidor.

Use a opção [Bitsadmin resume](bitsadmin-resume.md) para ativar o trabalho na fila de transferência.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /create [type] displayname
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| ------- | -------- |
| type | -  **/Download** transfere dados de um servidor para um arquivo local.<p>-  **/upload** transfere dados de um arquivo local para um servidor.<p>-  **/upload-Reply** transfere dados de um arquivo local para um servidor e recebe um arquivo de resposta do servidor.<p>O padrão desse parâmetro é **/Download** quando não especificado na linha de comando. Além disso, os tipos **/Upload** e **/upload-Reply** não estão disponíveis no bits 1,2 e versões anteriores. |
| displayname | O nome de exibição atribuído ao trabalho recém-criado. |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Cria um trabalho de download chamado *myDownloadJob*.

```
C:\>bitsadmin /create myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
