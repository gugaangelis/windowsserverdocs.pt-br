---
title: Cliente de Desktop remoto - configuração com suporte
description: Saiba quais computadores você pode acessar usando clientes de área de trabalho remota
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884687"
---
# <a name="remote-desktop-client---supported-configuration"></a>Cliente de Desktop remoto - configuração com suporte

## <a name="supported-pcs"></a>PCs compatíveis
Você pode se conectar a computadores que executam os seguintes sistemas operacionais Windows:
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

Os computadores a seguir podem executar o gateway de área de trabalho remota:

- Windows Server 2008
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016
- Windows Small Business Server 2011

Os seguintes sistemas operacionais pode funcionar como servidores de acesso via Web RD ou RemoteApp:
- Windows Server 2008 R2
- Windows Server 2012
- Windows Server 2012 R2
- Windows Server 2016

## <a name="unsupported-windows-versions-and-editions"></a>Edições e versões do Windows sem suporte

O cliente de área de trabalho remota não conectará para essas edições e versões do Windows:

- Windows 7 Starter
- Windows 7 Home
- Windows 8 Home
- Página inicial do Windows 8.1
- Windows 10 Home

Se você quiser acessar os computadores que possuem uma dessas versões do Windows instaladas, é recomendável que você atualize para uma versão do Windows que dá suporte ao RDP.

## <a name="rd-gateway-messaging-is-not-supported"></a>Não há suporte para o sistema de mensagens do Gateway de área de trabalho remota
Cliente de área de trabalho remota não oferece suporte a mensagens do Gateway de área de trabalho remota. Verifique se que a área de trabalho recurso de diretiva de acesso remoto (RD RAPS) para o servidor de Gateway de área de trabalho remota não especifique **apenas permitir que os computadores com suporte para mensagens do Gateway de área de trabalho remota** ou não ser capaz de se conectar.