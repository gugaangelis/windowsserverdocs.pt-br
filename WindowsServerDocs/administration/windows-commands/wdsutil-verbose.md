---
title: Usando o comando Verbose
description: Artigo de referência para detalhado, que exibe a saída detalhada para um comando especificado.
ms.topic: reference
ms.assetid: fcf30ae7-818f-4e7e-a083-a1812682032b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2335ef8bf3e3b231851d424f99f0a7e878218c4b
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524871"
---
# <a name="using-the-verbose-command"></a>Usando o comando Verbose

Exibe a saída detalhada para um comando especificado. Você pode usar **/Verbose** com quaisquer outros comandos do WDSUTIL que você executar. Observe que você deve especificar **/Verbose** e **/Progress** diretamente após **WDSUTIL**.

## <a name="syntax"></a>Sintaxe

```
wdsutil /verbose <commands>
```

## <a name="examples"></a>Exemplos

Para excluir computadores aprovados da adição automática de banco de dados e mostrar saída detalhada, digite:

```
wdsutil /Verbose /progress /Delete-AutoAddDevices /Server:MyWDSServer /DeviceType:ApprovedDevices
```