---
title: Componentes locais do locatário
description: Descreve os componentes no local em sua implantação do RDS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/01/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b3eebb38-a835-4fa6-9e41-1966014bf2cb
author: lizap
manager: dongill
ms.openlocfilehash: a01dbd12d76b1efa84e38f2ded38cfd613fb2ac4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857397"
---
# <a name="tenant-on-premises-components"></a>Componentes locais do locatário

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

As informações a seguir descrevem os componentes locais que compõem a implantação de hospedagem de área de trabalho.  
  
##  <a name="clients"></a>Clientes  
Para acessar a aplicativos e áreas de trabalho hospedadas, os usuários devem usar clientes de área de trabalho remota que dão suporte ao protocolo RDP (Remote Desktop) 7.1 ou posterior. Em particular, o cliente deve oferecer suporte de Gateway de área de trabalho remota e o agente de Conexão de área de trabalho remota. Para entregar aplicativos para área de trabalho local, o cliente também deve suportar o recurso de RemoteApp. Para alcançar a escala mais alta do gateway, o cliente deve oferecer suporte as conexões de transporte HTTP puras ao Gateway de área de trabalho remota.  
  
Informações adicionais:  
[Dispositivos habilitados para RemoteFX](https://social.technet.microsoft.com/wiki/contents/articles/14534.remotefx-enabled-devices.aspx)  
[O que há de novo no Windows Server 2012 R2 Remote Desktop Gateway](https://blogs.technet.microsoft.com/enterprisemobility/2013/03/14/whats-new-in-windows-server-2012-remote-desktop-gateway/#transport)  
[Clientes de área de trabalho remota da Microsoft](https://technet.microsoft.com/library/dn473009.aspx)  
[Aplicativo de área de trabalho remota para Windows na Microsoft Store](https://apps.microsoft.com/windows/app/remote-desktop/051f560e-5e9b-4dad-8b2e-fa5e0b05a480)  
[Área de trabalho remota da Microsoft - aplicativos Android no Google Play](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android)  
[Mac App Store - área de trabalho remota da Microsoft](https://itunes.apple.com/us/app/microsoft-remote-desktop/id715768417?mt=12)  
[Área de trabalho remota da Microsoft na Store do aplicativo](https://itunes.apple.com/us/app/microsoft-remote-desktop/id714464092?mt=8)  
  
##  <a name="active-directory-domain-services"></a>Active Directory Domain Services  
Alguns locatários maiores e mais sofisticados podem optar por hospedar um servidor de serviços de domínio Active Directory (AD DS) em suas instalações. Nesse caso, o servidor do AD DS no ambiente do locatário geralmente será uma réplica do servidor do AD DS que está no local do locatário. Isso é suportado, criando uma rede virtual no ambiente do locatário e usando a VPN do Azure para criar uma conexão site a site de rede do local do locatário para a rede virtual do locatário no data center do Azure.  
  
Informações adicionais:  
[Visão geral de rede Virtual do Microsoft Azure](https://azure.microsoft.com/documentation/articles/virtual-networks-overview/)  
[Criar um recurso de Gerenciador de rede virtual com uma conexão VPN Site a Site usando o Portal do Azure](https://azure.microsoft.com/documentation/articles/vpn-gateway-howto-site-to-site-resource-manager-portal/)  


