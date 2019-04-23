---
ms.assetid: ''
title: Limite de suporte para hora de alta precisão
description: Este artigo descreve o limite de suporte para o serviço de tempo do Windows (W32Time) em ambientes que exigem tempo do sistema estável e altamente precisos.
author: shortpatti
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 991bf4502546771dae9f092c6d5732f96b1278ab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866267"
---
# <a name="support-boundary-for-high-accuracy-time"></a>Limite de suporte para hora de alta precisão

>Aplica-se a: Windows Server 2016 e Windows 10 versão 1607 ou posterior

Este artigo descreve os limites de suporte para o serviço de tempo do Windows (W32Time) em ambientes que exigem tempo do sistema estável e altamente precisos.

## <a name="high-accuracy-support-for-windows-81-and-2012-r2-or-prior"></a>Suporte de alta precisão para Windows 8.1 e 2012 R2 (ou anterior)

Versões anteriores do Windows (antes do Windows 10 1607 ou Windows Server 2016 1607) não podem garantir tempo altamente preciso. O serviço de tempo do Windows nesses sistemas:

-   Fornecida a precisão de tempo necessário para atender aos requisitos de autenticação Kerberos versão 5

-   Fornecido tempo preciso livremente para servidores associados a uma floresta do Active Directory comuns e os clientes do Windows

Requisitos de precisão mais rigorosas estavam fora de especificação de design do serviço de tempo do Windows nesses sistemas operacionais e não é suportado.

## <a name="windows-10-and-windows-server-2016"></a>Windows 10 e Windows Server 2016

Precisão de tempo no Windows 10 e Windows Server 2016 foi consideravelmente aprimorado, mantendo de modo completo com versões anteriores NTP compatibilidade com versões mais antigas do Windows. Sob o direito de condições operacionais, sistemas que executam o Windows 10 ou Windows Server 2016 e versões mais recentes podem entregar 1 segundo, 50 de MS (milissegundos), ou 1 MS precisão.

>[!IMPORTANT]
>**Fontes de altamente precisos de tempo**<br>
>A precisão de tempo resultante em sua topologia é altamente dependente usando uma raiz precisa, estável (camada 1) fonte de tempo. Há Windows com base e não Windows com base altamente precisos, Windows compatível, horário NTP fonte hardware vendidos por fornecedores de terceiros 3º. Entre em contato com seu fornecedor em que a precisão de seus produtos.

>[!IMPORTANT]
>**Precisão de tempo**<br>
>A distribuição de ponta a ponta de horário com precisão de uma fonte de horário autoritativo altamente precisos para o dispositivo final envolve a precisão de tempo. Tudo o que introduz a assimetria rede influenciará negativamente precisão, dispositivos de rede física por exemplo ou alta carga de CPU no sistema de destino.

## <a name="high-accuracy-requirements"></a>Requisitos de alta precisão

O restante deste documento descreve os requisitos ambientais que devem ser atendidos para dar suporte a destinos respectivos alta precisão.

### <a name="target-accuracy-1-second-1s"></a>Precisão de destino: 1 segundo (1s)

Para alcançar 1s precisão para um destino específico de máquina em comparação com uma fonte de horário altamente precisos:

-   O sistema de destino deve executar o Windows 10, Windows Server 2016.

-   O sistema de destino deve sincronizar a hora de uma hierarquia de servidores de tempo, de NTP principal em uma fonte de tempo NTP altamente precisa, compatível com Windows.

-   Todos os sistemas operacionais de Windows na hierarquia de NTP mencionados acima devem ser configurados conforme documentado na [Configurando os sistemas de alta precisão](configuring-systems-for-high-accuracy.md) documentação.

-   A latência de rede unidirecional cumulativa entre a origem e destino não deve exceder 100 ms. O atraso de rede cumulativa é medido, adicionando o indivíduo unidirecionais atrasos entre pares de nós de cliente-servidor NTP na hierarquia, começando com o destino e terminando na origem. Para obter mais informações, leia o documento de sincronização de hora de alta precisão.

### <a name="target-accuracy-50-milliseconds"></a>Precisão de destino: 50 milissegundos

Todos os requisitos descritos na seção **precisão de destino: 1 segundo** se aplicam, exceto onde os controles mais rígidos são descritos nesta seção.

Os requisitos adicionais para alcançar a precisão de 50 ms para um sistema de destino específicos são:

-   O computador de destino deve ter melhor do que 5 ms de latência de rede entre sua fonte de tempo.

-   O sistema de destino deve ser nenhuma outra que stratum 5 em uma fonte de horário altamente precisos

    >[!Note]
    >Execute "w32tm. exe /status /query" na linha de comando para ver a camada.

-   O sistema de destino deve estar dentro de 6 ou saltos de rede da fonte de tempo altamente precisos

-   Um dia médio da CPU em todos os stratums não deve exceder 90%

-   Para sistemas virtualizados, um dia médio da CPU do host não deve exceder 90%

### <a name="target-accuracy-1-millisecond"></a>Precisão de destino: 1 milissegundo

Todos os requisitos descritos nas seções **precisão de destino: 1 segundo** e **precisão de destino: 50 milissegundos** se aplicam, exceto onde os controles mais rígidos são descritos nesta seção.

Os requisitos adicionais para alcançar a precisão de 1 ms para um sistema de destino específicos são:

-   O computador de destino deve ter melhor do que 0,1 ms de latência de rede entre sua fonte de tempo

-   O sistema de destino deve ser nenhuma outra que stratum 5 em uma fonte de horário altamente precisos

    >[!Note]
    >Execute 'w32tm. exe /status /query' na linha de comando para ver o stratum

-   O sistema de destino deve estar dentro de 4 ou menos saltos de rede da fonte de tempo altamente precisos

-   A utilização da CPU média de um dia em cada nível não deve exceder 80%

-   Para sistemas virtualizados, um dia médio da CPU do host não deve exceder 80%
