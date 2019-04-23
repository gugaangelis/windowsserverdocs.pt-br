---
title: nfsstat
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9db8b903d4c3681b2b3bae3424f8af83696ae2c7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853287"
---
# <a name="nfsstat"></a>nfsstat



Você pode usar **nfsstat** para exibir ou redefinir contagens de chamadas feitas para o Server for NFS.

## <a name="syntax"></a>Sintaxe

```
nfsstat [-z]
```

## <a name="description"></a>Descrição

Quando usada sem as **- z** opção, o **nfsstat** utilitário de linha de comando exibe o número de NFS V2, NFS V3 e chamadas de montagem V3 feitas no servidor, pois os contadores foram definidos como 0, quando o serviço iniciado ou quando os contadores foram redefinidos usando **nfsstat - z**.