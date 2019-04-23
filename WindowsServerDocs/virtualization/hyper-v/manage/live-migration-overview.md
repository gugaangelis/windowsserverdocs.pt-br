---
title: Visão geral de migração ao vivo
description: Fornece a visão geral da funcionalidade de migração ao vivo no Windows Server 2016.
ms.prod: windows-server-threshold
ms.service: na
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5cc875ab-05c4-439e-b27d-6bfc77054660
author: johncslack
ms.author: joslack
ms.date: 06/27/2017
ms.openlocfilehash: 2bbe897ffb8b200a72fac5a662e518d4a4be1131
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887847"
---
# <a name="live-migration-overview"></a>Visão geral de migração ao vivo

Migração ao vivo é um recurso do Hyper-V no Windows Server.  Ele permite que você mover de maneira transparente máquinas virtuais em execução de um host Hyper-V para outro sem tempo de inatividade perceptível.  O principal benefício de migração ao vivo é a flexibilidade; Máquinas virtuais em execução não estão vinculados a um único computador host.  Isso permite que as ações como descarregar um host específico de máquinas virtuais antes de encerrar ou atualizá-lo.  Quando combinado com o Clustering de Failover do Windows, migração ao vivo permite a criação de altamente disponível e falhas sistemas tolerantes a falhas. 

## <a name="related-technologies-and-documentation"></a>Documentação e as tecnologias relacionadas

Migração ao vivo é geralmente usada em conjunto com algumas tecnologias relacionadas, como o Clustering de Failover e o System Center Virtual Machine Manager.  Se você estiver usando a migração ao vivo por meio dessas tecnologias, aqui estão os ponteiros para a documentação mais recente:
* [Clustering de failover](../../../failover-clustering/failover-clustering-overview.md) (Windows Server 2016) 
* [System Center Virtual Machine Manager](https://docs.microsoft.com/system-center/vmm/) (System Center 2016) 

Se você estiver usando versões mais antigas do Windows Server, ou precisa de detalhes sobre os recursos introduzidos em versões mais antigas do Windows Server, aqui são ponteiros para a documentação de histórico: 
* [Migração ao vivo](https://technet.microsoft.com/library/ee815293(v=ws.10).aspx) (Windows Server 2008 R2)  
* [Migração ao vivo](https://technet.microsoft.com/library/hh831435(v=ws.11).aspx) (Windows Server 2012 R2) 
* [Clustering de failover](https://technet.microsoft.com/library/hh831579(v=ws.11).aspx) (Windows Server 2012 R2)
* [Clustering de failover](https://technet.microsoft.com/library/ff182338(v=ws.10).aspx) (Windows Server 2008 R2)
* [System Center Virtual Machine Manager](https://technet.microsoft.com/library/gg610610.aspx) (System Center 2012 R2)
* [System Center Virtual Machine Manager](https://technet.microsoft.com/library/cc917964.aspx) (System Center 2008 R2)

## <a name="live-migration-in-windows-server-2016"></a>Migração ao vivo no Windows Server 2016

No Windows Server 2016, há menos restrições na implantação da migração ao vivo.  Agora, ele funciona sem Clustering de Failover.  Outra funcionalidade permanece inalterada em relação às versões anteriores da migração ao vivo.  Para obter mais detalhes sobre como configurar e usar migração ao vivo sem Clustering de Failover: 
* [Configurar hosts para migração ao vivo sem Clustering de Failover](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md)
* [Use a migração ao vivo sem Clustering de Failover para mover uma máquina virtual](use-live-migration-without-failover-clustering-to-move-a-virtual-machine.md)