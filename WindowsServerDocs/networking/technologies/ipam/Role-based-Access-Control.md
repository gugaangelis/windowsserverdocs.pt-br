---
title: Controle de acesso baseado em função
description: Este tópico faz parte do guia de gerenciamento de gerenciamento de endereço IP (IPAM) no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ecdfc589-fa14-4bb3-ab7e-456ebc719385
ms.author: pashort
author: shortpatti
ms.openlocfilehash: eec6d2a89b24d4847cb993bab31d86881f2cae0f
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="role-based-access-control"></a>Controle de acesso baseado em função

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico fornece informações sobre como usar o controle de acesso baseado em função em IPAM.  
  
> [!NOTE]  
> Além de neste tópico, a seguinte documentação de controle de acesso IPAM está disponível nesta seção.  
>   
> -   [Gerenciar baseado em função com o Gerenciador do servidor de controle de acesso](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Server-Manager.md)  
> -   [Gerenciar baseado em função acessar controle com o Windows PowerShell](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md)  
  
Controle de acesso baseado em função permite que você especifique privilégios de acesso em vários níveis, incluindo o servidor DNS, zona DNS e níveis de registro de recurso DNS.  
Usando o controle de acesso baseado na função, você pode especificar quem tem controle granular sobre operações para criar, editar e excluir diferentes tipos de registros de recurso DNS.  
  
Você pode configurar o controle de acesso para que os usuários são restritos às seguintes permissões.  
  
-   Os usuários podem editar apenas registros de recurso DNS específicos  
  
-   Os usuários podem editar os registros de recurso DNS de um tipo específico, como PTR ou MX  
  
-   Os usuários podem editar os registros de recurso DNS para regiões específicas  
  
## <a name="see-also"></a>Consulte também  
[Gerenciar baseado em função com o Gerenciador do servidor de controle de acesso](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Server-Manager.md)  
[Gerenciar baseado em função acessar controle com o Windows PowerShell](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md)  
[Gerenciar IPAM](Manage-IPAM.md)  
  


