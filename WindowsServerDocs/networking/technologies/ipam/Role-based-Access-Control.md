---
title: Controle de acesso baseado em função
description: Este tópico faz parte do guia de gerenciamento do IPAM (gerenciamento de endereços IP) no Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: ecdfc589-fa14-4bb3-ab7e-456ebc719385
ms.author: lizross
author: eross-msft
ms.openlocfilehash: f79dc7ab4283de5ab1ec63861d5f543658947964
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87944668"
---
# <a name="role-based-access-control"></a>Controle de acesso baseado em função

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico fornece informações sobre como usar o controle de acesso baseado em função no IPAM.

> [!NOTE]
> Além deste tópico, a seguinte documentação de controle de acesso do IPAM está disponível nesta seção.
>
> -   [Gerenciar controle de acesso baseado em função com o Gerenciador do Servidor](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Server-Manager.md)
> -   [Gerenciar controle de acesso baseado em função com o Windows PowerShell](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md)

O controle de acesso baseado em função permite que você especifique privilégios de acesso em vários níveis, incluindo o servidor DNS, a zona DNS e os níveis de registro de recurso DNS.
Usando o controle de acesso baseado em função, você pode especificar quem tem controle granular sobre as operações para criar, editar e excluir diferentes tipos de registros de recursos de DNS.

Você pode configurar o controle de acesso para que os usuários sejam restritos às permissões a seguir.

-   Os usuários podem editar somente registros de recursos DNS específicos

-   Os usuários podem editar registros de recursos de DNS de um tipo específico, como PTR ou MX

-   Os usuários podem editar registros de recursos de DNS para zonas específicas

## <a name="see-also"></a>Consulte Também
[Gerenciar o controle de acesso baseado em função com o Gerenciador do servidor](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Server-Manager.md) 
 [Gerenciar o controle de acesso baseado em função com o Windows PowerShell](../../technologies/ipam/Manage-Role-Based-Access-Control-with-Windows-PowerShell.md) 
 [Gerenciar IPAM](Manage-IPAM.md)



