---
title: Configurar uma estação de RDP através de LAN conectada no MultiPoint Services
description: Saiba como configurar um sistema RDP através de LAN no MultiPoint Services
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
ms.openlocfilehash: 8d2f1644918f1a581c1bcab181cd084e12c6b576
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843697"
---
# <a name="set-up-an-rdp-over-lan-connected-station-in-multipoint-services"></a>Configurar uma estação de RDP através de LAN conectada no MultiPoint Services
Uma estação de RDP através de LAN conectada é um cliente fino, tradicionais para desktop ou laptop que se conecta ao MultiPoint Services em uma rede local (LAN), usando o protocolo de área de trabalho remota (RDP). Para obter mais informações sobre esse e outros tipos de estação, consulte [estações do MultiPoint](MultiPoint-services-Stations.md).  
  
## <a name="to-set-up-a-multipoint-station-using-a-computer-or-thin-client-on-a-lan"></a>Para configurar uma estação do MultiPoint usando um computador ou um cliente fino em uma LAN  
  
1.  Ligue o computador que está executando o MultiPoint Services.  
  
2.  Certifique-se de que o computador do MultiPoint Server está conectado à rede local por um comutador, roteador ou outro dispositivo de rede e se tem um endereço IP apropriado. (Um endereço IP que começa com 169.254 (um endereço APIPA) pode indicar um problema com a conexão de LAN ou que o servidor DHCP não pode ser acessado ou não está funcionando corretamente).  
  
3.  Conecte-se o computador cliente ou cliente fino à LAN.  
  
4.  Ative o computador cliente ou cliente fino.  
  
5.  No computador cliente ou cliente fino, inicie a Conexão de área de trabalho remota ou um aplicativo equivalente e insira o nome ou endereço IP do computador executando o MultiPoint Services.

## <a name="set-up-a-windows-10-device-for-remote-management-by-using-connector-services"></a>Configurar um dispositivo Windows 10 para o gerenciamento remoto usando o conector de serviços
Qualquer PC ou um laptop executando o Windows 10 pode ser gerenciado remotamente desde que:
- os serviços de conector foram habilitados  
- o computador foi adicionado para computadores gerenciados no servidor do MultiPoint.  

No computador executando o Windows 10, siga estas etapas para habilitar o conector do MultiPoint:

1. Na caixa de pesquisa, digite "Windows ativar ou desativar recursos do" e selecione o resultado de pesquisa apropriada. 

2. A lista de recursos permitirá **conector do MultiPoint**. Isso permitirá que os serviços do MultiPoint conector que são necessários para gerenciar o dispositivo. 

No servidor do MultiPoint:
1. Abra o MultiPoint Manager e selecione **adicionar ou remover PCs** ou **adicionar ou remover do MultiPoint Services**.

2. Selecione os computadores remotos que você deseja gerenciar e clique em **Okey**.  Você será solicitado para credenciais de administrador nas máquinas remotas.  Depois que isso for feito, você verá as máquinas remotas na guia página inicial do MultiPoint Manager.

Quando com êxito o painel Gerenciador de conjunto pode monitorar os usuários que trabalham no dispositivo gerenciado.

> [!IMPORTANT]  
> Quando o monitoramento gerenciado administratrive de dispositivos Windows 10 que os usuários não podem ser monitorados, exceto o servidor de configurações foram alteradas adequadamente. Consulte [editar configurações do servidor](Edit-Server-Settings.md)