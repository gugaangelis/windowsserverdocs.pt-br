---
title: O comando Update-ServerFiles
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 23aa79df-38c6-401e-91bd-cd23811b30b4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ec96e2ba9aea14ed9a203dabbb697187736b33a8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817437"
---
# <a name="the-update-serverfiles-command"></a>O comando Update-ServerFiles



Atualiza os arquivos na pasta compartilhada REMINST usando os arquivos mais recentes que são armazenados na pasta de %Windir%\System32\RemInst do servidor. Para garantir a validade de sua instalação dos serviços de implantação do Windows, você deve executar esse comando uma vez após cada atualização do servidor, instalação do service pack ou atualização para os arquivos de serviços de implantação do Windows.

## <a name="syntax"></a>Sintaxe

```
WDSUTIL [Options] /Update-ServerFiles [/Server:<Server name>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[/Server:\<Server name>]|Especifica o nome do servidor. Pode ser o nome NetBIOS ou o FQDN (nome de domínio totalmente qualificado). Se nenhum nome de servidor for especificado, o servidor local será usado.|

## <a name="BKMK_examples"></a>Exemplos

Para atualizar os arquivos, digite o seguinte:
```
WDSUTIL /Update-ServerFiles
WDSUTIL /Verbose /Progress /Update-ServerFiles /Server:MyWDSServer
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)