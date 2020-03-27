---
title: Serviço WINS
description: Este tópico fornece informações sobre como encerrar o WINS e usar o DNS para serviços de resolução de nomes em sua rede.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 32eabe7d-1130-4001-a79a-8ddb31993e5b
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 54e3f69500ccb3dbf6b2dfe47f6dd035e0a20511
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315160"
---
#  <a name="windows-internet-name-service-wins"></a>Serviço WINS

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

O serviço WINS é um serviço herdado de registro e resolução de nomes de computadores que mapeia nomes NetBIOS de computadores para endereços IP.

Se você ainda não tiver o WINS implantado em sua rede, não implante o WINS-em vez disso, implante o sistema de nome de domínio \(\)DNS. O DNS também fornece serviços de registro e resolução de nome de computador e inclui muitos benefícios adicionais sobre o WINS, como a integração com o Active Directory Domain Services.

Para obter mais informações, consulte [DNS (sistema de nomes de domínio)](https://docs.microsoft.com/windows-server/networking/dns/dns-top)

Se você já tiver implantado o WINS em sua rede, é recomendável implantar o DNS e, em seguida, encerrar o WINS.
