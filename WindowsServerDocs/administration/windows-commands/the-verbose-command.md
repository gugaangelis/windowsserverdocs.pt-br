---
title: verbose
description: Tópico de referência para detalhado, que exibe a saída detalhada para um comando especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fcf30ae7-818f-4e7e-a083-a1812682032b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3563673d1f80167e469d98a664a6f96ca49815a1
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721368"
---
# <a name="verbose"></a>verbose

Exibe a saída detalhada para um comando especificado. Você pode usar **/Verbose** com quaisquer outros comandos do WDSUTIL que você executar. Observe que você deve especificar **/Verbose** e **/Progress** diretamente após **WDSUTIL**.

## <a name="syntax"></a>Sintaxe

```
WDSUTIL /verbose <commands>
```

## <a name="examples"></a>Exemplos

Para excluir computadores aprovados da adição automática de banco de dados e mostrar saída detalhada, digite:
```
WDSUTIL /Verbose /progress /Delete-AutoAddDevices /Server:MyWDSServer /DeviceType:ApprovedDevices
```