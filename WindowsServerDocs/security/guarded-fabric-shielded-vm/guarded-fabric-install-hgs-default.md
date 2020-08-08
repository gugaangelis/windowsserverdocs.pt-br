---
title: Instalar o HGS em uma nova floresta
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: c18c14435c93d08c98b1a765fd820294a273a575
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87939540"
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

## <a name="next-step"></a>Próxima etapa

> [!div class="nextstepaction"]
> [Iniciar o HGS](guarded-fabric-initialize-hgs.md)


