---
title: Serviço de Cadastramento na Internet do Windows (WINS)
description: Este tópico fornece informações sobre o encerramento do WINS e uso do DNS para serviços de resolução de nome em sua rede.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 32eabe7d-1130-4001-a79a-8ddb31993e5b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bbc1871d29021aa3c99f14368a4711dac63f4cee
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843627"
---
#  <a name="windows-internet-name-service-wins"></a>Serviço de Cadastramento na Internet do Windows (WINS)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

O serviço WINS é um serviço herdado de registro e resolução de nomes de computadores que mapeia nomes NetBIOS de computadores para endereços IP.

Se você ainda não tiver WINS implantados em sua rede, não implantam o WINS - em vez disso, implantar o sistema de nomes de domínio \(DNS\). DNS também fornece serviços de registro e resolução de nome de computador e inclui muitos benefícios adicionais sobre o WINS, como integração com serviços de domínio do Active Directory.

Para obter mais informações, consulte [sistema de nome de domínio (DNS)](https://docs.microsoft.com/windows-server/networking/dns/dns-top)

Se você já tiver implantado o WINS na rede, é recomendável que você implantar o DNS e, em seguida, encerrar o WINS.
