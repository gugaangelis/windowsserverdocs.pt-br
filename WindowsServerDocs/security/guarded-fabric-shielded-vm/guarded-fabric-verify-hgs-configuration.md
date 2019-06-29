---
title: Verificar a configuração do HGS
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 8f01df37-f18e-4386-ae73-ecf84feaa9df
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 8098edd1eea475cea1face5541459b262364a07b
ms.sourcegitcommit: 63926404009f9e1330a4a0aa8cb9821a2dd7187e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67469553"
---
# <a name="verify-the-hgs-configuration"></a>Verificar a configuração do HGS

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016


Em seguida, é necessário validar que as coisas estão funcionando conforme o esperado. Para fazer isso, execute o seguinte comando em um console do Windows PowerShell com privilégios elevados:

```powershell
Get-HgsTrace -RunDiagnostics
```

Porque a configuração de HGS ainda não contém informações sobre os hosts que estarão na malha protegida, o diagnóstico indicará que não há hosts poderão atestar ainda com êxito. Ignore esse resultado e examine as outras informações fornecidas pelo diagnóstico.

[!INCLUDE [Guarded fabric diagnostics tool](../../../includes/guarded-fabric-diagnostics-tool.md)] 

Execute o diagnóstico em cada nó no cluster do HGS.

## <a name="next-step"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Implantar hosts protegidos](guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md)

