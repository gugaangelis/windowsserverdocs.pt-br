---
title: Alterar a porta de escuta na Área de Trabalho Remota
description: Saiba como alterar a porta de escuta do cliente de Área de Trabalho Remota.
ms.topic: article
author: lizap
ms.author: elizapo
ms.date: 07/19/2018
ms.localizationpriority: medium
ms.openlocfilehash: 6c28fbc7e453121e0a885e390f4b8435023a007e
ms.sourcegitcommit: 67a486b4fb3937a457eb00d21a2e33b753489fd8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/12/2020
ms.locfileid: "88149540"
---
# <a name="change-the-listening-port-for-remote-desktop-on-your-computer"></a>Alterar a porta de escuta da Área de Trabalho Remota em seu computador

>Aplica-se a: Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2008 R2

Quando você conecta a um computador (um cliente do Windows ou o Windows Server) por meio do cliente de Área de Trabalho Remota, o recurso de Área de Trabalho Remota no computador "ouve" a solicitação de conexão por uma porta de escuta definida (3389 por padrão). É possível alterar essa porta de escuta em computadores Windows modificando o registro.

1. Inicie o Editor do Registro. (Digite regedit na caixa de pesquisa.)
2. Navegue até a seguinte subchave do registro: **HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp**
3. Localize **PortNumber**
4. Clique em **Editar > Modificar** e, em seguida, clique em **Decimal**.
5. Digite o número da porta e, em seguida, clique em **OK**. 
6. Feche o editor do registro e reinicie o computador.

Da próxima vez que conectar a este computador usando a conexão de Área de Trabalho Remota, você deve digitar a nova porta. Se você estiver usando um firewall, certifique-se de configurar o firewall para permitir conexões para o novo número de porta.
