---
ms.assetid: ''
title: Visualização do Insider para recursos do serviço de tempo do Windows no Windows Server 2019
description: Novos recursos de serviço de tempo do Windows no Windows Server 2019
author: shortpatti
ms.author: pashort
ms.date: 09/05/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: ef0ff317f5957add5ecbe9f88ef83753b805ec41
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829017"
---
# <a name="insider-preview"></a>Visualização de insider 


## <a name="leap-second-support"></a>Suporte de fração de segundo


>Aplica-se a: 2019 do Windows Server e Windows 10, versão 1809

Fração de segundo é um ajuste de 1 segundo ocasional para UTC. Como reduz a rotação da Terra [UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) (uma escala de tempo atômica) divergirá dos [solar tempo médio de](https://en.wikipedia.org/wiki/Solar_time#Mean_solar_time) ou hora astronômico.  Depois que o UTC foi bifurcado no máximo.9 segundos, uma [fração de segundo](https://en.wikipedia.org/wiki/Leap_second) é inserido para manter o UTC em sincronia com o tempo médio de solar.

Segundos intercalares tornaram-se importante para atender aos requisitos normativos de rastreabilidade tanto nos Estados Unidos e da União Europeia e precisão.

Para obter mais informações, consulte:

-  Nosso [blog de comunicados](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  Guia de validação para o [desenvolvedores](https://aka.ms/Dev-LeapSecond)

-  Guia de validação para o [IT Pro](https://aka.ms/ITPro-LeapSecond)


## <a name="precision-time-protocol"></a>Protocolo de precisão de tempo

>Aplica-se a: 2019 do Windows Server e Windows 10, versão 1809

Um novo provedor de tempo incluído no Windows Server 2019 e Windows 10 (versão 1809) permite que você sincronize a hora usando o protocolo de tempo de precisão (PTP). Como o tempo distribui em uma rede, ele encontra atraso (latência), que, se não contabilizados, ou se não for simétrica, ele se torna cada vez mais difícil de entender o carimbo de hora enviado do servidor de tempo. PTP permite que os dispositivos de rede adicionar a latência introduzida por cada dispositivo de rede para as medidas de medição de tempo, fornecendo assim um exemplo de tempo muito mais preciso para o cliente do windows.

Para obter mais informações, consulte:

-  Nosso [blog de comunicados](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  Guia de validação para o [IT Pro](https://aka.ms/PTPValidation)


## <a name="software-timestamping"></a>Carimbo de software

>Aplica-se a: 2019 do Windows Server e Windows 10, versão 1809

Ao receber um pacote de tempo pela rede a partir de um servidor de horário, devem ser processado pela pilha de rede do sistema operacional antes que está sendo consumido no serviço de tempo. Cada componente na pilha da rede apresenta uma quantidade variável de latência que afeta a precisão da medição de tempo.

![carimbo de software](../media/Windows-Time-Service/software-timestamping.png)

Para resolver esse problema, o carimbo de software nos permite definir pacotes de carimbo de hora antes e depois "Windows Networking Components" mostrada acima para levar em conta o atraso no sistema operacional.

Para obter mais informações, consulte:

-  Nosso [blog de comunicados](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  Guia de validação para o [IT Pro](https://github.com/Microsoft/SDN/blob/master/FeatureGuide/Validation%20Guide%20-%20RS5%20-%20Software%20Timestamping.docx)



---