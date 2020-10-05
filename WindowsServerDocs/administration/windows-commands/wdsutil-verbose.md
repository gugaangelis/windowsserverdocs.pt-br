---
title: Usando o comando Verbose
description: Artigo de referência para detalhado, que exibe a saída detalhada para um comando especificado.
ms.topic: reference
ms.assetid: fcf30ae7-818f-4e7e-a083-a1812682032b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a5c05590bbbb3f1b185a64d6b0081a3d230d6b41
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729597"
---
# <a name="using-the-verbose-command"></a>Usando o comando Verbose

Exibe a saída detalhada para um comando especificado. Você pode usar **/Verbose** com quaisquer outros comandos do WDSUTIL que você executar. Observe que você deve especificar **/Verbose** e **/Progress** diretamente após **WDSUTIL**.

## <a name="syntax"></a>Syntax

```
WDSUTIL /verbose <commands>
```

## <a name="examples"></a>Exemplos

Para excluir computadores aprovados da adição automática de banco de dados e mostrar saída detalhada, digite:

```
WDSUTIL /Verbose /progress /Delete-AutoAddDevices /Server:MyWDSServer /DeviceType:ApprovedDevices
```