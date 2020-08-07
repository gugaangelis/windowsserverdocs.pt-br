---
title: Visualização do insider para recursos do HPN no Windows Server 2019
description: Saiba mais sobre os novos recursos de rede de alto desempenho no Windows Server 2019.
manager: dougkim
author: eross-msft
ms.author: lizross
ms.date: 09/12/2018
ms.topic: article
ms.openlocfilehash: 2eb6ec82d9127e89f1d7871d0143ae3716b4355e
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87955643"
---
# <a name="new-hpn-features-in-windows-server-2019"></a>Novos recursos do HPN no Windows Server 2019


## <a name="dynamic-vrss-and-vmmq"></a>VRSS e VMMQ dinâmicos

>Aplica-se a: Windows Server 2019

No passado, as filas de máquinas virtuais e as várias filas de máquinas virtuais permitiam uma taxa de transferência muito maior para VMs individuais, já que as taxas de transferência de rede chegaram pela primeira vez à marca de 10 GbE e além. Infelizmente, o planejamento, a linha de base, o ajuste e o monitoramento necessários para o sucesso se tornaram um grande empreendimento; muitas vezes, o administrador de ti pretende gastar.

O Windows Server 2019 melhora essas otimizações, distribuindo e ajustando dinamicamente o processamento de cargas de trabalho de rede, conforme necessário. O Windows Server 2019 garante a eficiência de pico e remove a carga de configuração para os administradores de ti.

Para obter mais informações, consulte:

-   [Blog do comunicado](https://blogs.technet.microsoft.com/networking/2018/08/22/netperf4vw/)

-   [Guia de validação do profissional de ti](https://aka.ms/DVMMQ-Validation)

## <a name="receive-segment-coalescing-rsc-in-the-vswitch"></a>Receber segmento Coalescing (RSC) em vSwitch

>Aplica-se a: Windows Server 2019 e Windows 10, versão 1809

A União de segmento de recebimento (RSC) no vSwitch é um aprimoramento que une vários segmentos TCP em um segmento maior antes de os dados atravessarem o vSwitch. O segmento grande melhora o desempenho de rede para cargas de trabalho virtuais.

Anteriormente, esse foi um descarregamento implementado pela NIC. Infelizmente, isso foi desabilitado no momento em que você anexou o adaptador a um comutador virtual. O RSC no vSwitch no Windows Server 2019 e no Windows 10 de outubro 2018 atualização remove essa limitação.

Por padrão, o RSC no vSwitch é habilitado em comutadores virtuais externos.

Para obter mais informações, consulte:

-  [Blog do comunicado](https://blogs.technet.microsoft.com/networking/2018/08/22/netperf4vw/)

-  [Guia de validação do profissional de ti](https://aka.ms/RSC-Validation)
