---
title: Diretrizes de configuração da NIC (placa de interface de rede) convergida
description: A NIC (placa de interface de rede) convergida permite que você exponha RDMA por meio de uma NIC virtual de partição de host (vNIC) para que os serviços de partição de host possam acessar o RDMA (acesso remoto direto à memória) nas mesmas NICs que os convidados do Hyper-V estão usando para tráfego TCP/IP.
ms.topic: article
ms.assetid: d7642338-9b33-4dce-8100-8b2c38d7127a
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/13/2018
ms.openlocfilehash: 2b077b911d3721907e70b198c62970aafe25e58d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87944750"
---
# <a name="converged-network-interface-card-nic-configuration-guidance"></a>\(Diretrizes de configuração da NIC da placa de interface de rede convergida \)

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

NIC de placa de interface de rede convergida \( \) permite expor RDMA por meio de uma \- vNIC NIC virtual de partição de host \( \) para que os serviços de partição de host possam acessar o RDMA de acesso remoto direto à memória \( \) nas mesmas NICs que os convidados do Hyper-V estão usando para tráfego TCP/IP.

Antes do recurso NIC convergido, os serviços de partição de host de gerenciamento \( \) que desejavam usar o RDMA eram necessários para usar NICs compatíveis com RDMA dedicados \- , mesmo que a largura de banda estivesse disponível nas NICs que estavam associadas ao comutador virtual Hyper-V.

Com a NIC convergida, as duas cargas de trabalho \( gerenciam os usuários de tráfego de RDMA e convidado \) podem compartilhar as mesmas NICs físicas, permitindo que você instale menos NICs em seus servidores.

Quando você implanta a NIC convergida com hosts Hyper-v do Windows Server 2016 e comutadores virtuais do Hyper-V, o vNICs nos hosts Hyper-V expõe serviços RDMA para hospedar processos usando RDMA em qualquer \- tecnologia RDMA baseada em Ethernet.

>[!NOTE]
>Para usar a tecnologia NIC convergida, os adaptadores de rede certificados em seus servidores devem dar suporte a RDMA.

Este guia fornece dois conjuntos de instruções, um para implantações em que os servidores têm um único adaptador de rede instalado, que é uma implantação básica da NIC convergida; e outro conjunto de instruções em que os servidores têm dois ou mais adaptadores de rede instalados, que é uma implantação de NIC convergida em uma \( equipe do conjunto de agrupamentos integrado de comutador \) de \- adaptadores de rede habilitado para RDMA.


## <a name="prerequisites"></a>Pré-requisitos

Veja a seguir os pré-requisitos para as implantações básica e de datacenter da NIC convergida.

>[!NOTE]
>Para os exemplos fornecidos, usamos um adaptador Ethernet Mellanox ConnectX-3 pro 40 Gbps, mas você pode usar qualquer um dos adaptadores de rede compatíveis com o Windows Server Certified RDMA \- que dão suporte a esse recurso. Para obter mais informações sobre adaptadores de rede compatíveis, consulte os [cartões de LAN](https://www.windowsservercatalog.com/results.aspx?&bCatID=1468&cpID=0&avc=85&ava=0&avt=0&avq=46&OR=1)do tópico do catálogo do Windows Server.

### <a name="basic-converged-nic-prerequisites"></a>Pré-requisitos básicos da NIC convergida

Para executar as etapas deste guia para a configuração básica da NIC convergida, você deve ter o seguinte.

- Dois servidores que executam o Windows Server 2016 Datacenter Edition ou o Windows Server 2016 Standard Edition.
- Um adaptador de rede com capacidade RDMA e certificado instalado em cada servidor.
- Função de servidor Hyper-V instalada em cada servidor.

### <a name="datacenter-converged-nic-prerequisites"></a>Pré-requisitos de NIC convergida do datacenter

Para executar as etapas deste guia para a configuração de NIC convergida do datacenter, você deve ter o seguinte.

- Dois servidores que executam o Windows Server 2016 Datacenter Edition ou o Windows Server 2016 Standard Edition.
- Dois adaptadores de rede com capacidade RDMA e certificados instalados em cada servidor.
- Função de servidor Hyper-V instalada em cada servidor.
- Você deve estar familiarizado com o conjunto de agrupamentos incorporados \( \) de switch, uma solução de agrupamento NIC alternativa usada em ambientes que incluem o Hyper-V e a pilha Sdn (rede definida pelo software) no Windows Server 2016. O conjunto integra algumas funcionalidades de agrupamento NIC ao comutador virtual Hyper-V. Para obter mais informações, consulte [acesso remoto direto à memória (RDMA) e Comutador incorporado (Set)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

## <a name="related-topics"></a>Tópicos relacionados
- [Configuração de NIC convergida com um único adaptador de rede](cnic-single.md)
- [Configuração NIC agrupada NIC convergida](cnic-datacenter.md)
- [Configuração de comutador físico para NIC convergida](cnic-app-switch-config.md)
- [Solucionando problemas de configurações de NIC convergida](cnic-app-troubleshoot.md)

---