---
title: Guia de cenários de política de DNS
description: Este tópico faz parte do guia de cenário de política DNS do Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: 50fdb08a-bbd8-4107-954a-6699672110ff
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 2168bc6d2f2b3a5f365bb2738a15ce7f96408de2
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317927"
---
# <a name="dns-policy-scenario-guide"></a>Guia de cenários de política de DNS

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Este guia destina-se ao uso por administradores de DNS, rede e sistemas.  
  
A política DNS é um novo recurso para DNS no Windows Server&reg; 2016. Você pode usar este guia para aprender a usar a política DNS para controlar como um servidor DNS processa consultas de resolução de nomes com base em parâmetros diferentes que você define em políticas.   
  
Este guia contém informações de visão geral da política DNS, bem como cenários de política DNS específicos que fornecem instruções sobre como configurar o comportamento do servidor DNS para atingir suas metas, incluindo o gerenciamento de tráfego baseado na localização geográfica para primário e servidores DNS secundários, alta disponibilidade de aplicativo, DNS de divisão e muito mais.  
  
Este guia contém as seções a seguir.  
  
- [Visão geral das políticas de DNS](DNS-Policies-Overview.md)  
- [Usar a política DNS para o gerenciamento de tráfego baseado na localização geográfica com servidores primários](primary-geo-location.md)  
- [Usar a política DNS para o gerenciamento de tráfego baseado na localização geográfica com implantações primárias secundárias](primary-secondary-geo-location.md)  
- [Usar a política DNS para respostas de DNS inteligentes com base na hora do dia](dns-tod-intelligent.md)
- [Respostas DNS com base na hora do dia com um servidor de aplicativos de nuvem do Azure](dns-tod-azure-cloud-app-server.md)
- [Usar a política DNS para implantação de DNS de divisão-Brain](split-brain-DNS-deployment.md)
- [Usar a política DNS para o DNS de divisão de cérebro no Active Directory](dns-sb-with-ad.md)
- [Usar a política DNS para aplicar filtros em consultas DNS](apply-filters-on-dns-queries.md)
- [Usar política DNS para balanceamento de carga de aplicativo](app-lb.md)
- [Usar política DNS para balanceamento de carga de aplicativo com reconhecimento de localização geográfica](app-lb-geo.md)

