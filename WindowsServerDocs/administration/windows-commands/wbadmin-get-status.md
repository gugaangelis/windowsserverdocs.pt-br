---
title: wbadmin get status
description: Artigo de referência para WBADMIN Get status, que relata o status da operação de backup ou recuperação que está em execução no momento.
ms.topic: reference
ms.assetid: 2911b944-7b95-46aa-8c1e-1d55a2fcc94c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3245a6c92bf8a3ebde070f2f85e484dc9a317ce1
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89031984"
---
# <a name="wbadmin-get-status"></a>wbadmin get status



Relata o status da operação de backup ou recuperação que está em execução no momento.

Para usar esse subcomando, você deve ser membro do grupo **operadores de backup** ou do grupo **Administradores** , ou você deve ter recebido as permissões apropriadas. Além disso, você deve executar o **Wbadmin** em um prompt de comandos com privilégios elevados. (Para abrir um prompt de comando com privilégios elevados, clique com o botão direito do mouse em **prompt de comando**e clique em **Executar como administrador**.)

## <a name="syntax"></a>Sintaxe

```
wbadmin get status
```

### <a name="parameters"></a>Parâmetros

Este subcomando não tem parâmetros.

## <a name="remarks"></a>Comentários

-   Esse subcomando não será interrompido até que a operação de backup ou recuperação atual seja concluída — o subcomando continuará a ser executado mesmo se você fechar a janela de comando.
-   Se você quiser interromper a operação de backup ou recuperação atual, use o subcomando **Wbadmin Stop Job** .

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Cmdlet [Get-WBJob](/powershell/module/windowserverbackup/?view=winserver2012r2-ps)
