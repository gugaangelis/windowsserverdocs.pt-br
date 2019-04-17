---
title: Configurar a ordem das Interfaces de rede
description: Este tópico faz parte do guia ajuste de desempenho do subsistema de rede para Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 3266328c-ca82-40d2-90ca-854b7088ccaa
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2edf9d312e50a0fd8485e0032dee8413675367cf
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="configure-the-order-of-network-interfaces"></a>Configurar a ordem das Interfaces de rede

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

No Windows Server 2016 e no Windows 10, você pode usar a métrica de interface para configurar a ordem das interfaces de rede.

Isso é diferente em versões anteriores do Windows e Windows Server, que permitia configurar a ordem de vinculação dos adaptadores de rede usando a interface do usuário ou os comandos **INetCfgComponentBindings::MoveBefore** e **INetCfgComponentBindings::MoveAfter**. Esses dois métodos para ordenar interfaces de rede não estão disponíveis no Windows Server 2016 e Windows 10.

Em vez disso, você pode usar o novo método para definir a ordem enumerada dos adaptadores de rede, configurando a métrica de interface de cada adaptador. Você pode configurar a métrica de interface usando o [conjunto NetIPInterface](https://docs.microsoft.com/en-us/powershell/module/nettcpip/set-netipinterface) comando do Windows PowerShell.

Quando rotas de tráfego de rede são escolhidas e você tiver configurado o **InterfaceMetric** parâmetro do **conjunto NetIPInterface** de comando, a métrica geral que é usada para determinar a preferência de interface é a soma da métrica de rota e a métrica de interface. Normalmente, a métrica de interface dá preferência a uma determinada interface, como o uso com fio se ambos com fio e sem fio estão disponíveis.

O exemplo de comando do Windows PowerShell a seguir mostra o uso deste parâmetro.

    Set-NetIPInterface -InterfaceIndex 12 -InterfaceMetric 15

A ordem em que os adaptadores aparecem em uma lista é determinada pela métrica de interface IPv4 ou IPv6.  Para obter mais informações, consulte [GetAdaptersAddresses função](https://msdn.microsoft.com/library/windows/desktop/aa365915%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396).

Para obter links para todos os tópicos neste guia, consulte [ajuste de desempenho do subsistema de rede](net-sub-performance-top.md).
