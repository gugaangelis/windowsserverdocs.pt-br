---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Serviço de Tempo do Windows
description: ''
author: eross-msft
ms.author: lizross
manager: dougkim
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 4b8b1e91f56ec4d6c070037a0f3cc5ec4d50c63e
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314919"
---
# <a name="windows-time-service-w32time"></a>Serviço de Horário do Windows (W32Time)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 ou posterior

O Serviço de Horário do Windows (W32Time) sincroniza a data e a hora de todos os computadores em execução em um AD DS (Active Directory Domain Services). A sincronização de tempo é essencial para a operação adequada de muitos serviços do Windows e de aplicativos LOB (linha de negócios). O Serviço de Horário do Windows usa o protocolo NTP para sincronizar os relógios dos computadores na rede. O NTP verifica se um valor de relógio exato ou um carimbo de data/hora pode ser atribuído a solicitações de validação de rede e de acesso a recursos.

No tópico do Serviço de Horário do Windows (W32Time), o seguinte conteúdo está disponível:
- **[Hora Precisa do Windows Server 2016](accurate-time.md).** A precisão da sincronização de hora no Windows Server 2016 foi substancialmente aprimorada, mantendo, ao mesmo tempo, a compatibilidade completa do NTP com versões mais antigas do Windows. Sob condições operacionais razoáveis, você pode manter uma precisão de 1 ms com relação ao UTC ou melhor para membros de domínio do Windows Server 2016 e da Atualização de Aniversário do Windows 10.
- **[Limite de suporte para ambientes de alta precisão](support-boundary.md).** Este artigo descreve os limites de suporte para o Serviço de Horário do Windows (W32Time) em ambientes que exigem que a hora do sistema seja altamente precisa e estável.
- **[Configurar sistemas para oferecer alta precisão](configuring-systems-for-high-accuracy.md).** A sincronização de tempo no Windows 10 e no Windows Server 2016 foi substancialmente aprimorada.  Em condições operacionais razoáveis, os sistemas podem ser configurados para manter a precisão de 1 ms (milissegundo) ou melhor (com respeito ao UTC).
- **[Tempo do Windows para Rastreabilidade](windows-time-for-traceability.md).** As regulamentações em muitos setores exigem que os sistemas sejam rastreáveis para o UTC.  Isso significa que a diferença de um sistema pode ser atestada com respeito ao UTC.  Para habilitar cenários de conformidade regulatória, o Windows 10 e o Server 2016 fornecem novos logs de eventos para oferecer uma visão da perspectiva do Sistema Operacional a fim de criar uma compreensão das ações executadas no relógio do sistema.  Esses logs de evento são gerados continuamente para o Serviço de Horário do Windows e podem ser examinados ou arquivados para análise posterior.
- **[Referência técnica do Serviço de Horário do Windows](windows-time-service-tech-ref.md).** O serviço W32Time fornece sincronização de relógio de rede para computadores sem a necessidade de configuração extensiva. O serviço W32Time é essencial para a operação bem-sucedida da autenticação do Kerberos V5 e, portanto, para a autenticação baseada em AD DS.
    - **[Como o Serviço de data/hora do Windows Funciona](How-the-Windows-Time-Service-Works.md).** Embora o Serviço de Horário do Windows não seja uma implementação exata do protocolo NTP, ele usa o complexo conjunto de algoritmos definido nas especificações do NTP para verificar se os relógios em computadores em toda a rede são os mais precisos possíveis.
    - **[Ferramentas e configurações do Serviço de Horário do Windows](Windows-Time-Service-Tools-and-Settings.md).** A maioria dos computadores membros do domínio tem um tipo de cliente de tempo de NT5DS, o que significa que eles sincronizam o tempo com base na hierarquia de domínio. A única exceção comum a isso é o controlador de domínio que funciona como o mestre de operações do emulador PDC (controlador de domínio primário) do domínio raiz da floresta, que geralmente é configurado para sincronizar o tempo com uma fonte de horário externa.


## <a name="related-topics"></a>Tópicos relacionados
Para obter mais informações sobre a hierarquia de domínio e o sistema de pontuação, confira [“O que é o Serviço de Horário do Windows?”](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) .

O modelo de plug-in do provedor de horário do Windows está [documentado no TechNet](https://msdn.microsoft.com/library/windows/desktop/ms725475%28v=vs.85%29.aspx).

Um adendo referenciado pelo artigo Tempo Preciso do Windows 2016 pode ser baixado [aqui](https://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf)

Para obter uma visão geral rápida do Serviço de Horário do Windows, assista a este [vídeo de visão geral de alto nível](https://aka.ms/WS2016TimeVideo).
