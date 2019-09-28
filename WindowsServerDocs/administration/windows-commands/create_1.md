---
title: criar
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 837aa449-9b60-41ae-9ef1-ef67af6e5918
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2245efb6c3bce8aecf8edf730694804ffbdc3d80
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71378778"
---
# <a name="create"></a>criar



inicia o processo de criação de cópia de sombra usando as configurações de contexto e opção atuais. Requer pelo menos um volume no conjunto de cópias de sombra.

## <a name="syntax"></a>Sintaxe

```
create
```

## <a name="remarks"></a>Comentários

-   Você deve adicionar pelo menos um volume com o comando **Add volume** antes de poder usar o comando **Create** .
-   Você pode usar o comando **Iniciar backup** para especificar um backup completo, em vez de um backup de cópia.
-   Depois de executar o comando **Create** , você pode usar o comando **exec** para executar um script de duplicação para backup da cópia de sombra.

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)