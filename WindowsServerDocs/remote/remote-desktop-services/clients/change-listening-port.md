---
title: Alterar a porta de escuta na Área de Trabalho Remota
description: Saiba como alterar a porta de escuta do cliente de Área de Trabalho Remota.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 07/19/2018
ms.localizationpriority: medium
ms.openlocfilehash: 818ae5217d0144b2a4ec6e45f3a7757455cfabf1
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80854659"
---
# <a name="change-the-listening-port-for-remote-desktop-on-your-computer"></a>Alterar a porta de escuta da Área de Trabalho Remota em seu computador

>Aplica-se a: Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2008 R2

Quando você conecta a um computador (um cliente do Windows ou o Windows Server) por meio do cliente de Área de Trabalho Remota, o recurso de Área de Trabalho Remota no computador "ouve" a solicitação de conexão por uma porta de escuta definida (3389 por padrão). É possível alterar essa porta de escuta em computadores Windows modificando o registro.

1. Inicie o Editor do Registro. (Digite regedit na caixa de pesquisa.)
2. Navegue até a seguinte subchave do registro: HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp\PortNumber
3. Clique em **Editar > Modificar** e, em seguida, clique em **Decimal**.
4. Digite o número da porta e, em seguida, clique em **OK**. 
5. Feche o editor do registro e reinicie o computador.

Da próxima vez que conectar a este computador usando a conexão de Área de Trabalho Remota, você deve digitar a nova porta. Se você estiver usando um firewall, certifique-se de configurar o firewall para permitir conexões para o novo número de porta.
