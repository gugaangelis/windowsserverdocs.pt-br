---
title: Cliente de Área de Trabalho Remota - configuração com suporte
description: Saiba quais computadores você pode acessar usando clientes de Área de Trabalho Remota
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bb932dad-6f74-484f-8f7b-dd957b615d44
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 06/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: d38008b6387385917ad21ce7e169b8ff3f4d18ba
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2019
ms.locfileid: "63748967"
---
# <a name="remote-desktop-client---supported-configuration"></a>Cliente de Área de Trabalho Remota - configuração com suporte

## <a name="supported-pcs"></a>PCs compatíveis
É possível conectar a computadores que executam os seguintes sistemas operacionais Windows:
- Windows 10 Pro
- Windows 10 Enterprise
- Windows 8 Enterprise
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