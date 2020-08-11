---
title: Novidades nos Serviços de Área de Trabalho Remota
description: Fornece a descrição dos novos recursos RDS no Windows Server 2016.
ms.author: chrimo
ms.date: 10/11/2016
ms.topic: article
ms.assetid: 04d52dff-e61b-4633-9908-be8600abc2ba
author: ChristianMontoya
manager: scottman
ms.openlocfilehash: 2048b78e17b4888492e252fb4b5454f9428a5e34
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87961590"
---
# <a name="whats-new-in-remote-desktop-services"></a>Novidades nos Serviços de Área de Trabalho Remota

O RDS (Serviços de Área de Trabalho Remota) criado no Windows Server 2016 é uma plataforma de virtualização que permite uma ampla variedade de cenários de clientes. As melhorias em toda a solução RDS incorporam o trabalho feito pela equipe de Área de Trabalho Remota e outros parceiros de tecnologia da Microsoft. Os cenários e as tecnologias a seguir são novos ou aprimorados no Windows Server 2016.

Confira também nossa sessão do Ignite 2016: [Aproveite as melhorias de RDS no Windows Server 2016](https://channel9.msdn.com/Events/Ignite/2016/BRK3098). Neste vídeo, a equipe de produto examina todos os recursos novos e aprimorados nos Serviços de Área de Trabalho Remota, incluindo suporte a vGPU.

## <a name="app-compatibility---windows-server-2016-and-windows-10"></a>Compatibilidade dos aplicativos – Windows Server 2016 e Windows 10
Compilado na mesma base do Windows 10, o Windows Server 2016 não só tem a mesma aparência que você espera de uma área de trabalho, mas também pode executar muitos dos mesmos aplicativos. O emparelhamento do Windows Server 2016 com os recursos gráficos (abaixo) fornece um ambiente para que todos os usuários sejam produtivos.

## <a name="azure-sql-database---the-new-database-for-your-highly-available-environment"></a>Banco de Dados SQL do Azure – o novo banco de dados para o seu ambiente altamente disponível
O Agente de Conexão de Área de Trabalho Remota é capaz de armazenar todas as informações de implantação (como os estados de conexão e mapeamentos de usuário/host) em um banco de dados SQL compartilhado, como um Banco de Dados SQL do Azure. Elimine o manual de implantação do Grupo de Disponibilidade Always On do SQL Server, obtenha a cadeia de conexão para o Banco de Dados SQL do Azure e comece a usar o seu ambiente altamente disponível.

Informações adicionais: [Usar o banco de dados SQL do Azure para seu ambiente de alta disponibilidade do Agente de Conexão de Área de Trabalho Remota](https://techcommunity.microsoft.com/t5/microsoft-security-and/new-windows-server-2016-capability-use-azure-sql-db-for-your/ba-p/249787)

## <a name="graphics---solving-graphics-needs-across-various-scenarios"></a>Gráficos – resolver necessidades de elementos gráficos em vários cenários
Graças à Atribuição de Dispositivos Discretos do Hyper-V, agora você pode mapear GPUs em um computador host diretamente para uma VM para ser consumida por seus aplicativos que exigem GPU. Também foram realizadas melhorias no vGPU do RemoteFX, incluindo suporte a OpenGL 4.4, OpenCL 1.1, resolução de 4K e máquinas virtuais do Windows Server.

Informações adicionais: [Atribuição de dispositivo discreto](https://techcommunity.microsoft.com/t5/virtualization/bg-p/Virtualization)

## <a name="rd-connection-broker---improved-connection-handling-during-logon-storms"></a>Agente de Conexão de Área de Trabalho Remota – manipulação aprimorada de conexão durante tempestades de logon
Com a manipulação aprimorada de conexão, o Agente de Conexão de Área de Trabalho Remota agora é capaz de lidar com mais de 10.000 solicitações simultâneas de logon, às vezes, observadas durante "tempestades de logon". O Agente de Conexão de Área de Trabalho Remota aprimorado também simplifica a manutenção da implantação ao adicionar mais rapidamente servidores de volta para o ambiente.

Informações adicionais: [Desempenho aprimorado do Agente de Conexão de Área de Trabalho Remota](https://techcommunity.microsoft.com/t5/microsoft-security-and/improved-remote-desktop-connection-broker-performance-with/ba-p/249559)

## <a name="rdp-10---new-capabilities-built-into-the-protocol"></a>RDP 10 – novos recursos incorporados ao protocolo
Agora o RDP 10 usa o codec H.264/AVC 444, otimizando adequadamente em vídeo e texto. Com essa versão, também há suporte para comunicação remota por caneta. Com essas capacidades, suas sessões remotas começam a se parecer ainda mais com uma sessão local.

Informações adicionais: [Melhorias de RDP 10 AVC/H.264 no Windows 10 e Windows Server 2016](https://techcommunity.microsoft.com/t5/microsoft-security-and/remote-desktop-protocol-rdp-10-avc-h-264-improvements-in-windows/ba-p/249588)

## <a name="personal-session-desktops---providing-individual-desktops-to-any-end-user"></a>Áreas de trabalho de sessão pessoal – fornecem áreas de trabalho individuais para qualquer usuário final
As áreas de trabalho de sessão pessoal são uma nova maneira de ter sua própria área de trabalho pessoal hospedada por você na nuvem. Os privilégios administrativos e os hosts da sessão dedicados eliminam a complexidade dos ambientes de hospedagem em que os usuários desejam gerenciar a área de trabalho como se ela fosse deles.

Informações adicionais: [Áreas de trabalho de sessão pessoal](rds-personal-session-desktops.md)
