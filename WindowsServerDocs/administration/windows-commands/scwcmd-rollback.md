---
title: Scwcmd rollback
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4fd9f89b-0420-420a-ad20-4a328636b1e7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9679f8a8ef2e62f451d7ee01c3c5718825b5cbeb
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835139"
---
# <a name="scwcmd-rollback"></a>Scwcmd: rollback

> Aplica-se a: Windows Server 2012 R2, Windows Server 2012

Aplica a política de reversão mais recente disponível e, em seguida, exclui essa política de reversão.

## <a name="syntax"></a>Sintaxe

```
scwcmd rollback /m:<ComputerName> [/u:<UserName>] [/pw:<Password>]
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/m:\<ComputerName >|Especifica o nome NetBIOS, o nome DNS ou o endereço IP de um computador em que a operação de reversão deve ser executada.|
|/u:\<nome de usuário >|Especifica uma conta de usuário alternativa a ser usada ao executar uma reversão remota. O padrão é o usuário conectado.|
|/PW:\<> de senha|Especifica uma credencial de usuário alternativa a ser usada ao executar uma reversão remota. O padrão é o usuário conectado.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

Scwcmd. exe só está disponível em computadores que executam o Windows Server 2008 R2, o Windows Server 2008 ou o Windows Server 2003.

## <a name="examples"></a><a name=BKMK_Examples></a>Disso

Para reverter a política de segurança em um computador no endereço IP 172.16.0.0, digite:
```
scwcmd rollback /m:172.16.0.0
```

## <a name="additional-references"></a>Referências adicionais

-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)