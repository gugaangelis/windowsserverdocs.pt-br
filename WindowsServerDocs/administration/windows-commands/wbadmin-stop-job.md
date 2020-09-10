---
title: wbadmin stop job
description: Artigo de referência para WBADMIN Stop Job, que cancela a operação de backup ou recuperação que está em execução no momento. As operações canceladas não podem ser reiniciadas — você deve executar novamente uma operação de backup ou recuperação cancelada desde o início.
ms.topic: reference
ms.assetid: 3b83b398-39c7-4410-bf17-5c1fb1a4f46d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7060fcac4c7a712695800534a23eddbcd043a4f8
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633642"
---
# <a name="wbadmin-stop-job"></a>wbadmin stop job



Cancela a operação de backup ou recuperação que está em execução no momento. As operações canceladas não podem ser reiniciadas — você deve executar novamente uma operação de backup ou recuperação cancelada desde o início.

Para interromper uma operação de backup ou recuperação com este subcomando, você deve ser membro do grupo **operadores de backup** ou do grupo **Administradores** ou ter recebido a autoridade apropriada. Além disso, você deve executar o **Wbadmin** em um prompt de comandos com privilégios elevados. (Para abrir um prompt de comando com privilégios elevados, clique com o botão direito do mouse em **prompt de comando** e clique em **Executar como administrador**.)

## <a name="syntax"></a>Sintaxe

```
wbadmin stop job
[-quiet]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|-quiet|Executa o subcomando sem prompts para o usuário.|

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)