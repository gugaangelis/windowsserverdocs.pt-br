---
title: Proteger a implantação de RDS – recuperação de desastre
description: Saiba mais sobre as opções de recuperação de desastre para os Serviços de Área de Trabalho Remota
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 9ff6a3b0-ea14-424e-9524-209252e9f1a8
author: lizap
ms.author: elizapo
ms.date: 06/12/2017
ms.openlocfilehash: 5c3257be5b3e9a53b8aa2400777f0519f8891843
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80861289"
---
# <a name="configure-disaster-recovery-for-remote-desktop-services"></a>Configurar a recuperação de desastres para os Serviços de Área de Trabalho Remota

Quando você implanta os Serviços de Área de Trabalho Remota em seu ambiente, ele se torna uma parte essencial de sua infraestrutura, especialmente os aplicativos e recursos que você compartilha com usuários. Se a implantação de RDS ficar inativa devido a qualquer coisa, seja uma falha de rede ou um desastre natural, usuários não conseguirão acessar esses aplicativos e recursos, o que afeta negativamente os seus negócios. Para evitar isso, você pode configurar uma solução de recuperação de desastre que permite executar um failover em sua implantação, ou seja, se a sua implantação do RDS estiver indisponível, seja qual for a razão, existe um backup disponível para assumir automaticamente o controle.

Para manter sua implantação do RDS em execução no caso de um único componente ou equipamento ficar inativo, é recomendável configurar a implantação de RDS para alta disponibilidade. Você pode fazer isso configurando um [farm RDSH](rds-scale-rdsh-farm.md) e garantindo que seus [Agentes de Conexão estão clusterizados para alta disponibilidade](rds-connection-broker-cluster.md). 

As soluções de recuperação de desastre que recomendamos aqui são para proteger sua implantação de um desastre grave, algo que desative a implantação de RDS inteira, incluindo funções redundantes configuradas para alta disponibilidade. Se tal desastre ocorrer, ter uma solução de recuperação de desastre incorporada à sua implantação permite que um failover ocorra em toda a implantação para você disponibilizar rapidamente para seus usuários os aplicativos e recursos de backup e em execução.

Use as informações a seguir para implantar soluções de recuperação de desastre em RDS:

- [Aproveitar vários data centers do Azure para garantir que os usuários possam acessar a sua implantação do RDS mesmo se um data center do Azure ficar inativo (redundância geográfica)](rds-multi-datacenter-deployment.md)
- [Implantar o Azure Site Recovery para fornecer um failover para componentes RDS em failovers de site a site ou de site para o Azure](rds-disaster-recovery-with-azure.md)


