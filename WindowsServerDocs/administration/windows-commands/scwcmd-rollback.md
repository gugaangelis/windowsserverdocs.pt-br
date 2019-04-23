---
title: Scwcmd rollback
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4fd9f89b-0420-420a-ad20-4a328636b1e7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6d6cd79c7068d86915141a37b5a4510bddefc94c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852197"
---
# <a name="scwcmd-rollback"></a>Scwcmd: rollback

> Aplica-se a: Windows Server 2012 R2, Windows Server 2012

Aplica-se a política de reversão mais recente disponível e, em seguida, exclui essa diretiva de reversão.

## <a name="syntax"></a>Sintaxe

```
scwcmd rollback /m:<ComputerName> [/u:<UserName>] [/pw:<Password>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/m:\<ComputerName>|Especifica o nome NetBIOS, nome DNS ou endereço IP de um computador em que a operação de reversão deve ser executada.|
|/u:\<UserName>|Especifica uma conta de usuário alternativas para usar ao executar uma reversão remota. O padrão é o conectado no usuário.|
|/pw:\<senha >|Especifica uma credencial de usuário alternativas para usar ao executar uma reversão remota. O padrão é o conectado no usuário.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

Scwcmd.exe só está disponível em computadores que executam o Windows Server 2008 R2, Windows Server 2008 ou Windows Server 2003.

## <a name="BKMK_Examples"></a>Exemplos

Para reverter a política de segurança em um computador no endereço IP 172.16.0.0, digite:
```
scwcmd rollback /m:172.16.0.0
```

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)