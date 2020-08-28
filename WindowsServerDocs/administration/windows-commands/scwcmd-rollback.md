---
title: Scwcmd rollback
description: Artigo de referência para * * * *-
ms.topic: reference
ms.assetid: 4fd9f89b-0420-420a-ad20-4a328636b1e7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 25c726b649028f66ca97ebc0280175d1713b7ef7
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89036174"
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
|opção\<ComputerName>|Especifica o nome NetBIOS, o nome DNS ou o endereço IP de um computador em que a operação de reversão deve ser executada.|
|/u\<UserName>|Especifica uma conta de usuário alternativa a ser usada ao executar uma reversão remota. O padrão é o usuário conectado.|
|pt\<Password>|Especifica uma credencial de usuário alternativa a ser usada ao executar uma reversão remota. O padrão é o usuário conectado.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

Scwcmd.exe só está disponível em computadores que executam o Windows Server 2008 R2, o Windows Server 2008 ou o Windows Server 2003.

## <a name="examples"></a>Exemplos

Para reverter a política de segurança em um computador no endereço IP 172.16.0.0, digite:
```
scwcmd rollback /m:172.16.0.0
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)