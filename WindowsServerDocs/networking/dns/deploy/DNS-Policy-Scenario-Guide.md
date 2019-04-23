---
title: Guia de cenários de política de DNS
description: Este tópico faz parte do DNS política cenário guia para o Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 50fdb08a-bbd8-4107-954a-6699672110ff
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b3a1400a68e22fc4988c87c9222b66f718cf5fd0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865637"
---
# <a name="dns-policy-scenario-guide"></a>Guia de cenários de política de DNS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este guia destina-se ao uso por administradores DNS, rede e sistemas.  
  
Política de DNS é um novo recurso para o DNS no Windows Server&reg; 2016. Você pode usar este guia para aprender a usar a política DNS para controlar como um servidor DNS processa consultas de resolução de nome com base nos parâmetros diferentes que definem em políticas.   
  
Este guia contém informações de visão geral da política DNS, bem como cenários específicos de política DNS que fornecem instruções sobre como configurar o comportamento do servidor DNS para alcançar suas metas, incluindo o gerenciamento de tráfego com base em localização geográfica para primário e servidores DNS secundários, alta disponibilidade de aplicativos, o DNS com partição de rede e muito mais.  
  
Este guia contém as seções a seguir.  
  
- [Visão geral das políticas DNS](DNS-Policies-Overview.md)  
- [Usar a política de DNS para a localização geográfica com base em gerenciamento de tráfego com servidores primários](primary-geo-location.md)  
- [Usar a política de DNS para a localização geográfica com base em gerenciamento de tráfego com implantações primárias e secundárias](primary-secondary-geo-location.md)  
- [Usar política de DNS para respostas DNS inteligente com base na hora do dia](dns-tod-intelligent.md)
- [Servidor de aplicativos de nuvem de respostas DNS com base na hora do dia com o Azure](dns-tod-azure-cloud-app-server.md)
- [Usar a política de DNS para a implantação de DNS com partição de rede](split-brain-DNS-deployment.md)
- [Usar a política de DNS para o DNS com partição de rede no Active Directory](dns-sb-with-ad.md)
- [Usar a política DNS para a aplicação de filtros em consultas DNS](apply-filters-on-dns-queries.md)
- [Usar a política de DNS para balanceamento de carga do aplicativo](app-lb.md)
- [Usar a política de DNS para balanceamento de aplicativo com reconhecimento de localização geográfica](app-lb-geo.md)

