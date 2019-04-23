---
title: Configurar a malha de DNS para hosts protegidos (AD)
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 074b6d09-f16e-49bf-b88a-377139d35067
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 8a2ddc0f2ab495b2500d4bfe48f3ee83c333c769
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842747"
---
# <a name="configure-the-fabric-dns-for-guarded-hosts"></a>Configurar a malha de DNS para hosts protegidos

>Aplica-se a: Windows Server 2016


>[!IMPORTANT]
>Modo AD é substituído, começando com o Windows Server 2019. Para ambientes em que o atestado de TPM não for possível, configure [atestado de chaves de host](guarded-fabric-initialize-hgs-key-mode.md). Atestado de chaves do host fornece garantia semelhante para o modo do AD e é mais simples de configurar. 

Um administrador de malha precisa configurar a malha de que DNS leva para permitir que os hosts protegidos resolver o cluster HGS. O cluster HGS já deve estar [configurado pelo administrador do HGS](/WindowsServerDocs/virtualization/guarded-fabric-shielded-vm/guarded-fabric-setting-up-the-host-guardian-service-hgs.md).



[!INCLUDE [Configure fabric DNS](../../../includes/guarded-fabric-configure-fabric-dns.md)] 


## <a name="next-step"></a>Próximas etapas

>[!div class="nextstepaction"]
[Configurar o DNS de HGS e uma relação de confiança unidirecional](guarded-fabric-configure-dns-forwarding-and-trust.md)
