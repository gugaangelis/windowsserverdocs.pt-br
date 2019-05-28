---
ms.assetid: 9cafa3e1-8118-4a75-a7c2-1dbe40b1a444
title: Configurar regras de declarações
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d0dd8528e5fbd6829b313a3e6bc47f5a17f6a12f
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189717"
---
# <a name="configure-claim-rules"></a>Configurar regras de declaração

Em uma de declarações\-modelo de identidade baseada no, a função dos serviços de Federação do Active Directory \(do AD FS\) como federação serviços é emitir um token que contém um conjunto de declarações. Regras de declarações controlam as decisões em relação a declarações de problemas do AD FS. Regras de declaração e todos os dados de configuração de servidor são armazenados no banco de dados de configuração do AD FS.  
  
O AD FS toma decisões de emissão que se baseiam em informações de identidade que são fornecidas a ele na forma de declarações e outras informações contextuais. Em um alto nível, o AD FS opera como um processador de regras por meio de um conjunto de declarações como entrada, realiza uma série de transformações e, em seguida, retorna um conjunto diferente de declarações como saída. 

Os tópicos a seguir o ajudarão a criar as regras que processará o AD FS: 
  
-   [Criar uma regra para passar ou filtrar uma declaração de entrada](Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)  
  
-   [Criar uma regra para permitir todos os usuários](Create-a-Rule-to-Permit-All-Users.md)  
  
-   [Criar uma regra para permitir ou negar usuários com base em uma declaração de entrada](Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)  
  
-   [Criar uma regra para enviar atributos LDAP como declarações](Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)  
  
-   [Criar uma regra para enviar associação a um grupo como uma declaração](Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)  
  
-   [Criar uma regra para transformar uma declaração de entrada](Create-a-Rule-to-Transform-an-Incoming-Claim.md)  
  
-   [Criar uma regra para enviar uma declaração do método de autenticação](Create-a-Rule-to-Send-an-Authentication-Method-Claim.md) 
-   [Criar uma regra para enviar uma declaração compatível do AD FS 1.x](Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md) 
  
-   [Criar uma regra para enviar declarações usando uma regra personalizada](Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule.md)  

## <a name="see-also"></a>Consulte também  
[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md) 
