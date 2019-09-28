---
title: Inicializar HGS
ms.custom: na
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 07c40b1da829239dda5210dde0dabe485f659ae0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403590"
---
# <a name="initialize-the-host-guardian-service-hgs"></a>Inicializar o serviço guardião de host (HGS)

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Ao inicializar o HGS, você especifica o modo que o HGS usará para medir a integridade dos hosts protegidos. Há duas opções mutuamente exclusivas. Para obter informações básicas sobre qual modo escolher, consulte [malha protegida e guia de planejamento de VM blindada para hosters](guarded-fabric-planning-for-hosters.md).

Os tópicos a seguir abrangem as etapas de implantação para cada modo:

- [TPM-atestado confiável (modo TPM)](guarded-fabric-initialize-hgs-tpm-mode.md)
- [Atestado de chave de host (modo de chave)](guarded-fabric-initialize-hgs-key-mode.md)
- [Administrador-atestado confiável (modo de AD)](guarded-fabric-initialize-hgs-ad-mode.md)

Você deve executar essas etapas em um servidor físico.