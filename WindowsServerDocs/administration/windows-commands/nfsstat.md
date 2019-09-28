---
title: nfsstat
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: da7a9768-44bd-404b-97ee-c388d00dc395
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4f9db119596b5602f18acfa10af6aa1b7cbbc9b2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373184"
---
# <a name="nfsstat"></a>nfsstat



Você pode usar **nfsstat** para exibir ou redefinir contagens de chamadas feitas ao servidor para NFS.

## <a name="syntax"></a>Sintaxe

```
nfsstat [-z]
```

## <a name="description"></a>Descrição

Quando usado sem a opção **-z** , o utilitário de linha de comando **nfsstat** exibe o número de chamadas de NFS v2, NFS v3 e Mount v3 feitas ao servidor desde que os contadores foram definidos como 0, seja quando o serviço foi iniciado ou quando os contadores foram redefinidos usando  **nfsstat-z**.