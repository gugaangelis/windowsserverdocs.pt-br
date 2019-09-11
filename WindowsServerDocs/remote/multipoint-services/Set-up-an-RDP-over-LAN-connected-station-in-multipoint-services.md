---
title: Configurar uma estação conectada RDP em LAN nos serviços do MultiPoint
description: Saiba como configurar um sistema RDP via LAN nos serviços do MultiPoint
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 60e1a025-c2fb-4708-a3ff-c44c223a3224
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 40899b277ae60169a0eca34b359172941e5391fb
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70871557"
---
# <a name="set-up-an-rdp-over-lan-connected-station-in-multipoint-services"></a>Configurar uma estação conectada RDP em LAN nos serviços do MultiPoint
Uma estação conectada de RDP via LAN é um cliente fino, desktop tradicional ou laptop que se conecta aos serviços do MultiPoint em uma rede local (LAN) usando o protocolo RDP (RDP). Para obter mais informações sobre esse e outros tipos de estação, consulte [MultiPoint stations](MultiPoint-services-Stations.md).  
  
## <a name="to-set-up-a-multipoint-station-using-a-computer-or-thin-client-on-a-lan"></a>Para configurar uma estação do MultiPoint usando um computador ou cliente fino em uma LAN  
  
1.  Ligue o computador que está executando os serviços do MultiPoint.  
  
2.  Verifique se o computador do MultiPoint Server está conectado à LAN por um comutador, roteador ou outro dispositivo de rede e tem um endereço IP adequado. (Um endereço IP que começa com 169,254 (um endereço APIPA) pode indicar que há um problema com a conexão LAN ou que o servidor DHCP não pode ser acessado ou não está funcionando corretamente.)  
  
3.  Conecte o computador cliente ou o cliente fino à LAN.  
  
4.  Ligue o computador cliente ou o cliente fino.  
  
5.  No computador cliente ou cliente fino, inicie Conexão de Área de Trabalho Remota ou um aplicativo equivalente e insira o nome ou endereço IP do computador que executa os serviços do MultiPoint.

## <a name="set-up-a-windows-10-device-for-remote-management-by-using-connector-services"></a>Configurar um dispositivo Windows 10 para gerenciamento remoto usando serviços de conector
Qualquer PC ou laptop que esteja executando o Windows 10 pode ser gerenciado remotamente, desde que:
- os serviços do conector foram habilitados  
- o computador foi adicionado aos computadores gerenciados no MultiPoint Server.  

No PC que executa o Windows 10, siga estas etapas para habilitar o conector do MultiPoint:

1. Na caixa de pesquisa, digite "ativar ou desativar recursos do Windows" e selecione o resultado de pesquisa adequado. 

2. Na lista de recursos, habilite o **conector do MultiPoint**. Isso habilitará os serviços do conector do MultiPoint, que são necessários para gerenciar o dispositivo. 

No servidor MultiPoint:
1. Abra o Gerenciador do MultiPoint e selecione **Adicionar ou remover computadores pessoais** ou **Adicionar ou remover serviços do MultiPoint**.

2. Selecione os computadores remotos que você deseja gerenciar e clique em **OK**.  Você será solicitado a fornecer credenciais de administrador nos computadores remotos.  Quando isso for feito, você verá os computadores remotos na guia início do Gerenciador do MultiPoint.

Quando a configuração bem-sucedida do Dashboard Manager pode monitorar os usuários que estão trabalhando no dispositivo gerenciado.

> [!IMPORTANT]  
> Ao monitorar dispositivos Windows 10 gerenciados, os usuários administratrive não podem ser monitorados, exceto que as configurações do servidor foram alteradas de acordo. Consulte [Editar configurações do servidor](Edit-Server-Settings.md)