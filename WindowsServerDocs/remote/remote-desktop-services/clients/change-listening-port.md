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
ms.openlocfilehash: 5bf90010143e742f7a0c9b5c262be01e6ccf8c5c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59882717"
---
# <a name="change-the-listening-port-for-remote-desktop-on-your-computer"></a>Alterar a porta de escuta de área de trabalho remota em seu computador

>Aplica-se a: Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2, Windows Server 2008 R2

Quando você se conecta a um computador (um cliente do Windows ou o Windows Server) por meio do cliente de área de trabalho remota, o recurso de área de trabalho remota no computador "ouve" a solicitação de conexão por meio de uma porta de escuta definida (3389 por padrão). Você pode alterar essa porta de escuta em computadores Windows ao modificar o registro.

1. Inicie o editor do registro. (Digite regedit na caixa de pesquisa).
2. Navegue até a seguinte subchave do registro: HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp\PortNumber
3. Clique em **Editar > Modify**e, em seguida, clique em **Decimal**.
4. Digite o novo número de porta e, em seguida, clique em **Okey**. 
5. Feche o editor do registro e reinicie o computador.

Na próxima vez que você se conectar a este computador usando a conexão de área de trabalho remota, você deve digitar a nova porta. Se você estiver usando um firewall, certifique-se de configurar o firewall para permitir conexões para o novo número de porta.
