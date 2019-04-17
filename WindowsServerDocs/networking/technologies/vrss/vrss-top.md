---
title: Virtual Receive Side Scaling (vRSS)
description: Saiba mais sobre Virtual Receive Side Scaling (vRSS) no Windows Server e como configurar um adaptador de rede virtual para carregar o tráfego de rede equilíbrio entre vários núcleos lógicos em uma VM. Você também pode configurar vários núcleos de física para um host de placa de Interface de rede virtual (vNIC).
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
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133682"
---
# Virtual receber lado dimensionamento \(vRSS\)

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Neste tópico, você saberá mais sobre como configurar um adaptador de rede virtual para carregar o tráfego de rede equilíbrio entre vários núcleos lógicos em uma VM e Virtual Receive Side Scaling (vRSS). Você também pode usar vRSS para configurar vários núcleos físicos para um host virtual placa de rede \(vNIC\).

Essa configuração permite que a carga de um adaptador de rede virtual para ser distribuídos em vários processadores virtuais em uma máquina virtual \(VM\), permitindo que a VM processar mais tráfego de rede com mais rapidez do que ele pode com um único processador lógico.

>[!TIP]
>Você pode usar vRSS em máquinas virtuais em hosts do hyper\-v que têm processadores múltiplos, um único processador de vários núcleos, ou mais de um vários processadores core instalado e configurado para uso da VM.

vRSS é compatível com todas as outras tecnologias de rede do hyper\-v. vRSS is dependent on Virtual Machine Queue \(VMQ\) in the Hyper\-V host and RSS in the VM or on the host vNIC.

Por padrão, o Windows Server permite vRSS, mas você pode desabilitá-lo em uma VM usando o Windows PowerShell. For more information, see [Manage vRSS](vrss-manage.md) and [Windows PowerShell Commands for RSS and vRSS](vrss-wps.md).



## Compatibilidade do sistema operacional

You can use RSS on any multiprocessor or multicore computer - or vRSS on any multiprocessor or multicore VM - that is running Windows Server 2016.

Vários processadores ou multinucleada VMs que estejam executando os seguintes sistemas operacionais Microsoft também oferecem suporte a vRSS.

- Windows Server 2016
- Windows 10 Pro ou Enterprise
- Windows Server 2012 R2
- Windows 8.1 Pro ou Enterprise
- Windows Server 2012 com os componentes de integração do Windows Server 2012 R2 instalados.
- O Windows 8 com os componentes de integração do Windows Server 2012 R2 instalados.

Para obter informações sobre o suporte de vRSS para VMs em execução FreeBSD ou Linux como um sistema operacional convidado no Hyper-V, consulte o [suporte para Linux e FreeBSD máquinas virtuais do Hyper-V no Windows](https://docs.microsoft.com/windows-server/virtualization/hyper-v/Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows).
  
## Requisitos de hardware

Estes são os requisitos de hardware para vRSS.
 
- Adaptadores de rede física devem dar suporte a fila de máquina Virtual \(VMQ\). Se VMQ estiver desabilitada ou não é compatível, vRSS está desabilitada para o host hyper\-v-V e as VMs configuradas no host.
- Adaptadores de rede devem ter uma velocidade de link de 10 Gbps ou mais.
- Hosts hyper\-v-V devem ser configurados com vários processadores ou processador de vários \-core pelo menos um usar vRSS.
- Máquinas virtuais \(VMs\) deve ser configurado para usar dois ou mais processadores lógicos.


## Cenários de caso de uso

Os seguintes cenários de caso de uso de dois indicam o uso comum das vRSS para balanceamento de carga de processador e balanceamento de carga de software.

### Balanceamento de carga de processador
  
Anthony, um administrador de rede, está configurando um novo host Hyper-V com o adaptador de rede dois que dá suporte a virtualização de saída de entrada único de raiz \(SR\-IOV\). Ele implanta o Windows Server 2016 para hospedar um servidor de arquivos VM.

Depois de instalar o hardware e software, Anthony configura uma VM usar oito processadores virtuais e 4096 MB de memória. Infelizmente, Anthony não tem a opção de ativar SR\-IOV porque seus VMs dependem de imposição da política por meio do comutador virtual que ele criou com o Gerenciador de comutador Virtual do hyper\-v. Por isso, Anthony decide usar vRSS em vez de SR\-IOV.

Inicialmente, Anthony atribui quatro processadores virtuais usando o Windows PowerShell estão disponíveis para uso com vRSS. O uso do servidor de arquivos depois de uma semana aparenta ser bastante populares, portanto, Anthony verifica o desempenho da VM.  Ele descobre utilização completa de quatro processadores virtuais.

Por isso, Anthony decide adicionar processadores a VM para uso por vRSS.  Ele atribui dois processadores virtuais mais a VM, o que ficam automaticamente disponíveis para vRSS para ajudar a lidar com a carga de rede grande. Seus esforços resultam em melhor desempenho para o servidor de arquivos VM, com os processadores de seis tratando a carga de tráfego de rede com eficiência.


### Balanceamento de carga de software

Sandra, um administrador de rede, está configurando uma única VM de alto desempenho em um dos seus sistemas para atuar como um balanceador de carga de software. Ela atribuiu todos os processadores lógicos disponíveis para essa única VM.

Após a instalação do Windows Server, ela usa vRSS para obter o tráfego de rede paralelo vários processadores lógicos na VM de processamento. Como o Windows Server permite vRSS, Sandra não precisa fazer alterações de configuração.


## Tópicos relacionados

- [Planejar o uso de vRSS](vrss-plan.md)
- [Habilitar vRSS em um adaptador de rede Virtual](vrss-enable.md)
- [Gerenciar vRSS](vrss-manage.md)
- [vRSS perguntas frequentes](vrss-faq.md)
- [Windows PowerShell Commands for RSS and vRSS](vrss-wps.md)

---
