---
title: criar
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 70a8f53d9cb90fc36a76b11de2f93da71874617a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881267"
---
# <a name="create"></a>criar



Inicia o processo de criação de cópia de sombra, usando as configurações de opção e o contexto atuais. Requer pelo menos um volume no conjunto de cópia de sombra.

## <a name="syntax"></a>Sintaxe

```
create
```

## <a name="remarks"></a>Comentários

-   Você deve adicionar pelo menos um volume com o **Adicionar volume** comando antes de poder usar o **criar** comando.
-   Você pode usar o **Iniciar backup** comando para especificar um backup completo, em vez de um backup de cópia.
-   Depois de executar o **crie** de comando, você pode usar o **exec** comando para executar um script de eliminação de duplicação para backup de cópia de sombra.

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)