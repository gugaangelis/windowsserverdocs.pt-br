---
title: Planejar o uso de vRSS
description: Você pode usar este tópico para preparar sua máquina virtual e o host Hyper-V para usando vRSS no Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 695e6192-5e84-4ab4-b33e-8ebf6b8f5cbb
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/04/2018
ms.openlocfilehash: e6558b00e87721d8ab81c84946a14745c4faa812
ms.sourcegitcommit: e84e328c13a701e8039b16a4824a6e58a6e59b0b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/22/2018
ms.locfileid: "4133382"
---
# Planejar o uso de vRSS

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

No Windows Server 2016, vRSS é habilitada por padrão, no entanto, você deve preparar seu ambiente para permitir vRSS funcione corretamente em uma máquina virtual \(VM\) ou em um host adaptador virtual \(vNIC\). No Windows Server 2012 R2, vRSS foi desabilitado por padrão.

Quando você planeja e preparar o uso de vRSS, certifique-se de que:

- O adaptador de rede física é compatível com \(VMQ\) fila de máquina Virtual e tem uma velocidade de link de 10 Gbps ou mais.
- VMQ está habilitado a placa de rede física e a porta do comutador Virtual do hyper\-v
- Não há nenhum \(SR\-IOV\) virtualização de saída único Input\ raiz configurado para a VM.
- Agrupamento NIC está configurado corretamente.
- A VM tem vários \(LPs\) processadores lógicos.

>[!NOTE]
>vRSS is also enabled by default for any host vNICs that have RSS enabled.

A seguir está informações adicionais necessárias para concluir essas etapas de preparação.
  
1. **Capacidade de adaptador de rede**. Verifique se o adaptador de rede é compatível com \(VMQ\) fila de máquina Virtual e tem uma velocidade de link de 10 Gbps ou mais. Se a velocidade do link é menos de 10 Gbps, o comutador Virtual do hyper\-v desabilita VMQ por padrão, embora ele ainda mostra VMQ como habilitado nos resultados do comando do Windows PowerShell **Get-NetAdapterVmq**. Um método que você pode usar para verificar que VMQ está habilitada ou desabilitada é usar o comando **Get-NetAdapterVmqQueue**.  Se VMQ estiver desabilitado, os resultados deste comando mostram que não há nenhum QueueID atribuído ao adaptador de rede virtual de VM ou host. 
  
2. **Habilitar VMQ**. Verificar se VMQ está habilitado no computador host. vRSS não funcionará se o host não oferece suporte a VMQ. Você pode verificar se VMQ está habilitado executando **Get-VMSwitch** e localizar o adaptador que está usando o comutador virtual. Em seguida, execute **Get-NetAdapterVmq** e certifique-se de que o adaptador é mostrado nos resultados e habilitou VMQ.
  
3. **Ausência de SR\-IOV**. Verificar se uma virtualização de saída único Input\ raiz \(SR\-IOV\) \(VF\) função Virtual driver não está conectado à interface de rede VM. Você pode verificar isso usando o comando **Get-NetAdapterSriov** . If a VF driver is loaded, RSS uses the scaling settings from this driver instead of those configured by vRSS. If the VF driver does not support RSS, then vRSS is disabled.
  
4. **Agrupamento NIC configuração**. Se você estiver usando o agrupamento NIC, é importante que você configure corretamente VMQ para trabalhar com as configurações de agrupamento NIC. Para obter informações detalhadas sobre o agrupamento NIC implantação e gerenciamento, consulte [O agrupamento NIC](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

5. **Número de LPs**. Verifique se a VM tem mais de um processador lógico \(LP\). vRSS relies on RSS in the VM or on the Hyper-V host to load balance received traffic to multiple LPs for parallel processing. Você pode observar LPs quantos sua VM tem executando o comando do Windows PowerShell **Get-VMProcessor** no host. Depois que você executar o comando, você pode observar a entrada de coluna contagem do número de LPs.

O vNIC do host sempre tem acesso a todos os processadores físicos; Para configurar o vNIC do host para usar um número específico de processadores, use as configurações **BaseProcessorNumber -** e **-MaxProcessors** quando você executar o comando do Windows PowerShell **Conjunto NetAdapterRss** .

---