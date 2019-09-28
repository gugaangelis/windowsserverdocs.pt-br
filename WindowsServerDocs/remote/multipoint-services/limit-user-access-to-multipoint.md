---
title: Limitar o acesso do usuário ao servidor
description: Saiba como conceder ou negar acesso a serviços do MultiPoint para usuários e grupos
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4cabd4f1-a764-4be6-bc6e-0a5f5566390c
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 62f2a3f9b94ac3f0474636c34e8ec1f81c568cad
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389062"
---
# <a name="limit-users-access-to-the-multipoint-server"></a>Limitar o acesso dos usuários ao MultiPoint Server
Se você ingressar o MultiPoint Server em um Active Directory domínio ou usar contas de usuário local, todos os usuários terão acesso ao MultiPoint Server por padrão. Antes de permitir que os usuários façam logon nas estações em seu ambiente de serviços do MultiPoint, você deve restringir o acesso ao servidor.  
  
Qualquer usuário no grupo Área de Trabalho Remota usuários pode fazer logon no MultiPoint Server. Por padrão, o grupo de usuários todos é membro do grupo Área de Trabalho Remota usuários e, portanto, cada usuário local e usuário de domínio pode fazer logon no MultiPoint Server. Para restringir o acesso ao MultiPoint Server, remova o grupo de usuários Everyone do grupo Área de Trabalho Remota Users e, em seguida, adicione usuários ou grupos específicos ao grupo Área de Trabalho Remota usuários.  
  
## <a name="add-or-remove-users-or-groups-to-the-remote-desktop-users-group"></a>Adicionar ou remover usuários ou grupos no grupo Área de Trabalho Remota usuários  
  
1.  Na tela **Iniciar** , abra **Gerenciamento do computador**.  
  
2.  Na árvore de console, em **usuários e grupos locais**, clique em **grupos**.  
  
3.  Clique duas vezes **área de trabalho remota usuários**e siga as instruções para adicionar ou remover usuários.  
  
    -   Para restringir o acesso geral ao servidor, remova o grupo todos.  
  
    -   Para dar acesso às estações aos usuários do MultiPoint Server, adicione cada conta local ou cada usuário de domínio ou conta de grupo ao grupo Área de Trabalho Remota usuários.  