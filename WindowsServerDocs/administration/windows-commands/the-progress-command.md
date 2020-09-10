---
title: Usando o comando Progress
description: Artigo de referência para o progresso, que exibe o progresso enquanto um comando está em execução.
ms.topic: reference
ms.assetid: 8ce5e77b-e13f-4ac3-948d-31770a6c7e25
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 51f127cd9d707652e43a5cae2a2e0bfd7a573b69
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640784"
---
# <a name="using-the-progress-command"></a>Usando o comando Progress

Exibe o progresso enquanto um comando está em execução. Você pode usar o **/Progress** com outros comandos do WDSUTIL que você executar. Observe que você deve especificar **/Verbose** e **/Progress** diretamente após **WDSUTIL**.

## <a name="syntax"></a>Sintaxe

```
WDSUTIL /progress <commands>
```

## <a name="examples"></a>Exemplos

Para inicializar o servidor e exibir o progresso, digite:
```
WDSUTIL /Verbose /Progress /Initialize-Server /Server:MyWDSServer /RemInst:C:\RemoteInstall
```