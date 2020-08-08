---
title: Guia de cenários de política de DNS
description: Este tópico faz parte do guia de cenário de política DNS do Windows Server 2016
manager: brianlic
ms.topic: article
ms.assetid: 50fdb08a-bbd8-4107-954a-6699672110ff
ms.author: lizross
author: eross-msft
ms.openlocfilehash: ed28fe6dd472b505d2a39ac55c74c399ef63e068
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87966653"
---
# <a name="dns-policy-scenario-guide"></a>Guia de cenários de política de DNS

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este guia destina-se ao uso por administradores de DNS, rede e sistemas.

A política DNS é um novo recurso para DNS no Windows Server &reg; 2016. Você pode usar este guia para aprender a usar a política DNS para controlar como um servidor DNS processa consultas de resolução de nomes com base em parâmetros diferentes que você define em políticas.

Este guia contém informações de visão geral da política DNS, bem como cenários de política DNS específicos que fornecem instruções sobre como configurar o comportamento do servidor DNS para atingir suas metas, incluindo o gerenciamento de tráfego baseado na localização geográfica para servidores DNS primários e secundários, alta disponibilidade do aplicativo, DNS de divisão e muito mais.

Este guia contém as seções a seguir.

- [Visão geral das políticas de DNS](DNS-Policies-Overview.md)
- [Usar política de DNS para gerenciamento de tráfego baseado em localização geográfica com servidores primários](primary-geo-location.md)
- [Usar política de DNS para gerenciamento de tráfego baseado em localização geográfica com implantações primárias e secundárias](primary-secondary-geo-location.md)
- [Usar a política de DNS para respostas de DNS inteligente com base na hora do dia](dns-tod-intelligent.md)
- [Respostas DNS com base na hora do dia com um servidor de aplicativos de nuvem do Azure](dns-tod-azure-cloud-app-server.md)
- [Usar a política DNS para implantação de DNS de divisão-Brain](split-brain-DNS-deployment.md)
- [Usar a política DNS para o DNS de divisão de cérebro no Active Directory](dns-sb-with-ad.md)
- [Usar a política DNS para aplicar filtros em consultas DNS](apply-filters-on-dns-queries.md)
- [Usar a Política de DNS para balanceamento de carga de aplicativo](app-lb.md)
- [Usar a Política de DNS para balanceamento de aplicativo com reconhecimento de localização geográfica](app-lb-geo.md)

