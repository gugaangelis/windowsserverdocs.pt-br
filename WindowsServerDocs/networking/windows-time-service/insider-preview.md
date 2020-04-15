---
title: Insider Preview para os recursos do Serviço de Horário do Windows no Windows Server 2019
description: Novos recursos do Serviço de Horário do Windows no Windows Server 2019
author: dcuomo
ms.author: dacuo
ms.date: 09/05/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: f26822d52b55191ad7096135a2757e9f72b7e772
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861639"
---
# <a name="insider-preview"></a>Insider Preview 


## <a name="leap-second-support"></a>Suporte a segundo bissexto


>Aplica-se a: Windows Server 2019 e Windows 10, versão 1809

Um segundo bissexto é um ajuste ocasional de 1 segundo para UTC. À medida que a rotação da Terra desacelera, o [UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) (escala de tempo atômica) diverge da [hora solar](https://en.wikipedia.org/wiki/Solar_time#Mean_solar_time) ou do tempo astronômico.  Depois que o UTC tiver divergido em pelo menos 0,9 segundos, um [segundo bissexto](https://en.wikipedia.org/wiki/Leap_second) será inserido para manter o UTC em sincronia com o tempo solar médio.

Os segundos intercalares se tornaram importantes para atender aos requisitos regulatórios de precisão e rastreabilidade nos Estados Unidos e na União Europeia.

Para obter mais informações, consulte:

-  Nosso [blog de comunicado](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  Guia de Validação para [Desenvolvedores](https://aka.ms/Dev-LeapSecond)

-  Guia de Validação para [Profissionais de TI](https://aka.ms/ITPro-LeapSecond)


## <a name="precision-time-protocol"></a>Protocolo de tempo de precisão

>Aplica-se a: Windows Server 2019 e Windows 10, versão 1809

Um novo provedor de horário incluído no Windows Server 2019 e no Windows 10 (versão 1809) permite que você sincronize a hora usando o protocolo PTP. Conforme o tempo é distribuído em uma rede, ocorre atraso (latência) que, se não for contabilizado ou não for simétrico, dificultará cada vez mais o entendimento do carimbo de data/hora enviado do servidor de horário. O PTP permite que os dispositivos de rede adicionem a latência introduzida por cada dispositivo de rede nas medições de tempo, fornecendo, assim, um tempo bem mais preciso ao cliente Windows.

Para obter mais informações, consulte:

-  Nosso [blog de comunicado](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  Guia de Validação para [Profissionais de TI](https://aka.ms/PTPValidation)


## <a name="software-timestamping"></a>Carimbo de data/hora do software

>Aplica-se a: Windows Server 2019 e Windows 10, versão 1809

Ao receber um pacote de tempo pela rede de um servidor de horário, ele deve ser processado pela pilha de rede do sistema operacional antes de ser consumido no serviço de horário. Cada componente na pilha de rede introduz uma quantidade variável de latência que afeta a precisão da medição de tempo.

![carimbo de data/hora do software](../media/Windows-Time-Service/software-timestamping.png)

Para resolver esse problema, o carimbo de data/hora do software nos permite carimbar pacotes com data/hora antes e depois dos "Componentes de Rede do Windows" mostrados acima para considerar o atraso no sistema operacional.

Para obter mais informações, consulte:

-  Nosso [blog de comunicado](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  Guia de Validação para [Profissionais de TI](https://github.com/Microsoft/SDN/blob/master/FeatureGuide/Validation%20Guide%20-%20RS5%20-%20Software%20Timestamping.docx)



---