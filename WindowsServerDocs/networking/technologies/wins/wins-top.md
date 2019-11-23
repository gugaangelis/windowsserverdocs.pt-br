---
title: Serviço de Cadastramento na Internet do Windows (WINS)
description: Este tópico fornece informações sobre como encerrar o WINS e usar o DNS para serviços de resolução de nomes em sua rede.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 32eabe7d-1130-4001-a79a-8ddb31993e5b
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 219c313dfeb26319cd5f537df417724de7044648
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405246"
---
#  <a name="windows-internet-name-service-wins"></a>Serviço de Cadastramento na Internet do Windows (WINS)

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

O serviço WINS é um serviço herdado de registro e resolução de nomes de computadores que mapeia nomes NetBIOS de computadores para endereços IP.

Se você ainda não tiver o WINS implantado em sua rede, não implante o WINS-em vez disso, implante o sistema de nome de domínio \(\)DNS. O DNS também fornece serviços de registro e resolução de nome de computador e inclui muitos benefícios adicionais sobre o WINS, como a integração com o Active Directory Domain Services.

Para obter mais informações, consulte [DNS (sistema de nomes de domínio)](https://docs.microsoft.com/windows-server/networking/dns/dns-top)

Se você já tiver implantado o WINS em sua rede, é recomendável implantar o DNS e, em seguida, encerrar o WINS.
