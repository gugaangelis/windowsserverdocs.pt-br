---
title: Virtual Receive Side Scaling (vRSS)
description: Saiba mais sobre Virtual Receive Side Scaling (vRSS) no Windows Server e como configurar um adaptador de rede virtual para balancear o tráfego de rede de carga entre vários núcleos de processador lógico em uma VM. Você também pode configurar múltiplos de núcleos físicos para um host de placa de Interface de rede virtual (vNIC).
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 9be477b3-f81d-4e84-a6b0-ac4c1ea97715
ms.date: 09/05/2018
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0c1cb11cb8ce69463a31cfa5061290f79d8dda91
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875227"
---
# <a name="virtual-receive-side-scaling-vrss"></a>Virtual Receive Side Scaling \(vRSS\)

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Neste tópico, você aprenderá sobre Virtual Receive Side Scaling (vRSS) e como configurar um adaptador de rede virtual para balancear o tráfego de rede de carga entre vários núcleos de processador lógico em uma VM. Você também pode usar o vRSS para configurar vários núcleos físicos para um host de placa de Interface de rede virtual \(vNIC\).

Essa configuração permite que a carga de um adaptador de rede virtual sejam distribuídas entre vários processadores virtuais em uma máquina virtual \(VM\), permitindo que a VM processar mais tráfego de rede com mais rapidez do que ele pode fazer com um único processador lógico.

>[!TIP]
>Você pode usar o vRSS em máquinas virtuais no Hyper\-hosts V com vários processadores, um único processador de vários núcleos, ou mais de um vários processadores de núcleo instalado e configurado para uso VM.

o vRSS é compatível com todos os outro Hyper\-V tecnologias de rede. o vRSS é dependente de fila de máquina Virtual \(VMQ\) em que o Hyper\-host V e o RSS em VM ou na vNIC do host.

Por padrão, Windows Server permite que o vRSS, mas você pode desabilitá-lo em uma VM usando o Windows PowerShell. Para obter mais informações, consulte [gerenciar vRSS](vrss-manage.md) e [comandos do Windows PowerShell para RSS e vRSS](vrss-wps.md).



## <a name="operating-system-compatibility"></a>Compatibilidade de sistema operacional

Você pode usar o RSS em qualquer computador com vários processadores ou vários núcleos - ou vRSS em qualquer VM com vários processadores ou vários núcleos - que está executando o Windows Server 2016.

Multiprocessadores ou multicore VMs que estão executando os seguintes sistemas operacionais Microsoft também dão suporte a vRSS.

- Windows Server 2016
- Windows 10 Pro ou Enterprise
- Windows Server 2012 R2
- Windows 8.1 Pro ou Enterprise
- Windows Server 2012 com os componentes de integração do Windows Server 2012 R2 instalados.
- Windows 8 com os componentes de integração do Windows Server 2012 R2 instalados.

Para obter informações sobre o suporte de vRSS para máquinas virtuais executando FreeBSD ou Linux como um sistema operacional convidado no Hyper-V, consulte [máquinas virtuais de suporte para Linux e FreeBSD para Hyper-V no Windows](https://docs.microsoft.com/windows-server/virtualization/hyper-v/Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows).
  
## <a name="hardware-requirements"></a>Requisitos de hardware

A seguir é os requisitos de hardware para o vRSS.
 
- Adaptadores de rede física devem oferecer suporte a fila de máquina Virtual \(VMQ\). Se VMQ estiver desabilitada ou não tem suporte, o vRSS está desabilitado para o Hyper\-host V e todas as VMs configuradas no host.
- Adaptadores de rede devem ter uma velocidade de link de 10 Gbps ou mais.
- Hyper\-hosts V devem ser configurados com vários processadores ou vários pelo menos um\-processador para usar o vRSS core.
- Máquinas virtuais \(VMs\) deve ser configurado para usar duas ou mais processadores lógicos.


## <a name="use-case-scenarios"></a>Cenários de caso de uso

Os seguintes cenários de caso de uso de dois retratar o uso comum de vRSS para balanceamento de carga de processador e de balanceamento de carga de software.

### <a name="processor-load-balancing"></a>Balanceamento de carga do processador
  
Antônio, um administrador de rede, está configurando um novo host do Hyper-V com o adaptador de rede de dois que dá suporte à virtualização de saída única entrada de raiz \(SR\-IOV\). Ele implanta o Windows Server 2016 para hospedar um servidor de arquivos da VM.

Depois de instalar o hardware e software, Antônio irá configurar uma VM para usar oito processadores virtuais e 4096 MB de memória. Infelizmente, ele não tem a opção de ligar o SR\-IOV porque suas VMs dependem da imposição de política através do comutador virtual, ele criou com Hyper\-Gerenciador de comutador Virtual V. Por isso, Anthony decide usar o vRSS, em vez de SR\-IOV.

Inicialmente, ele atribui quatro processadores virtuais por meio do Windows PowerShell esteja disponível para uso com o vRSS. O uso do servidor de arquivos após uma semana parecia ser muito popular, portanto, Antônio verifica o desempenho da VM.  Ele descobre que a utilização total de quatro processadores virtuais.

Por isso, Anthony decide adicionar processadores para a máquina virtual para uso pelo vRSS.  Ele atribui outros dois processadores virtuais para a VM, que ficam disponíveis automaticamente para o vRSS para ajudar a lidar com a carga de rede de grande porte. Seus esforços resultam em melhor desempenho para o servidor de arquivos da VM, com os processadores de seis controlar a carga do tráfego de rede de forma eficiente.


### <a name="software-load-balancing"></a>Balanceamento de carga do software

Sandra, um administrador de rede, está configurando uma única VM de alto desempenho em um de seus sistemas para atuar como um balanceador de carga de software. Ela tem atribuído a todos os processadores lógicos disponíveis para essa única VM.

Depois de instalar o Windows Server, ela usa o vRSS para obter o tráfego de rede paralela de processamento em vários processadores lógicos na VM. Como o Windows Server permite que o vRSS, Sandra não precisa fazer alterações de configuração.


## <a name="related-topics"></a>Tópicos relacionados

- [Planejar o uso de vRSS](vrss-plan.md)
- [Habilite o vRSS em um adaptador de rede Virtual](vrss-enable.md)
- [Manage vRSS](vrss-manage.md)
- [Perguntas frequentes sobre o vRSS](vrss-faq.md)
- [Comandos do Windows PowerShell para RSS e vRSS](vrss-wps.md)

---
