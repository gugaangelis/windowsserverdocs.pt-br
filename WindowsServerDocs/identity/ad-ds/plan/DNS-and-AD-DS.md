---
ms.assetid: c32606b4-2ee2-4df3-a704-8ac6723e188f
title: DNS e AD DS
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 817c6ca168e33df1a0c59e151a303c4259313743
ms.sourcegitcommit: 11421f4005f9f3a3f6c0db95b1836d0f765a9fa3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2020
ms.locfileid: "81624294"
---
# <a name="dns-and-ad-ds"></a>DNS e AD DS

> Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Active Directory Domain Services (AD DS) usa os serviços de resolução de nomes DNS (sistema de nomes de domínio) para possibilitar que os clientes localizem controladores de domínio e os controladores de domínio que hospedam o serviço de diretório para se comunicarem entre si.

AD DS permite uma fácil integração do namespace de Active Directory em um namespace DNS existente. Recursos como zonas DNS integradas ao Active Directory facilitam a implantação do DNS, eliminando a necessidade de configurar zonas secundárias e, em seguida, configurar transferências de zona.

Para obter informações sobre como o DNS dá suporte a AD DS, consulte a seção [suporte a DNS para Active Directory referência técnica](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc781627(v=ws.10)).

> [!NOTE]
> Se você implementar um namespace não contíguo no qual o nome de domínio AD DS difere do sufixo DNS primário que os clientes usam, a integração AD DS com o DNS será mais complexa. Para obter mais informações, consulte [namespace não contíguo](Disjoint-Namespace.md).

## <a name="in-this-section"></a>Nesta seção

- [Localização do controlador de domínio](Domain-Controller-Location.md)
- [Zonas DNS integradas ao Active Directory](Active-Directory-Integrated-DNS-Zones.md)
- [Nomenclatura de computador](Computer-Naming.md)
- [Namespace não contíguo](Disjoint-Namespace.md)
