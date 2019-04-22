---
ms.assetid: 46dce9d4-7293-4b1c-9710-78b04f2e347a
title: Configurando regras de declaração
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6494f584edd5f84a5987707953f79edbce15cc02
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814657"
---
# <a name="configuring-claim-rules"></a>Configurando regras de declaração

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Em uma de declarações\-modelo de identidade baseada no, a função dos serviços de Federação do Active Directory \(do AD FS\) como federação serviços é emitir um token que contém um conjunto de declarações. Regras de declarações controlam a decisão em relação de declarações que emitem o AD FS. Regras de declaração e todos os dados de configuração de servidor são armazenados no banco de dados de configuração do AD FS.  
  
O AD FS toma decisões de emissão que se baseiam em informações de identidade que são fornecidas a ele na forma de declarações e outras informações contextuais. Em um alto nível, o AD FS opera como um processador de regras por meio de um conjunto de declarações como entrada, realiza uma série de transformações e, em seguida, retorna um conjunto diferente de declarações como saída.  
  
-   [Criar uma regra para passar ou filtrar uma declaração de entrada](../../ad-fs/operations/Create-a-Rule-to-Pass-Through-or-Filter-an-Incoming-Claim.md)  
  
-   [Criar uma regra para permitir todos os usuários](../../ad-fs/operations/Create-a-Rule-to-Permit-All-Users.md)  

-   [Criar uma regra para enviar uma declaração compatível do AD FS 1.x](../../ad-fs/operations/Create-a-Rule-to-Send-an-AD-FS-1x-Compatible-Claim.md)
  
-   [Criar uma regra para permitir ou negar usuários com base em uma declaração de entrada](../../ad-fs/operations/Create-a-Rule-to-Permit-or-Deny-Users-Based-on-an-Incoming-Claim.md)  
  
-   [Criar uma regra para enviar atributos LDAP como declarações](../../ad-fs/operations/Create-a-Rule-to-Send-LDAP-Attributes-as-Claims.md)  
  
-   [Criar uma regra de associação do grupo de envio como uma declaração](../../ad-fs/operations/Create-a-Rule-to-Send-Group-Membership-as-a-Claim.md)  
  
-   [Criar uma regra para transformar uma declaração de entrada](../../ad-fs/operations/Create-a-Rule-to-Transform-an-Incoming-Claim.md)  
  
-   [Criar uma regra para enviar uma declaração de método de autenticação](../../ad-fs/operations/Create-a-Rule-to-Send-an-Authentication-Method-Claim.md)  
  
-   [Criar uma regra para enviar declarações usando uma regra personalizada](../../ad-fs/operations/Create-a-Rule-to-Send-Claims-Using-a-Custom-Rule.md)  

## <a name="additional-references"></a>Referências adicionais  

[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md)
