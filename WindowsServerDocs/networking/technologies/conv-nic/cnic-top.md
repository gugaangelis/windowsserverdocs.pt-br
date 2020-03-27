---
title: Diretrizes de configuração da NIC (placa de interface de rede) convergida
description: A NIC (placa de interface de rede) convergida permite que você exponha RDMA por meio de uma NIC virtual de partição de host (vNIC) para que os serviços de partição de host possam acessar o RDMA (acesso remoto direto à memória) nas mesmas NICs que os convidados do Hyper-V estão usando para tráfego TCP/IP.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: d7642338-9b33-4dce-8100-8b2c38d7127a
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/13/2018
ms.openlocfilehash: 8824a6c6189a447f97f285052af8e5c13a66e766
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312813"
---
# <a name="converged-network-interface-card-nic-configuration-guidance"></a>Placa de interface de rede convergida \(as diretrizes de configuração\) NIC

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Placa de interface de rede convergida \(NIC\) permite expor RDMA por meio de um host\-partição virtual NIC \(vNIC\) para que os serviços de partição de host possam acessar o acesso remoto direto à memória \(RDMA\) nas mesmas NICs que os convidados do Hyper-V estão usando para tráfego TCP/IP.

Antes do recurso NIC convergido, o gerenciamento \(partição de host\) serviços que desejavam usar o RDMA eram necessários para usar NICs compatíveis com RDMA\-, mesmo que a largura de banda estivesse disponível nas NICs que foram associadas ao comutador virtual Hyper-V.

Com a NIC convergida, as duas cargas de trabalho \(usuários de gerenciamento de tráfego de RDMA e convidado\) podem compartilhar as mesmas NICs físicas, permitindo que você instale menos NICs em seus servidores.

Quando você implanta a NIC convergida com hosts Hyper-v do Windows Server 2016 e comutadores virtuais do Hyper-V, o vNICs nos hosts do Hyper-V expõe serviços RDMA para hospedar processos usando RDMA por meio de qualquer tecnologia RDMA baseada em\-Ethernet.

>[!NOTE]
>Para usar a tecnologia NIC convergida, os adaptadores de rede certificados em seus servidores devem dar suporte a RDMA.

Este guia fornece dois conjuntos de instruções, um para implantações em que os servidores têm um único adaptador de rede instalado, que é uma implantação básica da NIC convergida; e outro conjunto de instruções em que os servidores têm dois ou mais adaptadores de rede instalados, que é uma implantação de NIC convergida em um switch incorporado \(conjunto de\) equipe de adaptadores de rede com capacidade RDMA\-.


## <a name="prerequisites"></a>{1&gt;{2&gt;Pré-requisitos&lt;2}&lt;1}

Veja a seguir os pré-requisitos para as implantações básica e de datacenter da NIC convergida.

>[!NOTE]
>Para os exemplos fornecidos, usamos um adaptador Ethernet Mellanox ConnectX-3 pro 40 Gbps, mas você pode usar qualquer um dos adaptadores de rede compatíveis com Windows Server Certified RDMA\-que dão suporte a esse recurso. Para obter mais informações sobre adaptadores de rede compatíveis, consulte os [cartões de LAN](https://www.windowsservercatalog.com/results.aspx?&bCatID=1468&cpID=0&avc=85&ava=0&avt=0&avq=46&OR=1)do tópico do catálogo do Windows Server.

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
- Você deve estar familiarizado com o switch Embedded Integration \(conjunto\), uma solução de agrupamento NIC alternativa usada em ambientes que incluem o Hyper-V e a pilha de SDN (rede definida pelo software) no Windows Server 2016. O conjunto integra algumas funcionalidades de agrupamento NIC ao comutador virtual Hyper-V. Para obter mais informações, consulte [acesso remoto direto à memória (RDMA) e Comutador incorporado (Set)](../../../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).

## <a name="related-topics"></a>Tópicos relacionados
- [Configuração de NIC convergida com um único adaptador de rede](cnic-single.md)
- [Configuração NIC agrupada NIC convergida](cnic-datacenter.md)
- [Configuração de comutador físico para NIC convergida](cnic-app-switch-config.md)
- [Solucionando problemas de configurações de NIC convergida](cnic-app-troubleshoot.md)

---