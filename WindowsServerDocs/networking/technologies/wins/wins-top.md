---
title: Serviço de nomes de Internet do Windows (WINS)
description: Este tópico fornece informações sobre o encerramento do WINS e usando o DNS para serviços de resolução de nome em sua rede.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 32eabe7d-1130-4001-a79a-8ddb31993e5b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5a3d132ada7b1ede83b046499058399a9da12190
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
#  <a name="windows-internet-name-service-wins"></a>Serviço de nomes de Internet do Windows (WINS)

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Serviço de cadastramento na Internet do Windows (WINS) é um computador herdados registro e resolução de serviço de nomes que mapeia os nomes de computadores NetBIOS para endereços IP.

Se você não tiver WINS implantados em sua rede, não implantar WINS - em vez disso, implantar \(DNS\) Domain Name System. DNS também fornece serviços de registro e resolução de nome de computador e inclui muitos benefícios adicionais sobre o WINS, como a integração com serviços de domínio do Active Directory.

Para obter mais informações, consulte [sistema de nome de domínio (DNS)](https://docs.microsoft.com/windows-server/networking/dns/dns-top)

Se já tiver implantado o WINS em sua rede, é recomendável que você implantar DNS e encerrar o WINS.
