---
title: Virtual Receive Side Scaling (vRSS)
description: Saiba mais sobre o vRSS (virtual Receive Side Scaling) no Windows Server e como configurar um adaptador de rede virtual para balancear a carga do tráfego de rede de entrada entre vários núcleos de processador lógico em uma VM. Você também pode configurar múltiplos núcleos físicos para uma vNIC (placa de interface de rede virtual) do host.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 9be477b3-f81d-4e84-a6b0-ac4c1ea97715
ms.date: 09/05/2018
ms.localizationpriority: medium
manager: dougkim
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 8841e0e5b33df6b44d63598ebf1f29caf89e1f3f
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315252"
---
# <a name="virtual-receive-side-scaling-vrss"></a>\) de \(vRSS do lado virtual Receive

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Neste tópico, você aprenderá sobre o vRSS (recebimento virtual de recepção) e como configurar um adaptador de rede virtual para balancear a carga do tráfego de rede de entrada entre vários núcleos de processador lógico em uma VM. Você também pode usar o vRSS para configurar vários núcleos físicos para uma placa de interface de rede virtual do host \(vNIC\).

Essa configuração permite que a carga de um adaptador de rede virtual seja distribuída entre vários processadores virtuais em uma máquina virtual \(\)VM, permitindo que a VM processe mais o tráfego de rede mais rapidamente do que pode com um único processador lógico.

>[!TIP]
>Você pode usar o vRSS em VMs em hosts Hyper\-V que tenham vários processadores, um único processador de vários núcleos ou mais de um dos vários processadores de núcleo instalados e configurados para uso da VM.

o vRSS é compatível com todas as outras tecnologias de rede Hyper\-V. o vRSS é dependente de Fila de Máquina Virtual \(\) de VMQ no host do Hyper\-V e RSS na VM ou no host vNIC.

Por padrão, o Windows Server habilita o vRSS, mas você pode desabilitá-lo em uma VM usando o Windows PowerShell. Para obter mais informações, consulte [gerenciar](vrss-manage.md) os [comandos VRSS e Windows PowerShell para RSS e vRSS](vrss-wps.md).



## <a name="operating-system-compatibility"></a>Compatibilidade do sistema operacional

Você pode usar o RSS em qualquer multiprocessador ou computador Multicore-ou vRSS em qualquer VM multiprocessador ou multicore-que esteja executando o Windows Server 2016.

As VMs multiprocessador ou de vários núcleos que estão executando os seguintes sistemas operacionais da Microsoft também dão suporte a vRSS.

- Windows Server 2016
- Windows 10 pro ou Enterprise
- Windows Server 2012 R2
- Windows 8.1 pro ou Enterprise
- Windows Server 2012 com os componentes de integração do Windows Server 2012 R2 instalados.
- Windows 8 com os componentes de integração do Windows Server 2012 R2 instalados.

Para obter informações sobre o suporte a vRSS para VMs que executam o FreeBSD ou o Linux como um sistema operacional convidado no Hyper-V, consulte [máquinas virtuais Linux e FreeBSD com suporte para Hyper-v no Windows](https://docs.microsoft.com/windows-server/virtualization/hyper-v/Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows).
  
## <a name="hardware-requirements"></a>Requisitos de hardware

A seguir estão os requisitos de hardware para o vRSS.
 
- Os adaptadores de rede física devem oferecer suporte a Fila de Máquina Virtual \(\)de VMQ. Se a VMQ estiver desabilitada ou não tiver suporte, o vRSS será desabilitado para o host Hyper\-V e todas as VMs configuradas no host.
- Os adaptadores de rede devem ter uma velocidade de link de 10 Gbps ou mais.
- Os hosts Hyper\-V devem ser configurados com vários processadores ou pelo menos um processador com vários\-núcleo para usar o vRSS.
- Máquinas virtuais \(VMs\) devem ser configuradas para usar dois ou mais processadores lógicos.


## <a name="use-case-scenarios"></a>Cenários de caso de uso

Os dois cenários de caso de uso a seguir descrevem o uso comum de vRSS para balanceamento de carga de processador e balanceamento de carga de software.

### <a name="processor-load-balancing"></a>Balanceamento de carga do processador
  
Anthony, um administrador de rede, está configurando um novo host Hyper-V com dois adaptadores de rede que dão suporte à virtualização de entrada/saída de raiz única \(\-\)do SR-IOV. Ele implanta o Windows Server 2016 para hospedar um servidor de arquivos de VM.

Depois de instalar o hardware e o software, Anthony configura uma VM para usar oito processadores virtuais e 4096 MB de memória. Infelizmente, Anthony não tem a opção de ativar o SR\-IOV porque suas VMs dependem da imposição de políticas por meio do comutador virtual criado com o Gerenciador de comutador virtual Hyper\-V. Por isso, Anthony decide usar o vRSS em vez de SR\-IOV.

Inicialmente, Anthony atribui quatro processadores virtuais usando o Windows PowerShell para estar disponível para uso com o vRSS. O uso do servidor de arquivos após uma semana parecia ser bastante popular, portanto, Anthony verifica o desempenho da VM.  Ele descobre a utilização total dos quatro processadores virtuais.

Por isso, Anthony decide adicionar processadores à VM para uso pelo vRSS.  Ele atribui dois processadores virtuais à VM, que ficam automaticamente disponíveis para o vRSS para ajudar a lidar com a carga de rede grande. Seus esforços resultam em um melhor desempenho para o servidor de arquivos de VM, com os seis processadores manipulando com eficiência a carga de tráfego de rede.


### <a name="software-load-balancing"></a>Balanceamento de carga do software

Sandra, um administrador de rede, está configurando uma única VM de alto desempenho em um dos seus sistemas para atuar como um balanceador de carga de software. Ela atribuiu todos os processadores lógicos disponíveis a essa única VM.

Depois de instalar o Windows Server, ela usa o vRSS para obter o processamento de tráfego de rede paralelo em vários processadores lógicos na VM. Como o Windows Server habilita o vRSS, Sandra não precisa fazer nenhuma alteração de configuração.


## <a name="related-topics"></a>Tópicos relacionados

- [Planejar o uso de vRSS](vrss-plan.md)
- [Habilitar vRSS em um adaptador de rede virtual](vrss-enable.md)
- [Gerenciar vRSS](vrss-manage.md)
- [Perguntas frequentes sobre o vRSS](vrss-faq.md)
- [Comandos do Windows PowerShell para RSS e vRSS](vrss-wps.md)

---
