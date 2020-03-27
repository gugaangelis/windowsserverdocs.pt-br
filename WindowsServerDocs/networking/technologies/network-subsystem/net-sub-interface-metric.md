---
title: Configurar a ordem das interfaces de rede
description: Este tópico faz parte do guia de ajuste de desempenho do subsistema de rede para o Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 3266328c-ca82-40d2-90ca-854b7088ccaa
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d82009a4b6e9bcf20ae9cccb4259956358b53148
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316616"
---
# <a name="configure-the-order-of-network-interfaces"></a>Configurar a ordem das interfaces de rede

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

No Windows Server 2016 e no Windows 10, você pode usar a métrica de interface para configurar a ordem das interfaces de rede.

Isso é diferente das versões anteriores do Windows e do Windows Server, que permitia a configuração da ordem de Associação dos adaptadores de rede usando a interface do usuário ou os comandos **INetCfgComponentBindings:: MoveBefore** e **INetCfgComponentBindings:: MoveAfter**. Esses dois métodos para ordenar interfaces de rede não estão disponíveis no Windows Server 2016 e no Windows 10.

Em vez disso, você pode usar o novo método para definir a ordem enumerada dos adaptadores de rede configurando a métrica de interface de cada adaptador. Você pode configurar a métrica de interface usando o comando [set-NetIPInterface](https://docs.microsoft.com/powershell/module/nettcpip/set-netipinterface) do Windows PowerShell.

Quando as rotas de tráfego de rede são escolhidas e você configurou o parâmetro **InterfaceMetric** do comando **set-NetIPInterface** , a métrica geral usada para determinar a preferência de interface é a soma da métrica de rota e a métrica da interface. Normalmente, a métrica de interface dá preferência a uma interface específica, como usar com fio se ambos com e sem fio estiverem disponíveis.

O exemplo de comando do Windows PowerShell a seguir mostra o uso desse parâmetro.

    Set-NetIPInterface -InterfaceIndex 12 -InterfaceMetric 15

A ordem na qual os adaptadores aparecem em uma lista é determinada pela métrica da interface IPv4 ou IPv6.  Para obter mais informações, consulte [função GetAdaptersAddresses](https://msdn.microsoft.com/library/windows/desktop/aa365915%28v=vs.85%29.aspx?f=255&MSPPError=-2147217396).

Para obter links para todos os tópicos deste guia, consulte [ajuste de desempenho do subsistema de rede](net-sub-performance-top.md).
