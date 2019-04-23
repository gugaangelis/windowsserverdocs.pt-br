---
ms.assetid: ''
title: Insider Preview para recursos HPN no Windows Server 2019
description: Saiba mais sobre os novos recursos de alto desempenho de rede no Windows Server 2019.
manager: dougkim
author: shortpatti
ms.author: pashort
ms.date: 09/12/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 3d3e974472f28c30d093fbd1094ef3693d984d19
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886267"
---
# <a name="insider-preview"></a>Insider Preview


## <a name="dynamic-vrss-and-vmmq"></a>O vRSS dinâmico e VMMQ

>Aplica-se a: Windows Server 2019

No passado, filas de máquina Virtual e máquina Virtual com vários habilitada a taxa de transferência superior a VMs individuais como taxas de transferência de rede atingidas pela primeira vez na marca de 10 GbE e muito mais. Infelizmente, a linha de base, planejamento, monitoramento e ajuste necessário para o sucesso se tornou uma grande empreitada; muitas vezes mais do que o administrador de TI devem gastar. 

2019 do Windows Server aprimora essas otimizações dinamicamente se espalhando e ajuste o processamento de cargas de trabalho de rede conforme necessário. Windows Server 2019 garante a máxima eficiência e remove a sobrecarga de configuração para os administradores de TI.

Para obter mais informações, consulte:

-   [Blog de comunicados](https://blogs.technet.microsoft.com/networking/2018/08/22/netperf4vw/)

-   [Guia de validação para o IT Pro](https://aka.ms/DVMMQ-Validation)

## <a name="receive-segment-coalescing-rsc-in-the-vswitch"></a>Receber segmento Coalescing (RSC) em vSwitch

>Aplica-se a: 2019 do Windows Server e Windows 10, versão 1809

Receber segmento de união (RSC) em vSwitch é um aprimoramento que une vários segmentos TCP em um segmento maior antes dos dados percorrendo o vSwitch. O segmento grande melhora o desempenho de rede para cargas de trabalho virtuais.

Anteriormente, isso era um descarregamento implementado pela NIC. Infelizmente, isso foi desabilitado no momento em que você anexou o adaptador a um comutador virtual. A RSC em vSwitch em 2019 do Windows Server e Windows 10 de outubro de 2018 atualização remove essa limitação.

Por padrão, a RSC em vSwitch está habilitada nos comutadores virtuais externos.

Para obter mais informações, consulte:

-  [Blog de comunicados](https://blogs.technet.microsoft.com/networking/2018/08/22/netperf4vw/)

-  [Guia de validação para o IT Pro](https://aka.ms/RSC-Validation)
