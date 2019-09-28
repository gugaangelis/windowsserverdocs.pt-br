---
ms.assetid: ''
title: Recursos do serviço de tempo do insider Preview para Windows no Windows Server 2019
description: Novos recursos de serviço de tempo do Windows no Windows Server 2019
author: shortpatti
ms.author: pashort
ms.date: 09/05/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 38682afa37a4c6882ee2e63a4abf4cd9fdbd2b27
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405212"
---
# <a name="insider-preview"></a>Visualização de insider 


## <a name="leap-second-support"></a>Segundo suporte Leap


>Aplica-se a: Windows Server 2019 e Windows 10, versão 1809

Um segundo salto é um ajuste ocasional de 1 segundo para UTC. À medida que a rotação da terra fica lenta, a [UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) (uma escala de tempo atômica) diverge da [média de hora solar](https://en.wikipedia.org/wiki/Solar_time#Mean_solar_time) ou astronômicos.  Depois que o UTC tiver divergência por no máximo. 9 segundos, um [segundo salto](https://en.wikipedia.org/wiki/Leap_second) será inserido para manter o UTC em sincronia com o tempo solar médio.

Os segundos de salto se tornaram importantes para atender aos requisitos regulatórios de precisão e rastreabilidade no Estados Unidos e na União Europeia.

Para obter mais informações, consulte:

-  Nosso [blog de anúncios](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  Guia de validação para os [desenvolvedores](https://aka.ms/Dev-LeapSecond)

-  Guia de validação do [profissional de ti](https://aka.ms/ITPro-LeapSecond)


## <a name="precision-time-protocol"></a>Protocolo de tempo de precisão

>Aplica-se a: Windows Server 2019 e Windows 10, versão 1809

Um novo provedor de tempo incluído no Windows Server 2019 e no Windows 10 (versão 1809) permite que você sincronize a hora usando o protocolo PTP. À medida que o tempo é distribuído em uma rede, ele encontra atraso (latência), que, se não for, ou se não for simétrico, torna-se cada vez mais difícil entender o carimbo de data/hora enviado do servidor de horário. O PTP permite que os dispositivos de rede adicionem a latência introduzida por cada dispositivo de rede às medições de tempo, fornecendo, assim, um exemplo de tempo muito mais preciso para o cliente Windows.

Para obter mais informações, consulte:

-  Nosso [blog de anúncios](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  Guia de validação do [profissional de ti](https://aka.ms/PTPValidation)


## <a name="software-timestamping"></a>Carimbo de data/hora do software

>Aplica-se a: Windows Server 2019 e Windows 10, versão 1809

Ao receber um pacote de tempo pela rede de um servidor de horário, ele deve ser processado pela pilha de rede do sistema operacional antes de ser consumido no serviço de tempo. Cada componente na pilha de rede introduz uma quantidade variável de latência que afeta a precisão da medição de tempo.

![Carimbo de data/hora do software](../media/Windows-Time-Service/software-timestamping.png)

Para resolver esse problema, o carimbo de data/hora do software nos permite pacotes de timestamp antes e depois dos "componentes de rede do Windows" mostrados acima para considerar o atraso no sistema operacional.

Para obter mais informações, consulte:

-  Nosso [blog de anúncios](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  Guia de validação do [profissional de ti](https://github.com/Microsoft/SDN/blob/master/FeatureGuide/Validation%20Guide%20-%20RS5%20-%20Software%20Timestamping.docx)



---