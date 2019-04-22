---
ms.assetid: c32606b4-2ee2-4df3-a704-8ac6723e188f
title: DNS e AD DS
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f6d75a78119d76a0f8380967292b1d0abc720597
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813137"
---
# <a name="dns-and-ad-ds"></a>DNS e AD DS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Os serviços de domínio do Active Directory (AD DS) usa serviços de resolução de nome de sistema de nome de domínio (DNS) para permitir que os clientes localizem os controladores de domínio e dos controladores de domínio que hospedam o serviço de diretório para se comunicar entre si.  
  
AD DS permite fácil integração do namespace do Active Directory em um namespace DNS existente. Recursos, como as zonas DNS integradas ao Active Directory torna mais fácil para a implantação de DNS, eliminando a necessidade de configurar zonas secundárias e, em seguida, configurar transferências de zona.  
  
Para obter informações sobre como o DNS oferece suporte ao AD DS, consulte a seção [suporte de DNS para a referência técnica do Active Directory](https://go.microsoft.com/fwlink/?LinkID=48147).  
  
> [!NOTE]  
> Se você implementar um namespace separado no qual o nome de domínio do AD DS difere o sufixo DNS primário que os clientes usam, integração do AD DS com o DNS é mais complexa. Para obter mais informações, consulte [Disjoint Namespace](../../ad-ds/plan/../../ad-ds/plan/Disjoint-Namespace.md).  
  
## <a name="in-this-section"></a>Nesta seção  
  
- [Local do controlador de domínio](../../ad-ds/plan/Domain-Controller-Location.md)  
- [Zonas de DNS integrado ao Active Directory](../../ad-ds/plan/Active-Directory-Integrated-DNS-Zones.md)  
- [Nomenclatura de computador](../../ad-ds/plan/Computer-Naming.md)  
- [Namespace não contíguo](../../ad-ds/plan/../../ad-ds/plan/Disjoint-Namespace.md)  
