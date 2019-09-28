---
title: Componentes locais do locatário
description: Descreve os componentes locais na implantação do RDS.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: a9ff1382d2a2e7e2acf0247fa2ba4ae8e9642162
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387845"
---
# <a name="tenant-on-premises-components"></a>Componentes locais do locatário

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

As informações a seguir descrevem os componentes locais que compõem a implantação da hospedagem de área de trabalho.  
  
##  <a name="clients"></a>Clientes  
Para acessar as áreas de trabalho e aplicativos hospedados, os usuários devem usar clientes de Área de Trabalho Remota que dão suporte a RDP (Protocolo de Área de Trabalho Remota) 7.1 ou posterior. Em particular, o cliente deve ter suporte para o Gateway de Área de Trabalho Remota e o Agente de Conexão de Área de Trabalho Remota. Para entregar aplicativos para área de trabalho local, o cliente também deve ter suporte para o recurso RemoteApp. Para alcançar a escala mais alta do gateway, o cliente deve oferecer ter suporte para conexões de transporte HTTP puras ao Gateway de Área de Trabalho Remota.  
  
Informações adicionais:  
[Dispositivos habilitados para RemoteFX](https://social.technet.microsoft.com/wiki/contents/articles/14534.remotefx-enabled-devices.aspx)  
[Novidades no Gateway de Área de Trabalho Remota do Windows Server 2012 R2](https://blogs.technet.microsoft.com/enterprisemobility/2013/03/14/whats-new-in-windows-server-2012-remote-desktop-gateway/#transport)  
[Clientes de Área de Trabalho Remota da Microsoft](https://technet.microsoft.com/library/dn473009.aspx)  
[Aplicativo de Área de Trabalho Remota para Windows na Microsoft Store](https://apps.microsoft.com/windows/app/remote-desktop/051f560e-5e9b-4dad-8b2e-fa5e0b05a480)  
[Área de Trabalho Remota da Microsoft - Aplicativos Android no Google Play](https://play.google.com/store/apps/details?id=com.microsoft.rdc.android)  
[Mac App Store - Área de Trabalho Remota da Microsoft](https://itunes.apple.com/app/microsoft-remote-desktop/id715768417?mt=12)  
[Área de Trabalho Remota da Microsoft na App Store](https://itunes.apple.com/app/microsoft-remote-desktop/id714464092?mt=8)  
  
##  <a name="active-directory-domain-services"></a>Active Directory Domain Services  
Alguns locatários maiores e mais sofisticados podem optar por hospedar um servidor AD DS (Active Directory Domain Services) em suas locais. Nesse caso, o servidor AD DS no ambiente do locatário normalmente será uma réplica do servidor do AD DS que está no local do locatário. Há suporte para isso criando uma rede virtual no ambiente do locatário e usando a VPN do Azure para criar uma conexão site a site da rede local do locatário para a rede virtual do locatário no data center do Azure.  
  
Informações adicionais:  
[Visão geral da rede virtual do Microsoft Azure](https://azure.microsoft.com/documentation/articles/virtual-networks-overview/)  
[Criar um uma VNet do gerenciador de recursos com uma conexão VPN Site a Site usando o portal do Azure](https://azure.microsoft.com/documentation/articles/vpn-gateway-howto-site-to-site-resource-manager-portal/)  


