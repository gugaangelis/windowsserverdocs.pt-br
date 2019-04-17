---
title: Guia de configuração do cartão (NIC) de Interface de Network convergido
description: Este tópico faz parte do convergidos NIC configuração guia para Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d7642338-9b33-4dce-8100-8b2c38d7127a
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 42f7ef674da1754253ad72ae30ad505f29ba6a7d
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="converged-network-interface-card-nic-configuration-guide"></a>Guia de configuração rede convergida Interface cartão \(NIC\)

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Placa de rede convergida \(NIC\) permite que você exponha RDMA por meio de uma partição host\ virtual NIC \(vNIC\) para que os serviços de partição de host podem acessar \(RDMA\) remoto acesso direto à memória nas NICs mesmo que os convidados Hyper-V estão usando para o tráfego de TCP/IP.

Antes do recurso de NIC convergidos, serviços de gerenciamento de \(host partition\) que desejavam usar RDMA foram necessária para usar NICs compatíveis com RDMA\ dedicadas, mesmo que a largura de banda estava disponível nas NICs que fossem vinculados ao Switch Virtual Hyper-V.

Com NIC convergidos, as cargas de dois trabalho \ (usuários de gerenciamento de traffic\ RDMA e convidados) podem compartilhar o placas NIC físicas mesmo, permitindo que você instale NICs menos em seus servidores.

Quando você implanta NIC convergidos com hosts do Windows Server 2016 Hyper-V e comutadores virtuais do Hyper-V, vNICs os hosts Hyper-V expor serviços RDMA para processos de host usando RDMA sobre em qualquer tecnologia baseada em Ethernet\ RDMA.

>[!NOTE]
>Para usar a tecnologia de NIC convergidos, os adaptadores de rede certificados em seus servidores devem dar suporte a RDMA.

Este guia fornece dois conjuntos de instruções, um para implantações onde seus servidores tem um adaptador de rede instalado, que é uma implementação básica de NIC convergidos; e outro conjunto de instruções onde seus servidores tem dois ou mais adaptadores de rede instalados, que é uma implantação do NIC convergidos sobre uma equipe Switch Embedded agrupamento \(SET\) RDMA\ capaz de adaptadores de rede.

Este guia contém os tópicos a seguir.

- [Configuração de NIC convergido com um único adaptador de rede](cnic-single.md)
- [Configuração de NIC emparelhados NIC convergido](cnic-datacenter.md)
- [Configuração do comutador físico para NIC convergido](cnic-app-switch-config.md)
- [Solução de problemas de configurações de NIC convergidos](cnic-app-troubleshoot.md)

## <a name="prerequisites"></a>Pré-requisitos

A seguir é os pré-requisitos para as implantações básico e Datacenter de NIC convergida.

>[!NOTE]
>Para os exemplos neste guia, um adaptador de Ethernet Gbps 40 Pro 3 Mellanox ConnectX é usado, mas você pode usar qualquer do Windows Server certificados compatíveis com RDMA\ adaptadores de rede que suportam esse recurso. Para saber mais sobre os adaptadores de rede compatíveis, consulte o tópico de catálogo do Windows Server [cartões de LAN](https://www.windowsservercatalog.com/results.aspx?&bCatID=1468&cpID=0&avc=85&ava=0&avt=0&avq=46&OR=1).

### <a name="basic-converged-nic-prerequisites"></a>Pré-requisitos do NIC convergidos básico

Para executar as etapas neste guia para configuração básica do NIC convergidos, você deve ter o seguinte.

- Dois servidores que executam o Windows Server 2016 Datacenter edition ou Windows Server 2016 Standard edition.
- Um RDMA compatível com, certificados instalado em cada servidor de adaptador de rede.
- A função de servidor do Hyper-V deve ser instalada em cada servidor.

### <a name="datacenter-converged-nic-prerequisites"></a>Pré-requisitos de Datacenter convergidos NIC

Para executar as etapas neste guia para a configuração de NIC convergidos datacenter, você deve ter o seguinte.

- Dois servidores que executam o Windows Server 2016 Datacenter edition ou Windows Server 2016 Standard edition.
- Dois RDMA compatível com, certificados instalados em cada servidor de adaptadores de rede.
- A função de servidor do Hyper-V deve ser instalada em cada servidor.
- Você deve estar familiarizado com o comutador Embedded agrupamento \(SET\), que é uma alternativa solução NIC agrupamento que você pode usar em ambientes que incluem o Hyper-V e a pilha de rede definidos Software (SDN) no Windows Server 2016. CONJUNTO integra algumas funcionalidades de agrupamento de NIC o Hyper-V Virtual Switch. Para obter mais informações, consulte [acesso direto de memória remoto (RDMA) e agrupamento Embedded Switch (conjunto)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

