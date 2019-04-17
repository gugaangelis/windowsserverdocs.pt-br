---
ms.assetid: af76ddbe-83a2-4a62-9989-873e3bb1c772
title: "Função de proprietário do site topologia"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7c98d313cac28ab07380791a384a87bffacfbda2
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="site-topology-owner-role"></a>Função de proprietário do site topologia

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O administrador que gerencia a topologia de site é conhecido como o proprietário de topologia do site. O proprietário do site topologia entenda as condições da rede entre sites e tem autoridade para alterar as configurações nos serviços de domínio do Active Directory (AD DS) para implementar mudanças na topologia de site. As alterações a topologia de site afetam as alterações na topologia de replicação. As responsabilidades do proprietário do site topologia incluem:  
  
-   Controlar alterações para a topologia de site se mudar de conectividade de rede.  
  
-   Obtendo e manter informações sobre conexões de rede e roteadores a partir do grupo de rede. O proprietário do site topologia deve manter uma lista de endereços de sub-rede, máscaras de sub-rede e o local ao qual cada pertence. O proprietário de topologia de site também deve entender os problemas sobre a velocidade de rede e a capacidade que afetam a topologia de site para definir efetivamente os custos de links de sites.  
  
-   Movendo objetos de servidor do Active Directory que representam os controladores de domínio entre sites se o endereço IP de um controlador de domínio muda para uma sub-rede diferente em um site diferente, ou se a própria sub-rede é atribuída a um site diferente. Em ambos os casos, o proprietário do site topologia deverá mover manualmente o objeto de servidor do Active Directory do controlador de domínio para o novo site.  
  


