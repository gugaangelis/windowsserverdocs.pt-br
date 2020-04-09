---
title: nfsstat
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: da7a9768-44bd-404b-97ee-c388d00dc395
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94eb389fdd694c08dcd1d77325f145a81e59ea0f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838879"
---
# <a name="nfsstat"></a>nfsstat



Você pode usar **nfsstat** para exibir ou redefinir contagens de chamadas feitas ao servidor para NFS.

## <a name="syntax"></a>Sintaxe

```
nfsstat [-z]
```

## <a name="description"></a>Descrição

Quando usado sem a opção **-z** , o utilitário de linha de comando **nfsstat** exibe o número de chamadas NFS v2, NFS v3 e Mount v3 feitas para o servidor desde que os contadores foram definidos como 0, seja quando o serviço foi iniciado ou quando os contadores foram redefinidos usando **nfsstat-z**.