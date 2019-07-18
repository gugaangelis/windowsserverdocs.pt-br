---
title: Configurar conexões compartilhadas para todos os usuários do gateway do centro de administração do Windows
description: Saiba como os administradores podem configurar seu gateway do centro de administração do Windows (Project Honolulu) uma vez para permitir que todos os usuários compartilhem uma única lista de conexões.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/28/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 46075154a225590376458881768ae86f74a572ad
ms.sourcegitcommit: 286e3181ebd2cb9d7dc7fe651858a4e0d61d153f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/17/2019
ms.locfileid: "68300723"
---
# <a name="configure-shared-connections-for-all-users-of-the-windows-admin-center-gateway"></a>Configurar conexões compartilhadas para todos os usuários do gateway do centro de administração do Windows

> Aplica-se a: Versão prévia do centro de administração do Windows, centro de administração do Windows

Com a capacidade de configurar conexões compartilhadas, os administradores de gateway podem configurar a lista de conexões uma vez para todos os usuários de um determinado gateway do centro de administração do Windows. 

Na guia **conexões** compartilhadas das configurações do gateway do centro de administração do Windows, os administradores de gateway podem adicionar servidores, clusters e conexões de PC como você faria na página todas as conexões, incluindo a capacidade de marcar conexões. Todas as conexões e marcas adicionadas na lista de conexões compartilhadas serão exibidas para todos os usuários desse gateway do centro de administração do Windows, na página todas as conexões.
    ![](../media/shared-cnxns-1.png)

Quando qualquer usuário do centro de administração do Windows acessar a página "todas as conexões" depois que as conexões compartilhadas tiverem sido configuradas, elas verão suas conexões agrupadas em duas seções: Conexões pessoais e compartilhadas. O grupo pessoal é uma lista de conexão de um usuário específico e persiste entre as sessões do navegador desse usuário. O grupo de conexões compartilhadas é o mesmo em todos os usuários e não pode ser modificado na página todas as conexões.
![](../media/shared-cnxns-2.png)