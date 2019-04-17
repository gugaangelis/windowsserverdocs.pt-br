---
ms.assetid: c32606b4-2ee2-4df3-a704-8ac6723e188f
title: DNS e o AD DS
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 8e7ee494c157396a7d58e9fd1b4b80060c4d99fc
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="dns-and-ad-ds"></a>DNS e o AD DS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Os serviços de domínio do Active Directory (AD DS) usa serviços de resolução de nome do sistema de nome de domínio (DNS) para torná-lo possível para os clientes localizem controladores de domínio e para os controladores de domínio que hospedam o serviço de diretório para se comunicar uns com os outros.  
  
AD DS permite a fácil integração do namespace do Active Directory em um namespace DNS existente. Recursos como zonas DNS integradas ao Active Directory tornam mais fácil para você implantar DNS, eliminando a necessidade de configurar as zonas secundárias e, em seguida, configurar transferências de zona.  
  
Para obter informações sobre como o DNS oferece suporte ao AD DS, consulte o suporte de DNS para referência técnica do Active Directory ([https://go.microsoft.com/fwlink/?LinkID=48147](https://go.microsoft.com/fwlink/?LinkID=48147)).  
  
> [!NOTE]  
> Se você implementar um namespace separado no qual o nome de domínio do AD DS difere o sufixo DNS primário que os clientes usam, integração com o AD DS com DNS é mais complexa. Para obter mais informações, consulte [ramificações Namespace](../../ad-ds/plan/../../ad-ds/plan/Disjoint-Namespace.md).  
  
## <a name="in-this-section"></a>Nesta seção  
  
-   [Localização de controlador de domínio](../../ad-ds/plan/Domain-Controller-Location.md)  
  
-   [Active Directory integrado zonas de DNS](../../ad-ds/plan/Active-Directory-Integrated-DNS-Zones.md)  
  
-   [Nomenclatura do computador](../../ad-ds/plan/Computer-Naming.md)  
  
-   [Namespace separado](../../ad-ds/plan/../../ad-ds/plan/Disjoint-Namespace.md)  
  


