---
title: Verificar a configuração do HGS
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 8f01df37-f18e-4386-ae73-ecf84feaa9df
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: d219b7aa9ca1e17df3281fd756106a6f07864116
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386353"
---
# <a name="verify-the-hgs-configuration"></a>Verificar a configuração do HGS

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016


Em seguida, precisamos validar que as coisas estão funcionando conforme o esperado. Para fazer isso, execute o seguinte comando em um console do Windows PowerShell elevado:

```powershell
Get-HgsTrace -RunDiagnostics
```

Como a configuração do HGS ainda não contém informações sobre os hosts que estarão na malha protegida, o diagnóstico indicará que nenhum host será capaz de atestar com êxito ainda. Ignore esse resultado e examine as outras informações fornecidas pelo diagnóstico.

[!INCLUDE [Guarded fabric diagnostics tool](../../../includes/guarded-fabric-diagnostics-tool.md)] 

Execute o diagnóstico em cada nó em seu cluster HGS.

## <a name="next-step"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Implantar hosts protegidos](guarded-fabric-configure-hgs-with-authorized-hyper-v-hosts.md)

