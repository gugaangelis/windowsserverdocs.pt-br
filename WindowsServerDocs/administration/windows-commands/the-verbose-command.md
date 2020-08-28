---
title: Usando o comando Verbose
description: Artigo de referência para detalhado, que exibe a saída detalhada para um comando especificado.
ms.topic: reference
ms.assetid: fcf30ae7-818f-4e7e-a083-a1812682032b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a1432656a89188755d63df974fa2732702a1a1ae
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89029984"
---
# <a name="using-the-verbose-command"></a>Usando o comando Verbose

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