---
title: Serviço de Cadastramento na Internet do Windows (WINS)
description: Este tópico fornece informações sobre como encerrar o WINS e usar o DNS para serviços de resolução de nomes em sua rede.
manager: brianlic
ms.topic: article
ms.assetid: 32eabe7d-1130-4001-a79a-8ddb31993e5b
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 36dc2b0e8bbb6b65b0cc3568641017aa51122650
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87955493"
---
#  <a name="windows-internet-name-service-wins"></a>Serviço de Cadastramento na Internet do Windows (WINS)

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

O serviço WINS é um serviço herdado de registro e resolução de nomes de computadores que mapeia nomes NetBIOS de computadores para endereços IP.

Se você ainda não tiver o WINS implantado em sua rede, não implante o WINS-em vez disso, implante o DNS do sistema de nome de domínio \( \) . O DNS também fornece serviços de registro e resolução de nome de computador e inclui muitos benefícios adicionais sobre o WINS, como a integração com o Active Directory Domain Services.

Para obter mais informações, consulte [DNS (sistema de nomes de domínio)](https://docs.microsoft.com/windows-server/networking/dns/dns-top)

Se você já tiver implantado o WINS em sua rede, é recomendável implantar o DNS e, em seguida, encerrar o WINS.
