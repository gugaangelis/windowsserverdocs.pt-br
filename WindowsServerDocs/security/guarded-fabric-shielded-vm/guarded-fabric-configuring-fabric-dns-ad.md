---
title: Configurar o DNS de malha para hosts protegidos (AD)
ms.topic: article
ms.assetid: 074b6d09-f16e-49bf-b88a-377139d35067
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 1875477934dad739a7231fd08f72cef7193e0eee
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87966143"
---
# <a name="configure-the-fabric-dns-for-guarded-hosts-ad"></a>Configurar o DNS de malha para hosts protegidos (AD)

>Aplica-se a: Windows Server 2016


>[!IMPORTANT]
>O modo AD é preterido a partir do Windows Server 2019. Para ambientes em que o atestado do TPM não é possível, configure o [atestado de chave do host](guarded-fabric-initialize-hgs-key-mode.md). O atestado de chave de host fornece garantia semelhante ao modo AD e é mais simples de configurar.

Um administrador de malha precisa configurar a malha que o DNS leva para permitir que hosts protegidos resolvam o cluster HGS.
O cluster HGS já deve estar [configurado pelo administrador do HgS](/WindowsServerDocs/virtualization/guarded-fabric-shielded-vm/guarded-fabric-setting-up-the-host-guardian-service-hgs.md).



[!INCLUDE [Configure fabric DNS](../../../includes/guarded-fabric-configure-fabric-dns.md)]


## <a name="next-step"></a>Próxima etapa

> [!div class="nextstepaction"]
> [Configurar o DNS do HGS e uma relação de confiança unidirecional](guarded-fabric-configure-dns-forwarding-and-trust.md)
