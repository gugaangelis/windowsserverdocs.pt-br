---
title: Cliente de Desktop remoto - compatível com a configuração
description: Saiba quais PCs, você pode acessar usando clientes de área de trabalho remota
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
ms.sourcegitcommit: 96e968bbe8dc50ebb1535ae1c8ce92fa73c83171
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2018
ms.locfileid: "1978043"
---
# <a name="remote-desktop-client---supported-configuration"></a>Cliente de Desktop remoto - compatível com a configuração

## <a name="supported-pcs"></a>PCs compatíveis
Você pode conectar PCs que estejam executando os seguintes sistemas operacionais Windows:
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

O gateway de área de trabalho remota podem ser executado nos seguintes computadores:

- Windows Server 2008
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016
- Windows Small Business Server 2011

Os seguintes sistemas operacionais podem servir como servidores de acesso via Web RD ou RemoteApp:
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

## <a name="unsupported-windows-versions-and-editions"></a>Edições e versões do Windows não suportados

O cliente de área de trabalho remota não se conectará para estas versões do Windows e edições:

- Windows 7 Starter
- Página inicial do Windows 7
- Página inicial do Windows 8
- Página inicial do Windows 8.1
- Windows 10 Home

Se você deseja acessar os computadores que possuem uma dessas versões do Windows instaladas, recomendamos que você atualizar para uma versão do Windows que ofereça suporte a RDP.

## <a name="rd-gateway-messaging-is-not-supported"></a>Não há suporte para mensagens de Gateway RD
Área de trabalho remota não oferece suporte a mensagens de Gateway RD. Verifique se a área de trabalho recurso diretiva de acesso remoto (remota) para seu servidor de Gateway RD não especificar **apenas permitem que os computadores com suporte para mensagens de Gateway RD** ou não será capaz de se conectar.