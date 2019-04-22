---
title: Configurar a ordem das interfaces de rede
description: Este tópico faz parte do guia de ajuste de desempenho do subsistema de rede para o Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 3266328c-ca82-40d2-90ca-854b7088ccaa
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 18bc9a268b4e69e4b87b6b1e310f1162473adb10
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821767"
---
# <a name="configure-the-order-of-network-interfaces"></a>Configurar a ordem das interfaces de rede

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

No Windows Server 2016 e Windows 10, você pode usar a métrica de interface para configurar a ordem das interfaces de rede.

Isso é diferente em versões anteriores do Windows e Windows Server, que permitia que você configurar a ordem de ligação dos adaptadores de rede por meio da interface do usuário ou os comandos **INetCfgComponentBindings::MoveBefore**e **INetCfgComponentBindings::MoveAfter**. Esses dois métodos para ordenar as interfaces de rede não estão disponíveis no Windows Server 2016 e Windows 10.

Em vez disso, você pode usar o novo método para definir a ordem enumerada de adaptadores de rede ao configurar a métrica de interface de cada adaptador. Você pode configurar a métrica de interface usando o [Set-NetIPInterface](https://docs.microsoft.com/powershell/module/nettcpip/set-netipinterface) comando do Windows PowerShell.

Quando as rotas de tráfego de rede são escolhidas e você tiver configurado o **InterfaceMetric** parâmetro do **NetIPInterface conjunto** de comando, a métrica geral que é usada para determinar a interface preferência é a soma da métrica de rota e a métrica de interface. Normalmente, a métrica de interface dá preferência a uma interface específica, como o uso com fio se ambos com fio e sem fio estão disponíveis.

O exemplo de comando do Windows PowerShell a seguir mostra o uso desse parâmetro.

    Set-NetIPInterface -InterfaceIndex 12 -InterfaceMetric 15

A ordem na qual os adaptadores são exibidos em uma lista é determinada pela métrica de interface IPv4 ou IPv6.  Para obter mais informações, consulte [GetAdaptersAddresses função](https://msdn.microsoft.com/library/windows/desktop/aa365915%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396).

Para obter links para todos os tópicos neste guia, consulte [ajuste de desempenho do subsistema de rede](net-sub-performance-top.md).
