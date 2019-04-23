---
title: Diretrizes de configuração de placa de Interface de rede (NIC) convergido
description: Placa de interface de rede convergido (NIC) permite que você exponha RDMA por meio de uma NIC de partição de host virtual (vNIC) para que os serviços de partição de host podem acessar direto memória RDMA (acesso remoto) nas NICs mesmos que os convidados do Hyper-V estão usando para o tráfego de TCP/IP.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: d7642338-9b33-4dce-8100-8b2c38d7127a
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/13/2018
ms.openlocfilehash: e9f5180285dda790e11cec543a109d0cb58edd2d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838837"
---
# <a name="converged-network-interface-card-nic-configuration-guidance"></a>Convergido Network Interface Card \(NIC\) diretrizes de configuração

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Placa de interface de rede convergida \(NIC\) permite que você exponha RDMA através de um host\-NIC virtual de partição \(vNIC\) para que os serviços de partição de host podem acessar remotos acesso direto à memória \(RDMA\) nas NICs mesmos que os convidados do Hyper-V estão usando para o tráfego de TCP/IP.

Antes do recurso de NIC convergida, gerenciamento \(hospedam partição\) os serviços que quisessem usar RDMA foram necessários para usar RDMA dedicada\-NICs compatíveis com, mesmo se a largura de banda estava disponível nas NICs que estavam vinculados a Hyper-V Comutador virtual.

Com uma NIC convergida, duas cargas de trabalho \(usuários do gerenciamento de tráfego RDMA e o convidado\) podem compartilhar os mesmos NICs físicas, permitindo que você instale menos NICs em seus servidores.

Quando você implanta o NIC convergida com hosts Windows Server 2016 Hyper-V e comutadores virtuais do Hyper-V, os vNICs nos hosts Hyper-V expor serviços RDMA para processos do host usando o RDMA sobre qualquer Ethernet\-com base em tecnologia RDMA.

>[!NOTE]
>Para usar a tecnologia de NIC convergida, os adaptadores de rede certificadas em seus servidores devem oferecer suporte a RDMA.

Este guia fornece dois conjuntos de instruções, um para implantações em que seus servidores têm um único adaptador de rede instalado, que é uma implantação básica do adaptador de rede convergido; e outro conjunto de instruções em que seus servidores têm dois ou mais adaptadores de rede instalados, que é uma implantação do adaptador de rede convergido ao longo de um agrupamento incorporado do comutador \(definir\) equipe de RDMA\-adaptadores de rede compatíveis.


## <a name="prerequisites"></a>Pré-requisitos

A seguir estão os pré-requisitos para as implantações Basic e do Datacenter da placa de rede convergida.

>[!NOTE]
>Para os exemplos fornecidos, podemos usar um adaptador de Ethernet Gbps 40 Pro Mellanox ConnectX-3, mas você pode usar qualquer servidor do Microsoft certified RDMA\-adaptadores de rede compatíveis que dão suporte a esse recurso. Para obter mais informações sobre os adaptadores de rede compatíveis, consulte o tópico do Windows Server Catalog [placas LAN](https://www.windowsservercatalog.com/results.aspx?&bCatID=1468&cpID=0&avc=85&ava=0&avt=0&avq=46&OR=1).

### <a name="basic-converged-nic-prerequisites"></a>Pré-requisitos de NIC convergida básicos

Para executar as etapas neste guia para a configuração de NIC convergida básica, você deve ter o seguinte.

- Dois servidores que executam o Windows Server 2016 Datacenter edition ou Windows Server 2016 Standard edition.
- Um compatíveis com RDMA, adaptador de rede instalado em cada servidor de certificados.
- Função de servidor Hyper-V instalada em cada servidor.

### <a name="datacenter-converged-nic-prerequisites"></a>Pré-requisitos do data center convergido NIC

Para executar as etapas neste guia para configuração do data center convergido NIC, você deve ter o seguinte.

- Dois servidores que executam o Windows Server 2016 Datacenter edition ou Windows Server 2016 Standard edition.
- Dois compatíveis com RDMA, certified adaptadores de rede instalados em cada servidor.
- Função de servidor Hyper-V instalada em cada servidor.
- Você deve estar familiarizado com o agrupamento incorporado do comutador \(definir\), uma solução usada em ambientes que incluem o Hyper-V e a pilha de Software Defined Networking (SDN) no Windows Server 2016 de agrupamento de NIC alternativo. CONJUNTO integra algumas funcionalidades de agrupamento NIC no comutador Virtual do Hyper-V. Para obter mais informações, consulte [acesso direto à memória remoto (RDMA) e Switch Embedded Teaming (SET)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

## <a name="related-topics"></a>Tópicos relacionados
- [Configuração de NIC convergida com um único adaptador de rede](cnic-single.md)
- [Configuração do NIC agrupado NIC convergida](cnic-datacenter.md)
- [Configuração de comutador físico para NIC convergida](cnic-app-switch-config.md)
- [Solução de problemas convergido configurações de NIC](cnic-app-troubleshoot.md)

---