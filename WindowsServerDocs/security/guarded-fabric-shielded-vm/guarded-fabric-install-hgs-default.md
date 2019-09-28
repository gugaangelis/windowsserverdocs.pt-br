---
title: Instalar o HGS em uma nova floresta
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 6dfbe24fb4d9011b48f366d7e5df92fdb80685d5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386595"
---
# <a name="install-hgs-in-a-new-forest"></a>Instalar o HGS em uma nova floresta 

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

## <a name="add-the-hgs-server-role"></a>Adicionar a função de servidor HGS

Execute os seguintes comandos em uma sessão do PowerShell com privilégios elevados para adicionar a função de servidor HGS e instalar o HGS.

[!INCLUDE [Install the HGS server role](../../../includes/guarded-fabric-install-hgs-server-role.md)] 

## <a name="install-hgs"></a>Instalar o HGS 

[!INCLUDE [Install HGS by default](../../../includes/install-hgs-default.md)] 

## <a name="next-steps"></a>Próximas etapas

- Para as próximas etapas para configurar o atestado baseado em TPM, consulte [inicializar o cluster HgS usando o modo TPM em uma nova floresta dedicada (padrão)](guarded-fabric-initialize-hgs-tpm-mode-default.md).
- Para as próximas etapas para configurar o atestado de chave de host, consulte [inicializar o cluster HgS usando o modo de chave em uma nova floresta dedicada (padrão)](guarded-fabric-initialize-hgs-key-mode-default.md).
- Para as próximas etapas para configurar o atestado baseado em administrador (preterido no Windows Server 2019), consulte [inicializar o cluster HgS usando o modo ad em uma nova floresta dedicada (padrão)](guarded-fabric-initialize-hgs-ad-mode-default.md).

## <a name="next-step"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Iniciar o HGS](guarded-fabric-initialize-hgs.md)


