---
title: Wbadmin obter status
description: Tópico de referência para WBADMIN Get status, que relata o status da operação de backup ou recuperação que está em execução no momento.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2911b944-7b95-46aa-8c1e-1d55a2fcc94c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1e8e5194cd49770f72ce810f4652d9bc98af75e2
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720165"
---
# <a name="wbadmin-get-status"></a>Wbadmin obter status



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

-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Cmdlet [Get-WBJob](https://technet.microsoft.com/library/jj902426.aspx)