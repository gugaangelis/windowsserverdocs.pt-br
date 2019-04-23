---
title: Interrupção do trabalho de WBADMIN
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3b83b398-39c7-4410-bf17-5c1fb1a4f46d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7a9e71fe2e4883c52c2418e21fc8764fd14e6c81
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889717"
---
# <a name="wbadmin-stop-job"></a>Interrupção do trabalho de WBADMIN



Cancela a operação de backup ou recuperação em execução no momento. Operações canceladas não podem ser reiniciadas, você deve executar novamente uma operação de backup ou recuperação cancelada desde o início.

Para interromper uma operação de backup ou recuperação com este subcomando, você deve ser um membro do **operadores de Backup** grupo ou o **administradores** grupo, ou você deve ter sido recebido a autoridade apropriada. Além disso, você deve executar **wbadmin** em um prompt de comando elevado. (Para abrir um atalho de prompt de comando com privilégios elevados **Prompt de comando** e, em seguida, clique em **executar como administrador**.)

## <a name="syntax"></a>Sintaxe

```
wbadmin stop job
[-quiet]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|-quiet|Executa o subcomando sem prompts para o usuário.|

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)