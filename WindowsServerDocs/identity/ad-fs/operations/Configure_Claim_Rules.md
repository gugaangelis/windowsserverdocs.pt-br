---
ms.assetid: 20d48afc-2623-43e9-8ed9-aeb9a0505630
title: Configurar regras de declarações no AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: b2c653aa7339815ce22c256cf2c6c931ca7c7fc9
ms.sourcegitcommit: de8fea497201d8f3d995e733dfec1d13a16cb8fa
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87863682"
---
# <a name="configure-claim-rules"></a>Configurar regras de declaração

Em um \- modelo de identidade baseado em declarações, a função de serviços de Federação do Active Directory (AD FS) (AD FS) como serviços de Federação é emitir um token que contenha um conjunto de declarações. As regras de declarações regem as decisões relacionadas a declarações que AD FS problemas. As regras de declaração e todos os dados de configuração do servidor são armazenados no banco de dado de configuração do AD FS.  
  
AD FS toma decisões de emissão baseadas em informações de identidade fornecidas a ela na forma de declarações e outras informações contextuais. Em um alto nível, AD FS Opera como um processador de regras, adotando um conjunto de declarações como entrada, realiza um número de transformações e, em seguida, retorna um conjunto diferente de declarações como saída. 

Os tópicos a seguir o ajudarão a criar as regras que AD FS processará: 
  
-   [Criar uma regra para passar ou filtrar uma declaração de entrada](../../ad-fs/operations/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)  
  
-   [Criar uma regra para permitir todos os usuários](../../ad-fs/operations/Create-a-Rule-to-Permit-All-Users.md)  
  
-   [Criar uma regra para permitir ou negar usuários com base em uma declaração de entrada](../../ad-fs/operations/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)  
  
-   [Criar uma regra para enviar atributos LDAP como declarações](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)  
  
-   [Criar uma regra para enviar associação a um grupo como uma declaração](../../ad-fs/operations/Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)  
  
-   [Criar uma regra para transformar uma declaração de entrada](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)  
  
-   [Criar uma regra para enviar uma declaração do método de autenticação](../../ad-fs/operations/Create-a-Rule-to-Send-an-Authentication-Method-Claim.md)  
  
-   [Criar uma regra para enviar declarações usando uma regra personalizada](../../ad-fs/operations/Create-a-Rule-to-Send-Claims-Using-a-Custom-rule.md)  

## <a name="see-also"></a>Consulte Também  
[Operações do AD FS](../ad-fs-operations.md) 
