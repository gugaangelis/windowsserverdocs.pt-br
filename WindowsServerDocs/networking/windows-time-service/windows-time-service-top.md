---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Serviço de Tempo do Windows
description: ''
author: shortpatti
ms.author: pashort
manager: dougkim
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: e3dbaa188426ac81073e706db3adc6ab0a655c01
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405184"
---
# <a name="windows-time-service-w32time"></a>Serviço de tempo do Windows (W32Time)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows 10 ou posterior

O serviço de tempo do Windows (W32Time) sincroniza a data e a hora de todos os computadores em execução no Active Directory Domain Services (AD DS). A sincronização de tempo é essencial para a operação adequada de muitos serviços do Windows e de aplicativos de linha de negócios (LOB). O serviço de tempo do Windows usa o protocolo NTP (NTP) para sincronizar os relógios dos computadores na rede. O NTP garante que um valor de relógio exato ou carimbo de data/hora possa ser atribuído a solicitações de validação de rede e de acesso a recursos.

No tópico serviço de tempo do Windows (W32Time), o seguinte conteúdo está disponível:
- **[Tempo preciso do Windows Server 2016](accurate-time.md).** A precisão da sincronização de tempo no Windows Server 2016 foi aprimorada substancialmente, mantendo a compatibilidade de NTP completa com versões mais antigas do Windows. Em condições operacionais razoáveis, você pode manter uma precisão de 1 ms com relação ao UTC ou melhor para membros do domínio de atualização de aniversário do Windows Server 2016 e do Windows 10.
- **[Limite de suporte para ambientes de alta precisão](support-boundary.md).** Este artigo descreve os limites de suporte para o serviço de tempo do Windows (W32Time) em ambientes que exigem a hora do sistema altamente precisa e estável.
- **[Configuração de sistemas para alta precisão](configuring-systems-for-high-accuracy.md).** A sincronização de tempo no Windows 10 e no Windows Server 2016 foi substancialmente aprimorada.  Em condições operacionais razoáveis, os sistemas podem ser configurados para manter a precisão de 1 ms (milissegundo) ou melhor (em relação ao UTC).
- **[Tempo do Windows para rastreamento](windows-time-for-traceability.md).** As regulamentações em muitos setores exigem que os sistemas sejam rastreáveis para o UTC.  Isso significa que o deslocamento de um sistema pode ser atestado em relação ao UTC.  Para habilitar cenários de conformidade regulatória, o Windows 10 e o Server 2016 fornecem novos logs de eventos para fornecer uma imagem da perspectiva do sistema operacional para formar uma compreensão das ações executadas no relógio do sistema.  Esses logs de eventos são gerados continuamente para o serviço de tempo do Windows e podem ser examinados ou arquivados para análise posterior.
- **[Referência técnica do serviço de tempo do Windows](windows-time-service-tech-ref.md).** O serviço W32Time fornece sincronização de relógio de rede para computadores sem a necessidade de configuração extensiva. O serviço W32Time é essencial para a operação bem-sucedida da autenticação Kerberos V5 e, portanto, para a autenticação baseada em AD DS.
    - **[Como funciona o serviço de tempo do Windows](How-the-Windows-Time-Service-Works.md).** Embora o serviço de tempo do Windows não seja uma implementação exata do NTP (protocolo NTP), ele usa o complexo conjunto de algoritmos que é definido nas especificações de NTP para garantir que os relógios em computadores em toda a rede sejam os mais precisos possível.
    - **[Ferramentas e configurações de serviço de tempo do Windows](Windows-Time-Service-Tools-and-Settings.md).** A maioria dos computadores membros do domínio tem um tipo de cliente de tempo de NT5DS, o que significa que eles sincronizam o tempo da hierarquia de domínio. A única exceção comum a isso é o controlador de domínio que funciona como o mestre de operações do emulador do controlador de domínio primário (PDC) do domínio raiz da floresta, que geralmente é configurado para sincronizar a hora com uma fonte de tempo externa.


## <a name="related-topics"></a>Tópicos relacionados
Para obter mais informações sobre a hierarquia de domínio e o sistema de pontuação, consulte o ["o que é o serviço de tempo do Windows?"](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) .

O modelo de plug-in do provedor de tempo do Windows está [documentado no TechNet](https://msdn.microsoft.com/library/windows/desktop/ms725475%28v=vs.85%29.aspx).

Um adendo referenciado pelo artigo de tempo preciso do Windows 2016 pode ser baixado [aqui](https://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf)

Para obter uma visão geral rápida do serviço de tempo do Windows, veja este [vídeo de visão geral de alto nível](https://aka.ms/WS2016TimeVideo).
