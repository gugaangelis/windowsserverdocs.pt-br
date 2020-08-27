---
ms.assetid: c32606b4-2ee2-4df3-a704-8ac6723e188f
title: DNS e AD DS
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.date: 08/08/2018
ms.topic: article
ms.openlocfilehash: 65e23e469c6361ac2eb614280b3f1e58e2bfff08
ms.sourcegitcommit: 1dc35d221eff7f079d9209d92f14fb630f955bca
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/26/2020
ms.locfileid: "88941106"
---
# <a name="dns-and-ad-ds"></a>DNS e AD DS

> Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Active Directory Domain Services (AD DS) usa os serviços de resolução de nomes DNS (sistema de nomes de domínio) para possibilitar que os clientes localizem controladores de domínio e os controladores de domínio que hospedam o serviço de diretório para se comunicarem entre si.

AD DS permite uma fácil integração do namespace de Active Directory em um namespace DNS existente. Recursos como zonas DNS integradas ao Active Directory facilitam a implantação do DNS, eliminando a necessidade de configurar zonas secundárias e, em seguida, configurar transferências de zona.

Para obter informações sobre como o DNS dá suporte a AD DS, consulte a seção [suporte a DNS para Active Directory referência técnica](/previous-versions/windows/it-pro/windows-server-2003/cc781627(v=ws.10)).

> [!NOTE]
> Se você implementar um namespace não contíguo no qual o nome de domínio AD DS difere do sufixo DNS primário que os clientes usam, a integração AD DS com o DNS será mais complexa. Para obter mais informações, consulte [namespace não contíguo](Disjoint-Namespace.md).

## <a name="in-this-section"></a>Nesta seção

- [Localização do controlador de domínio](Domain-Controller-Location.md)
- [Zonas DNS integradas ao Active Directory](Active-Directory-Integrated-DNS-Zones.md)
- [Nomenclatura de computador](Computer-Naming.md)
- [Namespace não contíguo](Disjoint-Namespace.md)
