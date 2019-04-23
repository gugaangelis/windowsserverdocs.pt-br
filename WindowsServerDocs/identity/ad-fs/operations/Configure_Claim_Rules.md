---
ms.assetid: 20d48afc-2623-43e9-8ed9-aeb9a0505630
title: Configurar regras de declarações
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 259e2b266b64a3b34c237cfe209a3558124c8ef2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871627"
---
# <a name="configure-claim-rules"></a>Configurar regras de declaração

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Em um declarações\-modelo de identidade baseada no, a função dos serviços de Federação do Active Directory (AD FS) como os serviços de Federação é emitir um token que contém um conjunto de declarações. Regras de declarações controlam as decisões em relação a declarações de problemas do AD FS. Regras de declaração e todos os dados de configuração de servidor são armazenados no banco de dados de configuração do AD FS.  
  
O AD FS toma decisões de emissão que se baseiam em informações de identidade que são fornecidas a ele na forma de declarações e outras informações contextuais. Em um alto nível, o AD FS opera como um processador de regras por meio de um conjunto de declarações como entrada, realiza uma série de transformações e, em seguida, retorna um conjunto diferente de declarações como saída. 

Os tópicos a seguir o ajudarão a criar as regras que processará o AD FS: 
  
-   [Criar uma regra para passar ou filtrar uma declaração de entrada](../../ad-fs/operations/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)  
  
-   [Criar uma regra para permitir todos os usuários](../../ad-fs/operations/Create-a-Rule-to-Permit-All-Users.md)  
  
-   [Criar uma regra para permitir ou negar usuários com base em uma declaração de entrada](../../ad-fs/operations/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)  
  
-   [Criar uma regra para enviar atributos LDAP como declarações](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)  
  
-   [Criar uma regra de associação do grupo de envio como uma declaração](../../ad-fs/operations/Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)  
  
-   [Criar uma regra para transformar uma declaração de entrada](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)  
  
-   [Criar uma regra para enviar uma declaração de método de autenticação](../../ad-fs/operations/Create-a-Rule-to-Send-an-Authentication-Method-Claim.md)  
  
-   [Criar uma regra para enviar declarações usando uma regra personalizada](../../ad-fs/operations/Create-a-Rule-to-Send-Claims-Using-a-Custom-rule.md)  

## <a name="see-also"></a>Consulte também  
[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
