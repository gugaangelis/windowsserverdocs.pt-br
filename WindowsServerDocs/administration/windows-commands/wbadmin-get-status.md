---
title: obter o status da WBADMIN
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2911b944-7b95-46aa-8c1e-1d55a2fcc94c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 35fd640aa56bca7c5f5d6f3901fe095d0b8a73cc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863407"
---
# <a name="wbadmin-get-status"></a>obter o status da WBADMIN



Relata o status da operação de backup ou recuperação em execução no momento.

Para usar este subcomando, você deve ser um membro do **operadores de Backup** grupo ou o **administradores** grupo, ou você deve ter sido recebido as permissões apropriadas. Além disso, você deve executar **wbadmin** em um prompt de comando elevado. (Para abrir um atalho de prompt de comando com privilégios elevados **Prompt de comando**e, em seguida, clique em **executar como administrador**.)

## <a name="syntax"></a>Sintaxe

```
wbadmin get status
```

## <a name="parameters"></a>Parâmetros

Esse subcomando não tem parâmetros.

## <a name="remarks"></a>Comentários

-   Esse subcomando não será interrompida até que o backup atual ou a operação de recuperação for concluída, o subcomando continue em execução, mesmo se você fechar a janela de comando.
-   Se você quiser interromper o backup atual ou a operação de recuperação, use o **wbadmin parar trabalho** subcomando.

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Get-WBJob](https://technet.microsoft.com/library/jj902426.aspx) cmdlet