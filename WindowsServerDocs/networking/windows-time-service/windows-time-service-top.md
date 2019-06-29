---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Serviço de Tempo do Windows
description: ''
author: shortpatti
ms.author: pashort
manager: dougkim
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 3233434403594ef9e2555c0329c4791d1fb99709
ms.sourcegitcommit: 63926404009f9e1330a4a0aa8cb9821a2dd7187e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67469584"
---
# <a name="windows-time-service-w32time"></a>Serviço de tempo do Windows (W32Time)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 ou posterior

O serviço de tempo do Windows (W32Time) sincroniza a data e hora para todos os computadores em execução nos serviços de domínio Active Directory (AD DS). Sincronização de hora é essencial para a operação correta de muitos serviços do Windows e aplicativos de linha de negócios (LOB). O serviço de tempo do Windows usa o protocolo NTP (Network Time) para sincronizar os relógios do computador na rede. NTP garante que um valor preciso de relógio ou carimbo de hora pode ser atribuído para solicitações de acesso de validação e recursos de rede.

O tópico de serviço de tempo do Windows (W32Time), o seguinte conteúdo está disponível:
- **[Hora precisa do Windows Server 2016](accurate-time.md).** Precisão de sincronização de hora no Windows Server 2016 melhorou substancialmente, mantendo de modo completo com versões anteriores NTP compatibilidade com versões mais antigas do Windows. Em condições de operação razoáveis, você pode manter um 1 ms precisão em relação ao UTC ou melhor para os membros de domínio do Windows Server 2016 e atualização de aniversário do Windows 10.
- **[Limite de suporte para ambientes de alta precisão](support-boundary.md).** Este artigo descreve os limites de suporte para o serviço de tempo do Windows (W32Time) em ambientes que exigem tempo do sistema estável e altamente precisos.
- **[Configurando os sistemas de alta precisão](configuring-systems-for-high-accuracy.md).** Sincronização de hora no Windows 10 e Windows Server 2016 foi consideravelmente aprimorada.  Em condições de operação razoáveis, os sistemas podem ser configurados para manter a 1 ms (milissegundos) melhor (em relação ao UTC) ou precisão.
- **[Tempo do Windows para Rastreabilidade](windows-time-for-traceability.md).** Regulamentos em muitos setores exigem sistemas para ser rastreadas para UTC.  Isso significa que o deslocamento do sistema pode ser Atestado em relação ao UTC.  Para habilitar cenários de conformidade normativa, o Windows 10 e no Server 2016 fornece novos logs de eventos para fornecer uma imagem da perspectiva do sistema operacional para formar uma compreensão das ações tomadas no relógio do sistema.  Esses logs de eventos são gerados continuamente para o serviço de tempo do Windows e pode ser examinados ou arquivados para análise posterior.
- **[Referência técnica do serviço de tempo do Windows](windows-time-service-tech-ref.md).** O serviço W32Time fornece sincronização de relógio de rede para computadores sem a necessidade de uma ampla configuração. O serviço W32Time é essencial para a operação bem-sucedida de autenticação Kerberos V5 e, portanto, a autenticação do AD DS.
    - **[Como funciona o serviço horário do Windows](How-the-Windows-Time-Service-Works.md).** Embora o serviço horário do Windows não seja uma implementação exata do protocolo NTP (Network Time), ele usa o pacote complexo de algoritmos que é definido nas especificações de NTP para garantir que os relógios dos computadores em toda uma rede sejam tão precisos quanto possível.
    - **[Ferramentas de serviço de tempo do Windows e configurações](Windows-Time-Service-Tools-and-Settings.md).** A maioria dos computadores de membros do domínio tem um tipo de cliente do tempo do NT5DS, o que significa que eles sincronizem o horário da hierarquia de domínio. A exceção apenas típica é o controlador de domínio que funciona como o mestre de operações do emulador PDC (controlador) domínio primário do domínio raiz da floresta, que geralmente é configurado para sincronizar a hora com uma fonte de tempo externa.


## <a name="related-topics"></a>Tópicos relacionados
Para obter mais informações sobre a hierarquia de domínio e o sistema de pontuação, consulte o ["Qual é o serviço de tempo do Windows?"](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) .

É o modelo de plug-in de provedor de tempo do windows [documentadas no TechNet](https://msdn.microsoft.com/library/windows/desktop/ms725475%28v=vs.85%29.aspx).

Um adendo mencionado no artigo de tempo do Windows 2016 precisos pode ser baixado [aqui](https://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf)

Para obter uma visão geral do serviço de tempo do Windows, dê uma olhada neste [vídeo de visão geral](https://aka.ms/WS2016TimeVideo).
