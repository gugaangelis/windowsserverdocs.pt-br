---
title: Desempenho ruim ou problemas com o aplicativo durante a conexão de área de trabalho remota
description: Solucionar problemas de desempenho ruim ou com o aplicativo durante a conexão de área de trabalho remota.
ms.reviewer: rklemen
ms.topic: troubleshooting
author: kaushika-msft
manager: dcscontentpm
ms.author: delhan
ms.date: 07/24/2019
ms.localizationpriority: medium
ms.openlocfilehash: 91ced9e729966ee9c46e76d01d7ccbec9a510f5b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857219"
---
# <a name="poor-performance-or-application-problems-during-remote-desktop-connection"></a>Desempenho ruim ou problemas com o aplicativo durante a conexão de área de trabalho remota

Este artigo aborda vários problemas comuns que os usuários podem enfrentar ao usarem a funcionalidade da área de trabalho remota.

### <a name="intermittent-problems-with-new-microsoft-azure-virtual-machines"></a>Problemas intermitentes com novas máquinas de virtuais do Microsoft Azure

Esse problema afeta as máquinas virtuais que foram provisionadas recentemente. Depois que o usuário se conecta à máquina virtual, a sessão de área de trabalho remota não carrega todas as configurações do usuário corretamente.

Para contornar esse problema, desconecte a máquina virtual, aguarde pelo menos 20 minutos e, em seguida, conecte-se novamente.

Para resolver esse problema, aplique as atualizações a seguir às máquinas virtuais, conforme apropriado:

  - Windows 10 e Windows Server 2016: KB 4343884, [30 de agosto de 2018 – KB4343884 (Build do sistema operacional 14393.2457)](https://support.microsoft.com/help/4343884/windows-10-update-kb4343884)
  - Windows Server 2012 R2: KB 4343891, [30 de agosto de 2018 – KB4343891 (versão prévia do pacote cumulativo de atualizações mensal)](https://support.microsoft.com/help/4343891/windows-81-update-kb4343891)

### <a name="video-playback-issues-on-windows-10-version-1709"></a>Problemas de reprodução de vídeo no Windows 10 versão 1709

Esse problema ocorre quando os usuários se conectam a computadores remotos que executam o Windows 10, versão 1709. Quando esses usuários reproduzem vídeo usando o codec VMR9 (Video Mixing Renderer 9), o player mostra apenas uma janela preta.

Esse é um problema conhecido no Windows 10, versão 1709. O problema não ocorre no Windows 10 versão 1703.

### <a name="desktop-sharing-issues-on-windows-10"></a>Problemas de compartilhamento de área de trabalho no Windows 10

Esse problema ocorre quando o usuário tem um perfil do usuário somente leitura (e o hive do Registro associado), assim como em um cenário de quiosque. Quando esse usuário se conecta a um computador remoto que está executando o Windows 10 versão 1803, ele não consegue compartilhar sua área de trabalho.

Para corrigir esse problema, aplique a atualização 4340917 do Windows 10, de [24 de julho de 2018 – KB4340917 (build do sistema operacional 17134.191)](https://support.microsoft.com/help/4340917/windows-10-update-kb4340917).

### <a name="performance-issues-when-mixing-versions-of-windows-10-if-nla-is-disabled"></a>Problemas de desempenho ao misturar versões do Windows 10 se o NLA está desabilitado

Esse problema ocorre quando os computadores cliente da Área de Trabalho Remota que executam o Windows 10 se conectam a áreas de trabalho remotas que executam versões diferentes do Windows 10 enquanto o NLA está desabilitado. Usuários de clientes da Área de Trabalho Remota em computadores que executam o Windows 10, versão 1709 ou anteriores, enfrentam um desempenho ruim quando se conectam a áreas de trabalho remotas que executam o Windows 10, versão 1803 ou posterior.

Isso ocorre porque quando o NLA está desabilitado, os computadores cliente mais antigos usam um protocolo mais lento ao se conectarem ao Windows 10 versão 1803 ou posterior.

Para resolver esse problema, aplique o KB 4340917 de [24 de julho de 2018 – KB4340917 (Build do sistema operacional 17134.191)](https://support.microsoft.com/help/4340917/windows-10-update-kb4340917).

### <a name="black-screen-issue"></a>Problema de tela preta

Esse problema ocorre no Windows 8.0, Windows 8.1, Windows 10 RTM e Windows Server 2012 R2. Um usuário inicia vários aplicativos em uma Área de Trabalho Remota e, em seguida, desconecta-se da sessão. Periodicamente, o usuário reconecta-se à área de trabalho remota para interagir com os aplicativos e, em seguida, desconecta-se novamente. Em algum momento, quando o usuário se reconecta, a sessão de área de trabalho remota mostra apenas uma tela preta. Para que a sessão seja exibida corretamente mais uma vez, o usuário precisa encerrar sua sessão do console do computador remoto ou do console do servidor RDSH e parar os aplicativos de sua sessão.

Para resolver esse problema, aplique as seguintes atualizações conforme apropriado:

  - Windows 8 e Windows Server 2012: KB4103719, [17 de maio de 2018 – KB4103719 (versão prévia do pacote cumulativo de atualizações mensal)](https://support.microsoft.com/help/4103719/windows-server-2012-update-kb4103719)
  - Windows 8.1 e Windows Server 2012 R2: KB 4103724, [17 de maio de 2018 – KB4103724 (versão prévia do pacote cumulativo de atualizações mensal)](https://support.microsoft.com/help/4103724/windows-81-update-kb4103724) e KB 4284863, [21 de junho de 2018 – KB4284863 (versão prévia do pacote cumulativo de atualizações mensal)](https://support.microsoft.com/help/4284863/windows-81-update-kb4284863)
  - Windows 10: Corrigido no KB4284860, [12 de junho de 2018 – KB4284860 (Build do sistema operacional 10240.17889)](https://support.microsoft.com/help/4284860/windows-10-update-kb4284860)
