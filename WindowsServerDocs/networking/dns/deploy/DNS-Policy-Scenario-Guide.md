---
title: Guia do cenário de política de DNS
description: Este tópico faz parte do DNS política cenário guia para Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 50fdb08a-bbd8-4107-954a-6699672110ff
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7eab7aa403271992e9bc38f20ca0ddfa52bdd9cf
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="dns-policy-scenario-guide"></a>Guia do cenário de política de DNS

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este guia destina-se ao uso por administradores de sistemas, rede e DNS.  
  
Política de DNS é um novo recurso para DNS no Windows Server&reg; 2016. Você pode usar este guia para aprender a usar a política DNS para controlar como um servidor DNS processa consultas de resolução de nomes com base nos parâmetros diferentes que você define em políticas.   
  
Este guia contém informações de visão geral de política DNS, bem como cenários específicos de diretiva DNS que fornecem instruções sobre como configurar o comportamento do servidor DNS para atingir seus objetivos, incluindo o gerenciamento de tráfego com base em localização geográfica para servidores DNS primário e secundários, alta disponibilidade de aplicativos, uma DNS e muito mais.  
  
Este guia contém as seguintes seções.  
  
- [Visão geral das políticas DNS](DNS-Policies-Overview.md)  
- [Usar a política de DNS para localização geográfica com base em gerenciamento de tráfego com servidores primários](primary-geo-location.md)  
- [Usar a política de DNS para localização geográfica com base em gerenciamento de tráfego com implantações de primário secundário](primary-secondary-geo-location.md)  
- [Usar a política de DNS para respostas DNS inteligente com base na hora do dia](dns-tod-intelligent.md)
- [O servidor do aplicativo de nuvem respostas DNS com base na hora do dia com um Azure](dns-tod-azure-cloud-app-server.md)
- [Usar a política de DNS para a implantação de uma DNS](split-brain-DNS-deployment.md)
- [Usar a política DNS para um DNS no Active Directory](dns-sb-with-ad.md)
- [Usar a política DNS para aplicação de filtros em consultas do DNS](apply-filters-on-dns-queries.md)
- [Usar a política DNS para balanceamento de carga do aplicativo](app-lb.md)
- [Usar a política DNS para aplicativo balanceamento de carga com reconhecimento de localização geográfica](app-lb-geo.md)

