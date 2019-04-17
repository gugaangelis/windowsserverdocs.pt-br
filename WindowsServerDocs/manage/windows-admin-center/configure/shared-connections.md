---
title: Configurar conexões compartilhadas para todos os usuários do gateway do Windows Admin Center
description: Saiba como os administradores podem configurar o gateway do Windows Admin Center (Project Honolulu) uma vez para permitir que todos os usuários compartilham uma única lista de conexões.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/28/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 1bdafa0d23934666fc0f6985249502e4960dc38e
ms.sourcegitcommit: 74d9eee13386a039186a70cdc97dc0b82e74bbf4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/29/2019
ms.locfileid: "9270995"
---
# Configurar conexões compartilhadas para todos os usuários do gateway do Windows Admin Center

> Aplicável a: Windows Admin Center Preview

Com a capacidade de configurar conexões compartilhadas, os administradores de gateway podem configurar a lista de conexões uma vez para todos os usuários de um determinado gateway do Windows Admin Center. 

A partir da guia **Conexões compartilhados** de gateway do Windows Admin Center configurações, os administradores de gateway podem adicionar servidores, clusters e conexões de computador como você faria na página de todas as conexões, incluindo a capacidade de conexões de marca. Quaisquer conexões e marcas adicionadas à lista de conexões compartilhadas serão exibida para todos os usuários do gateway Windows Admin Center, de todas as suas páginas de conexões.
    ![](../media/shared-cnxns-1.png)

Quando qualquer usuário do Windows Admin Center acessa a página "Todas as conexões" após compartilhado conexões foram configurados, eles verão suas conexões agrupadas em duas seções: conexões pessoal e compartilhado. O grupo pessoal é a lista de conexão de um usuário específico e persistirão entre sessões de navegador do usuário. O grupo de conexões compartilhado é o mesmo em todos os usuários e não pode ser modificado na página todas as conexões.
![](../media/shared-cnxns-2.png)