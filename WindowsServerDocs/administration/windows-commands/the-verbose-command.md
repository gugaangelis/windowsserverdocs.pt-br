---
title: modo detalhado
description: O tópico comandos do Windows para detalhado, que exibe a saída detalhada para um comando especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fcf30ae7-818f-4e7e-a083-a1812682032b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f67a4fe99a5051e8844e6386d87911946a808c1e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832839"
---
# <a name="verbose"></a>modo detalhado

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