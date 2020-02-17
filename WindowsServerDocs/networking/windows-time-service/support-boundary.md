---
ms.assetid: ''
title: Limite de suporte para a hora de alta precisão
description: Este artigo descreve o limite de suporte para o Serviço de Horário do Windows (W32Time) em ambientes que exigem que a hora do sistema seja altamente precisa e estável.
author: shortpatti
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 212b9c79bc2e43e966180b928c865a9053332c3f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405269"
---
# <a name="support-boundary-for-high-accuracy-time"></a>Limite de suporte para a hora de alta precisão

>Aplica-se a: Windows Server 2016 e Windows 10 versão 1607 ou posterior

Este artigo descreve os limites de suporte para o Serviço de Horário do Windows (W32Time) em ambientes que exigem que a hora do sistema seja altamente precisa e estável.

## <a name="high-accuracy-support-for-windows-81-and-2012-r2-or-prior"></a>Suporte de Alta Precisão para Windows 8.1 e 2012 R2 (ou anterior)

Versões anteriores do Windows (antes do Windows 10 1607 ou do Windows Server 2016 1607) não podem garantir um tempo altamente preciso. O Serviço de Horário do Windows nesses sistemas:

-   Fornecia a precisão de tempo necessária para atender aos requisitos de autenticação do Kerberos versão 5

-   Fornecia um tempo menos preciso para clientes e servidores Windows ingressados em uma floresta do Active Directory comum

Os requisitos de precisão mais rígidos estavam fora da especificação de design do serviço de Horário do Windows nesses sistemas operacionais e não há suporte para isso.

## <a name="windows-10-and-windows-server-2016"></a>Windows 10 e Windows Server 2016

A precisão de tempo no Windows 10 e no Windows Server 2016 foi substancialmente aprimorada, mantendo, ao mesmo tempo, compatibilidade de NTP completa com versões mais antigas do Windows. Sob as condições operacionais corretas, os sistemas que executam o Windows 10 ou o Windows Server 2016 e versões mais recentes podem fornecer uma precisão de 1 segundo, 50 ms (milissegundos) ou 1 ms.

>[!IMPORTANT]
>**Fontes de tempo altamente precisas**<br>
>A precisão de tempo resultante em sua topologia é altamente dependente do uso de uma fonte de horário precisa e de raiz estável (estrato 1). Há hardware de fonte de horário NTP altamente preciso baseado em Windows, não baseado em Windows e compatível com o Windows vendido por fornecedores terceiros. Entre em contato com seu fornecedor com relação à precisão dos produtos dele.

>[!IMPORTANT]
>**Precisão do tempo**<br>
>A precisão do tempo envolve a distribuição de ponta a ponta de tempo preciso de uma fonte de horário autoritativo altamente preciso para o dispositivo final. Qualquer coisa que introduz a assimetria de rede influenciará negativamente a precisão, por exemplo, dispositivos de rede física ou alta carga de CPU no sistema de destino.

## <a name="high-accuracy-requirements"></a>Requisitos de alta precisão

O restante deste documento descreve os requisitos ambientais que devem ser atendidos para dar suporte aos respectivos destinos de alta precisão.

### <a name="target-accuracy-1-second-1s"></a>Precisão de destino: 1 s (1 segundo)

Para obter a precisão de 1s para um computador de destino específico em comparação com uma fonte de horário altamente precisa:

-   O sistema de destino deve executar o Windows 10 e o Windows Server 2016.

-   O sistema de destino deve sincronizar o tempo de uma hierarquia de NTP de servidores de horário, ocasionando uma fonte de horário NTP altamente precisa e compatível com o Windows.

-   Todos os sistemas operacionais Windows na hierarquia de NTP mencionados acima devem ser configurados conforme documentado na documentação [Configurar Sistemas para Alta Precisão](configuring-systems-for-high-accuracy.md).

-   A latência de rede unidirecional cumulativa entre o destino e a origem não deve exceder 100 ms. O atraso de rede cumulativa é medido adicionando atrasos unidirecionais individuais entre pares de nós cliente-servidor NTP na hierarquia começando com o destino e terminando na origem. Para obter mais informações, leia o documento de sincronização de tempo de alta precisão.

### <a name="target-accuracy-50-milliseconds"></a>Precisão de destino: 50 milissegundos

Todos os requisitos descritos na seção **Precisão de destino: 1 segundo** se aplicam, exceto em situações em que controles mais estritos forem descritos nesta seção.

Os requisitos adicionais para obter precisão de 50 ms para um sistema de destino específico são:

-   O computador de destino deve ter latência de rede melhor que 5 ms entre a fonte de horário.

-   O sistema de destino não deve ser maior do que o estrato 5 com base em uma fonte de horário altamente precisa

    >[!Note]
    >Execute "w32tm /query /status" na linha de comando para ver o estrato.

-   O sistema de destino deve estar dentro de 6 ou menos saltos de rede da fonte de horário altamente precisa

-   A utilização média de CPU de um dia em todos os estratos não deve exceder 90%

-   Para sistemas virtualizados, a utilização média de um dia da CPU do host não deve exceder 90%

### <a name="target-accuracy-1-millisecond"></a>Precisão de destino: 1 milissegundo

Todos os requisitos descritos nas seções **Precisão de destino: 1 segundo** e **Precisão de destino: 50 milissegundos** se aplicam, exceto em situações em que controles mais estritos forem descritos nesta seção.

Os requisitos adicionais para obter precisão de 1 ms para um sistema de destino específico são:

-   O computador de destino deve ter latência de rede melhor que 0,1 ms entre a fonte de horário

-   O sistema de destino não deve ser maior do que o estrato 5 com base em uma fonte de horário altamente precisa

    >[!Note]
    >Execute 'w32tm /query /status' na linha de comando para ver o estrato

-   O sistema de destino deve estar dentro de 4 ou menos saltos de rede da fonte de horário altamente precisa

-   A utilização média de CPU de um dia em cada estrato não deve exceder 80%

-   Para sistemas virtualizados, a utilização média de um dia da CPU do host não deve exceder 80%
