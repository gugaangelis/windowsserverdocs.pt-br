---
title: nfsstat
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: da7a9768-44bd-404b-97ee-c388d00dc395
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4e2c02fdfeb9923993a1d4471862a6c8487c9d86
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723749"
---
# <a name="nfsstat"></a>nfsstat



Você pode usar **nfsstat** para exibir ou redefinir contagens de chamadas feitas ao servidor para NFS.

## <a name="syntax"></a>Sintaxe

```
nfsstat [-z]
```

## <a name="description"></a>Descrição

Quando usado sem a opção **-z** , o utilitário de linha de comando **nfsstat** exibe o número de chamadas NFS v2, NFS v3 e Mount v3 feitas para o servidor desde que os contadores foram definidos como 0, seja quando o serviço foi iniciado ou quando os contadores foram redefinidos usando **nfsstat-z**.