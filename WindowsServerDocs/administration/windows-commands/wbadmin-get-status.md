---
title: Wbadmin obter status
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 0270a29e557ec135301753dd66c1f5f2404a8acc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362393"
---
# <a name="wbadmin-get-status"></a>Wbadmin obter status



Relata o status da operação de backup ou recuperação que está em execução no momento.

Para usar esse subcomando, você deve ser membro do grupo **operadores de backup** ou do grupo **Administradores** , ou você deve ter recebido as permissões apropriadas. Além disso, você deve executar o **Wbadmin** em um prompt de comandos com privilégios elevados. (Para abrir um prompt de comando com privilégios elevados, clique com o botão direito do mouse em **prompt de comando**e clique em **Executar como administrador**.)

## <a name="syntax"></a>Sintaxe

```
wbadmin get status
```

## <a name="parameters"></a>Parâmetros

Este subcomando não tem parâmetros.

## <a name="remarks"></a>Comentários

-   Esse subcomando não será interrompido até que a operação de backup ou recuperação atual seja concluída — o subcomando continuará a ser executado mesmo se você fechar a janela de comando.
-   Se você quiser interromper a operação de backup ou recuperação atual, use o subcomando **Wbadmin Stop Job** .

#### <a name="additional-references"></a>Referências adicionais

-   [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Cmdlet [Get-WBJob](https://technet.microsoft.com/library/jj902426.aspx)