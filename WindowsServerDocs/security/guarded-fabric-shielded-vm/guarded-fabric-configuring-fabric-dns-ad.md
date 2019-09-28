---
title: Configurar o DNS de malha para hosts protegidos (AD)
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 074b6d09-f16e-49bf-b88a-377139d35067
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 411b845d57c36916dcbc73d51675f5d9f92bfa0e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386756"
---
# <a name="configure-the-fabric-dns-for-guarded-hosts"></a>Configurar o DNS de malha para hosts protegidos

>Aplica-se a: Windows Server 2016


>[!IMPORTANT]
>O modo AD é preterido a partir do Windows Server 2019. Para ambientes em que o atestado do TPM não é possível, configure o [atestado de chave do host](guarded-fabric-initialize-hgs-key-mode.md). O atestado de chave de host fornece garantia semelhante ao modo AD e é mais simples de configurar. 

Um administrador de malha precisa configurar a malha que o DNS leva para permitir que hosts protegidos resolvam o cluster HGS. O cluster HGS já deve estar [configurado pelo administrador do HgS](/WindowsServerDocs/virtualization/guarded-fabric-shielded-vm/guarded-fabric-setting-up-the-host-guardian-service-hgs.md).



[!INCLUDE [Configure fabric DNS](../../../includes/guarded-fabric-configure-fabric-dns.md)] 


## <a name="next-step"></a>Próximas etapas

> [!div class="nextstepaction"]
> [Configurar o DNS do HGS e uma relação de confiança unidirecional](guarded-fabric-configure-dns-forwarding-and-trust.md)
