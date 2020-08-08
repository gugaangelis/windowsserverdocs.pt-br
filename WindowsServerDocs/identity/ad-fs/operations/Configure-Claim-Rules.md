---
ms.assetid: 9cafa3e1-8118-4a75-a7c2-1dbe40b1a444
title: Configurar regras de declarações
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 7a85044010c95f64fe1167d7bf186f265fe4beb9
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87964763"
---
# <a name="configure-claim-rules-in-ad-fs-for-windows-server"></a>Configurar regras de declaração no AD FS para Windows Server

Em um \- modelo de identidade baseado em declarações, a função de Serviços de Federação do Active Directory (AD FS) \( AD FS \) como serviços de Federação é emitir um token que contenha um conjunto de declarações. As regras de declarações regem as decisões relacionadas a declarações que AD FS problemas. As regras de declaração e todos os dados de configuração do servidor são armazenados no banco de dado de configuração do AD FS.

AD FS toma decisões de emissão baseadas em informações de identidade fornecidas a ela na forma de declarações e outras informações contextuais. Em um alto nível, AD FS Opera como um processador de regras, adotando um conjunto de declarações como entrada, realiza um número de transformações e, em seguida, retorna um conjunto diferente de declarações como saída.

Os tópicos a seguir o ajudarão a criar as regras que AD FS processará:

-   [Criar uma regra para passar ou filtrar uma declaração de entrada](Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)

-   [Criar uma regra para permitir todos os usuários](Create-a-Rule-to-Permit-All-Users.md)

-   [Criar uma regra para permitir ou negar usuários com base em uma declaração de entrada](Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)

-   [Criar uma regra para enviar atributos LDAP como declarações](Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)

-   [Criar uma regra para enviar associação a um grupo como uma declaração](Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)

-   [Criar uma regra para transformar uma declaração de entrada](Create-a-Rule-to-Transform-an-Incoming-Claim.md)

-   [Criar uma regra para enviar uma declaração do método de autenticação](Create-a-Rule-to-Send-an-Authentication-Method-Claim.md)
-   [Criar uma regra para enviar uma declaração compatível com o AD FS 1. x](Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)

-   [Criar uma regra para enviar declarações usando uma regra personalizada](Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule.md)

## <a name="see-also"></a>Consulte Também
[Operações do AD FS](../ad-fs-operations.md)
