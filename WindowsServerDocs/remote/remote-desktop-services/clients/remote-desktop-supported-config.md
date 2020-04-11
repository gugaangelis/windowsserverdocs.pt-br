---
title: Cliente de Área de Trabalho Remota - configuração com suporte
description: Saiba quais computadores você pode acessar usando clientes de Área de Trabalho Remota
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: bb932dad-6f74-484f-8f7b-dd957b615d44
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 06/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: 1480a2a14a1c3fc23c4e5122e366741d37d9091f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856009"
---
# <a name="remote-desktop-client---supported-configuration"></a>Cliente de Área de Trabalho Remota - configuração com suporte

## <a name="supported-pcs"></a>PCs compatíveis
É possível conectar a computadores que executam os seguintes sistemas operacionais Windows:
- Windows 10 Pro
- Windows 10 Enterprise
- O Windows 8 Enterprise
- Windows 8 Professional
- Windows 7 Professional
- Windows 7 Enterprise
- Windows 7 Ultimate
- Windows 7 Ultimate
- Windows Server 2008
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016
- Windows Multipoint Server 2011
- Windows Multipoint Server 2012
- Windows Small Business Server 2008
- Windows Small Business Server 2011

Os computadores a seguir podem executar o Gateway de Área de Trabalho Remota:

- Windows Server 2008
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016
- Windows Small Business Server 2011

Os sistemas operacionais a seguir podem funcionar como servidores de Acesso via Web à Área de Trabalho Remota ou RemoteApp:
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

## <a name="unsupported-windows-versions-and-editions"></a>Edições e versões do Windows sem suporte

O cliente de Área de Trabalho Remota não conectará a estas edições e versões do Windows:

- Windows 7 Starter
- Windows 7 Home
- Windows 8 Home
- Windows 8.1 Home
- Windows 10 Home

Se você quiser acessar computadores que tenham uma dessas versões do Windows instaladas, é recomendável atualizar para uma versão do Windows com suporte a RDP.

## <a name="rd-gateway-messaging-is-not-supported"></a>Não há suporte para o sistema de mensagens do Gateway de Área de Trabalho Remota
O Cliente de Área de Trabalho Remota não dá suporte ao sistema de mensagens do Gateway de Área de Trabalho Remota. Verifique se a RD RAPS (Diretiva de Acesso ao Recurso da Área de Trabalho Remota) para o servidor de Gateway de Área de Trabalho Remota não especifica **Somente permitir computadores com suporte para mensagens do Gateway de Área de Trabalho Remota** ou não você não será capaz de conectar.