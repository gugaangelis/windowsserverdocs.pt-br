---
title: vRSS perguntas frequentes
description: Neste tópico, você encontrará que alguns comumente frequentes e respostas sobre como usar vRSS.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 61ae242e-82a8-430d-b07d-52b86c01e686
ms.localizationpriority: medium
manager: dougkim
ms.date: 09/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3fafe6c39285e65a9d39a76cc6b652dac5c3efbd
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133812"
---
# vRSS perguntas frequentes

Neste tópico, você encontrará que alguns comumente frequentes e respostas sobre como usar vRSS.

## Quais são os requisitos para os adaptadores de rede física que posso usar com vRSS?

Adaptadores de rede devem ser compatíveis com \(VMQ\) fila de máquina Virtual e devem ter uma velocidade de link de 10 Gbps ou mais.

Para obter mais informações, consulte a [Planejar o uso de vRSS](vrss-plan.md).

## VRSS funciona com núcleos hyper\-v-threaded?

Não. VRSS e VMQ ignoram núcleos hyper\-v de threads.

## VRSS funciona para host virtual NICs \(vNICs\)?

Sim. Use o parâmetro **- ManagementOS** em vez de nome \(VM\) da máquina virtual no comando **Set-VMNetworkAdapter** do Windows PowerShell e **Habilitar NetAdapterRss** sobre o vNIC do host.

For more information, see [Windows PowerShell Commands for RSS and vRSS](vrss-wps.md).

## Quantos processadores lógicos uma VM precisa usar vRSS?

VMs necessário dois ou mais \(LPs\) processadores lógicos para poder usar vRSS.

Para obter mais informações, consulte a [Planejar o uso de vRSS](vrss-plan.md).

## VRSS é compatível com o agrupamento NIC?

Sim. Se você estiver usando o agrupamento NIC, é importante que você configure corretamente VMQ para trabalhar com as configurações de agrupamento NIC. Para obter informações detalhadas sobre o agrupamento NIC implantação e gerenciamento, consulte [O agrupamento NIC](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

## vRSS está habilitada, mas como saber se ele está funcionando? 

Você poderá dizer vRSS está funcionando abrindo o Gerenciador de tarefas na sua VM e exibindo a utilização de processador virtual. Se houver várias conexões estabelecidas para a máquina virtual, você pode ver mais de um núcleo acima 0% de utilização.

Como uma única sessão TCP não pode ser balanceado entre vários núcleos lógicos, sua VM deve estar recebendo TCP várias sessões antes de você pode observar ou não vRSS está funcionando.

Se a VM está recebendo várias sessões TCP, mas você não vir mais de um núcleo LP acima utilização de 0%, certifique-se de que você concluiu todas as etapas de preparação no tópico [Planejar o uso de vRSS](vrss-plan.md).

## Eu estou olhando para o host e nem todos os processadores estão sendo usados. Parece que todos os outros um está sendo ignorado.
  
Verifique se o threading hyper está habilitada. VMQ e vRSS são projetados para ignorar hyper\-v-threaded núcleos.

## Are there different Windows PowerShell commands for RSS and vRSS?

Sim e não. While you use the same commands for both RSS in native hosts and RSS in VMs, vRSS also requires VMQ to be enabled on the physical NIC - and for the VM and vRSS to be enabled on the switch port.

For more information, see [Windows PowerShell Commands for RSS and vRSS](vrss-wps.md).

---
