---
title: Alterar a porta de escuta na área de trabalho remota
description: Saiba como alterar a porta de escuta para o cliente de área de trabalho remota.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 07/19/2018
ms.localizationpriority: medium
ms.openlocfilehash: b70479b644f4984c93136d6493483c372703244d
ms.sourcegitcommit: d3f73936160505a40633ad8dd5931ac5fe3eccdb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297311"
---
# Alterar a porta de escuta para a área de trabalho remota no computador

>Aplica-se a: Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2008 R2

Quando você se conecta a um computador (um cliente Windows ou o Windows Server) por meio do cliente de área de trabalho remota, o recurso de área de trabalho remota no seu computador "ouve" a solicitação de conexão por meio de uma porta de escuta definida (3389 por padrão). Você pode alterar essa porta de escuta em computadores Windows modificando o registro.

1. Inicie o editor do registro. (Digite regedit na caixa de pesquisa).
2. Navegue até a subchave de registro: HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp\PortNumber
3. Clique em **Editar gt _ modificar**e, em seguida, clique em **Decimal**.
4. Digite o novo número de porta e, em seguida, clique em **Okey**. 
5. Feche o editor do registro e reinicie o computador.

Na próxima vez que você se conectar a este computador usando a conexão de área de trabalho remota, você deve digitar a nova porta. Se você estiver usando um firewall, certifique-se de configurar seu firewall para permitir conexões para o novo número de porta.
