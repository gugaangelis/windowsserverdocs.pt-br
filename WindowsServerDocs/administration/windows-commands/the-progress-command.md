---
title: progresso
description: O tópico de comandos do Windows para progresso, que exibe o progresso enquanto um comando está em execução.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8ce5e77b-e13f-4ac3-948d-31770a6c7e25
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9957174203df8f2f5c02bf3ab8a4f3406701a8da
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833119"
---
# <a name="progress"></a>progresso

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