---
title: O que há de novo nos serviços de área de trabalho remota
description: Fornece a descrição dos novos recursos RDS no Windows Server 2016.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 10/11/2016
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 04d52dff-e61b-4633-9908-be8600abc2ba
author: ChristianMontoya
manager: scottman
ms.openlocfilehash: ad13fdce251c1f84bac725e9f1ee266c6aae5e13
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816307"
---
# <a name="whats-new-in-remote-desktop-services"></a>O que há de novo nos serviços de área de trabalho remota

Serviços de área de trabalho remota (RDS) criados no Windows Server 2016 é uma plataforma de virtualização, permitindo uma ampla variedade de cenários de clientes. Aprimoramentos em toda a solução RDS incorpora o trabalho feito pela equipe de área de trabalho remota e outros parceiros de tecnologia da Microsoft. Os cenários e as tecnologias a seguir são novos ou aprimorados no Windows Server 2016.

Certifique-se também fazer check-out de nossa sessão do Ignite 2016: [Aproveite as melhorias de RDS no Windows Server 2016](https://channel9.msdn.com/Events/Ignite/2016/BRK3098). Neste vídeo, a equipe de produto examina todos os recursos novos e aprimorados no serviços de área de trabalho remota, incluindo suporte a vGPU. 

## <a name="app-compatibility---windows-server-2016-and-windows-10"></a>Compatibilidade de aplicativos - Windows Server 2016 e Windows 10
Compilado na mesma base do Windows 10, Windows Server 2016 não só tem a mesma aparência você espera de uma área de trabalho, mas também pode executar muitos dos mesmos aplicativos. Emparelhamento do Windows Server 2016 com os recursos de gráficos (abaixo) fornece um ambiente para todos os usuários sejam produtivos. 

## <a name="azure-sql-database---the-new-database-for-your-highly-available-environment"></a>Azure SQL Database - o novo banco de dados para o seu ambiente altamente disponível
O agente de Conexão de área de trabalho remota é capaz de armazenar todas as informações de implantação (como os estados de conexão e mapeamentos de usuário/host) em um SQL banco de dados compartilhado, como um banco de dados SQL do Azure. Eliminar o manual de implantação do SQL Server sempre no grupo de disponibilidade, obtenha a cadeia de caracteres de conexão ao banco de dados SQL do Azure e começar a usar o seu ambiente altamente disponível.

Informações adicionais: [Use o BD SQL do Azure para o seu ambiente de alta disponibilidade do agente de Conexão de área de trabalho remota](https://blogs.technet.microsoft.com/enterprisemobility/2016/05/03/new-windows-server-2016-capability-use-azure-sql-db-for-your-remote-desktop-connection-broker-high-availability-environment/)

## <a name="graphics---solving-graphics-needs-across-various-scenarios"></a>Gráficos - resolver necessidades de elementos gráficos em vários cenários
Graças à atribuição de dispositivo discretos do Hyper-V, agora você pode mapear GPUs em uma máquina de host diretamente a uma VM para ser consumida por seus aplicativos que exigem GPU. Também foram feitas melhorias no vGPU do RemoteFX, incluindo suporte a OpenGL 4.4, OpenCL 1.1, resolução de k de 4 e máquinas virtuais do Windows Server.

Informações adicionais: [Atribuição de dispositivo discretos](https://blogs.technet.microsoft.com/virtualization/2015/11/)

## <a name="rd-connection-broker---improved-connection-handling-during-logon-storms"></a>Agente de Conexão de área de trabalho - conexão aprimorada manipulando durante tempestades de logon
Com a manipulação aprimorada de conexão, o agente de Conexão de área de trabalho agora é capaz de lidar com mais de 10.000 solicitações simultâneas de logon, às vezes, vistas durante "tempestades de logon". O agente de Conexão de área de trabalho aprimorado também torna a manutenção da implantação mais simples ao mais rapidamente adicionar servidores de volta para o ambiente.

Informações adicionais: [Melhor desempenho do agente de Conexão de área de trabalho remota](https://blogs.technet.microsoft.com/enterprisemobility/2015/12/15/improved-remote-desktop-connection-broker-performance-with-windows-server-2016-and-windows-server-2012-r2-hotfix-kb3091411/)

## <a name="rdp-10---new-capabilities-built-into-the-protocol"></a>RDP 10 - novos recursos incorporados ao protocolo
RDP 10 agora usa o codec H.264/AVC 444, otimizando adequadamente em vídeo e texto. Com esta versão, também há suporte para comunicação remota de caneta. Com essas capacidades, suas sessões remotas começaria a ficar ainda mais como uma sessão local.  

Informações adicionais: [Melhorias de RDP 10 AVC/H.264 no Windows 10 e Windows Server 2016](https://blogs.technet.microsoft.com/enterprisemobility/2016/01/11/remote-desktop-protocol-rdp-10-avch-264-improvements-in-windows-10-and-windows-server-2016-technical-preview/)

## <a name="personal-session-desktops---providing-individual-desktops-to-any-end-user"></a>Áreas de trabalho de sessão pessoal - fornecer áreas de trabalho individuais para qualquer usuário final
Áreas de trabalho de sessão pessoal é uma nova maneira de ter sua própria área de trabalho pessoal hospedada por você na nuvem. Privilégios administrativos e remove de hosts de sessão dedicado a complexidade da hospedagem de ambientes em que os usuários desejam gerenciar a área de trabalho, como ele é seu próprio.

Informações adicionais: [Áreas de trabalho de sessão pessoal](rds-personal-session-desktops.md)
