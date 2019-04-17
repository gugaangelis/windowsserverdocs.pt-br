---
ms.assetid: e34622ff-b2d0-4f81-8d00-dacd5d6c215e
title: Serviço de tempo do Windows
description: ''
author: shortpatti
ms.author: pashort
manager: brianlic
ms.date: 02/01/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: b997d1f26e8da82e0d595b1ce13763e0a87d6d03
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="windows-time-service-w32time"></a>Serviço de tempo do Windows (W32Time)

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O serviço de tempo do Windows (W32Time) sincroniza a data e hora para todos os computadores que executam nos serviços de domínio do Active Directory (AD DS). Sincronização de tempo é essencial para o bom funcionamento muitos serviços do Windows e aplicativos de linha de negócios (LOB). O serviço de tempo do Windows usa o protocolo de tempo de rede (NTP) para sincronizar os relógios do computador na rede. NTP garante que um valor preciso de relógio, ou timestamp pode ser atribuído a solicitações de acesso de validação e recursos de rede.

O tópico de serviço de tempo do Windows (W32Time), o seguinte conteúdo está disponível:
- **[Tempo precisas Windows 2016](accurate-time.md).** Precisão de sincronização de hora no Windows Server 2016 foi aprimorado substancialmente, mantendo completo para trás compatibilidade NTP com versões mais antigas do Windows.  Em condições operacionais razoáveis, você pode manter um 1 ms precisão em relação ao UTC ou superior para Windows Server 2016 e atualização de aniversário do Windows 10 membros do domínio.
- **[Referência técnica do serviço de tempo do Windows ](windows-time-service-tech-ref.md).** O serviço W32Time fornece sincronização do relógio de rede para computadores sem a necessidade de configuração ampla. O serviço W32Time é essencial para a operação bem-sucedida de autenticação Kerberos V5 e, portanto, a autenticação AD DS.
    - **[Como funciona o serviço de tempo do Windows ](How-the-Windows-Time-Service-Works.md).** Embora o serviço de tempo do Windows não é uma implementação exata do protocolo de tempo de rede (NTP), ele usa o pacote complexo de algoritmos que é definido nas especificações NTP para garantir que os relógios em computadores em uma rede sejam mais precisos possível.
    - **[Ferramentas de serviço de tempo do Windows e configurações ](Windows-Time-Service-Tools-and-Settings.md).** A maioria dos computadores membros do domínio têm um tipo de cliente do tempo de NT5DS, o que significa que eles sincronizem o tempo desde a hierarquia de domínio. A exceção apenas típica é o controlador de domínio que funciona como o mestre de operações de emulador domínio primário (PDC) do controlador de domínio raiz da floresta, que geralmente é configurado para sincronizar o tempo com uma fonte de horário externo.

## <a name="related-topics"></a>Tópicos relacionados
Para obter mais informações sobre a hierarquia de domínio e o sistema de pontuação, consulte o ["Qual é o serviço de tempo do Windows"?](https://blogs.msdn.microsoft.com/w32time/2007/07/07/what-is-windows-time-service/) Postagem de blog.

Modelo de plug-in do provedor de tempo do windows é [documentado no TechNet](https://msdn.microsoft.com/en-us/library/windows/desktop/ms725475%28v=vs.85%29.aspx).

Um adendo referenciado pelo artigo do tempo de precisão de 2016 do Windows pode ser baixado [aqui](http://windocs.blob.core.windows.net/windocs/WindowsTimeSyncAccuracy_Addendum.pdf)

Para uma rápida visão geral do serviço de tempo do Windows, dê uma olhada neste [visão geral vídeo ](https://aka.ms/WS2016TimeVideo).

<!-- In this guide
In this guide:
Windows Accurate Time
High Accuracy
Support Boundary
Configuration for High Accuracy
Traceability for Compliance
Best Practices
Technical Reference
How the Windows Time Service Works
Windows Time Service Tools and Settings
-->

