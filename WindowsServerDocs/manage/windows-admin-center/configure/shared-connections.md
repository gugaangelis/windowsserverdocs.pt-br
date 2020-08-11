---
title: Configurar conexões compartilhadas para todos os usuários do gateway do Windows Admin Center
description: Saiba como os administradores podem configurar o gateway do Windows Admin Center (Project Honolulu) uma vez para permitir que todos os usuários compartilhem uma lista única de conexões.
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/28/2019
ms.localizationpriority: medium
ms.openlocfilehash: eea0e467629a6110d38ed3081a1320e002496cfb
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87970873"
---
# <a name="configure-shared-connections-for-all-users-of-the-windows-admin-center-gateway"></a>Configurar conexões compartilhadas para todos os usuários do gateway do Windows Admin Center

> Aplica-se a: Versão prévia do Windows Admin Center, Windows Admin Center

Com a capacidade de configurar conexões compartilhadas, os administradores de gateway podem configurar a lista de conexões uma vez para todos os usuários de um determinado gateway do Windows Admin Center. Esse recurso só está disponível no modo de serviço do Windows Admin Center.

Na guia **Conexões compartilhadas** das configurações de gateway do Windows Admin Center, os administradores de gateway podem adicionar servidores, clusters e conexões do PC como você faria na página Todas as conexões, incluindo a capacidade de marcar conexões. Conexões e marcas adicionadas na lista Conexões compartilhadas serão exibidas para todos os usuários desse gateway do Windows Admin Center na página Todas as conexões deles.

![Windows Admin Center – Página Conexões Compartilhadas](../media/shared-cnxns-1.png)

Quando o usuário do Windows Admin Center acessar a página "Todas as conexões" depois que as Conexões compartilhadas tiverem sido configuradas, ele verá as conexões agrupadas em duas ações: Pessoal e Conexões compartilhadas. O grupo Pessoal é uma lista de conexão de um usuário específico e persiste em todas as sessões de navegador desse usuário. O grupo Conexões compartilhadas é o mesmo em todos os usuários e não pode ser modificado na página Todas as conexões.

![Windows Admin Center – Página Todas as Conexões](../media/shared-cnxns-2.png)