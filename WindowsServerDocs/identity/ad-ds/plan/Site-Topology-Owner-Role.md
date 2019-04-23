---
ms.assetid: af76ddbe-83a2-4a62-9989-873e3bb1c772
title: Função de proprietário da topologia do site
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 69443c2fc1af855c7df002e0ac91d43986eff6da
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832107"
---
# <a name="site-topology-owner-role"></a>Função de proprietário da topologia do site

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O administrador que gerencia a topologia de site é conhecido como o proprietário da topologia de site. O proprietário da topologia de site compreende as condições da rede entre sites e tem autoridade para alterar as configurações nos serviços de domínio Active Directory (AD DS) para implementar alterações na topologia de site. Alterações na topologia de site afetam as alterações na topologia de replicação. Responsabilidades do proprietário da topologia do site incluem:  
  
-   Controlando alterações na topologia de site, se a conectividade de rede for alterado.  
  
-   Obter e manter informações sobre conexões de rede e roteadores do grupo de rede. O proprietário da topologia de site deve manter uma lista de endereços de sub-rede, máscaras de sub-rede e o local para o qual cada pertence. O proprietário da topologia de site também deve entender os problemas sobre capacidade e a velocidade da rede que afetam a topologia de site para definir com eficiência os custos de links de site.  
  
-   Movendo objetos de servidor do Active Directory que representam os controladores de domínio entre sites se o endereço IP de um controlador de domínio será alterado para uma sub-rede diferente em um site diferente, ou se a sub-rede em si é atribuída a um site diferente. Em ambos os casos, o proprietário da topologia de site deverá mover manualmente o objeto de servidor do controlador de domínio do Active Directory para o novo site.  
  


