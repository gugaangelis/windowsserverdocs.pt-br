---
title: Limitar o acesso de usuário para o servidor
description: Saiba como conceder ou negar acesso aos serviços do MultiPoint para usuários e grupos
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4cabd4f1-a764-4be6-bc6e-0a5f5566390c
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 1466e19152847a6c7d88f77162c50ec73a5a7d27
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830807"
---
# <a name="limit-users-access-to-the-multipoint-server"></a>Limitar o acesso dos usuários para o Multipoint server
Se você ingressar MultiPoint server a um domínio do Active Directory ou usar contas de usuário local, todos os usuários têm acesso ao MultiPoint server por padrão. Antes de permitir que os usuários façam logon em estações em seu ambiente do MultiPoint Services, você deve restringir o acesso ao servidor.  
  
Qualquer usuário no grupo de usuários da área de trabalho remota pode fazer logon MultiPoint server. Por padrão, o usuário grupo Todos é um membro do grupo usuários da área de trabalho remota e, portanto, todos os usuários locais e usuários de domínio podem fazer logon no servidor do MultiPoint. Para restringir o acesso ao MultiPoint Server, remova o todos usuário de grupo do grupo de usuários da área de trabalho remota e, em seguida, adicionar usuários ou grupos específicos ao grupo usuários da área de trabalho remota.  
  
## <a name="add-or-remove-users-or-groups-to-the-remote-desktop-users-group"></a>Adicionar ou remover usuários ou grupos ao grupo usuários da área de trabalho remota  
  
1.  Dos **inicie** tela, abra **gerenciamento de computador**.  
  
2.  Na árvore de console, sob **usuários e grupos locais**, clique em **grupos**.  
  
3.  Clique duas vezes **Remote Desktop Users**e siga as instruções para adicionar ou remover usuários.  
  
    -   Para restringir o acesso geral para o servidor, remova o grupo Todos.  
  
    -   Para dar aos usuários acesso a seu MultiPoint server em estações, adicione cada conta local ou cada conta de usuário ou grupo de domínio ao grupo de usuários da área de trabalho remota.  