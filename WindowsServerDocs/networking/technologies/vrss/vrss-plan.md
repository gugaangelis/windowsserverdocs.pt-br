---
title: Planejar o uso de vRSS
description: Você pode usar este tópico para preparar sua máquina virtual e o host Hyper-V para usar o vRSS no Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 695e6192-5e84-4ab4-b33e-8ebf6b8f5cbb
ms.localizationpriority: medium
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/04/2018
ms.openlocfilehash: 3addb500a654ff9f23c56388fec3ef2c3855a1da
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395822"
---
# <a name="plan-the-use-of-vrss"></a>Planejar o uso de vRSS

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

No Windows Server 2016, o vRSS é habilitado por padrão. no entanto, você deve preparar seu ambiente para permitir que o vRSS funcione corretamente em uma máquina virtual \(VM @ no__t-1 ou em um adaptador virtual de host \(vNIC @ no__t-3. No Windows Server 2012 R2, o vRSS foi desabilitado por padrão.

Ao planejar e preparar o uso de vRSS, verifique se:

- O adaptador de rede física é compatível com Fila de Máquina Virtual \(VMQ @ no__t-1 e tem uma velocidade de link de 10 Gbps ou mais.
- A VMQ está habilitada na NIC física e na porta do comutador virtual Hyper @ no__t-0V
- Não há entrada de raiz única @ no__t-0Output Virtualization \(SR @ no__t-2IOV @ no__t-3 configurado para a VM.
- O agrupamento NIC está configurado corretamente.
- A VM tem vários processadores lógicos \(LPs @ no__t-1.

>[!NOTE]
>o vRSS também é habilitado por padrão para qualquer vNICs de host que tenha o RSS habilitado.

Veja a seguir as informações adicionais necessárias para concluir essas etapas de preparação.
  
1. **Capacidade do adaptador de rede**. Verifique se o adaptador de rede é compatível com Fila de Máquina Virtual \(VMQ @ no__t-1 e tem uma velocidade de link de 10 Gbps ou mais. Se a velocidade do link for inferior a 10 Gbps, o comutador virtual Hyper @ no__t-0V desabilitará a VMQ por padrão, embora ainda mostre a VMQ como habilitada nos resultados do comando **Get-NetAdapterVmq**do Windows PowerShell. Um método que você pode usar para verificar se a VMQ está habilitada ou desabilitada é usar o comando **Get-NetAdapterVmqQueue**.  Se a VMQ estiver desabilitada, os resultados desse comando mostrarão que não há Filaid atribuída à VM ou ao adaptador de rede virtual do host. 
  
2. **Habilite a VMQ**. Verifique se a VMQ está ativada no computador host. o vRSS não funcionará se o host não der suporte à VMQ. Você pode verificar se a VMQ está habilitada executando **Get-VMSwitch** e localizando o adaptador que o comutador virtual está usando. Em seguida, execute **Get-NetAdapterVmq** e assegure-se de que o adaptador é exibido nos resultados e que possui a VMQ habilitada.
  
3. **Ausência de Sr @ no__t-1IOV**. Verifique se uma única entrada de raiz @ no__t-0Output virtualização \(SR @ no__t-2IOV @ no__t-3 a função virtual \(VF @ no__t-5 driver não está conectada à interface de rede da VM. Você pode verificar isso usando o comando **Get-NetAdapterSriov** . Se um driver VF for carregado, o RSS usará as configurações de dimensionamento desse driver em vez das configuradas por vRSS. Se o driver VF não oferecer suporte a RSS, o vRSS será desabilitado.
  
4. **Configuração de agrupamento NIC**. Se você estiver usando o agrupamento NIC, é importante que você configure corretamente a VMQ para trabalhar com as configurações de agrupamento NIC. Para obter informações detalhadas sobre implantação e gerenciamento de agrupamento NIC, consulte [agrupamento NIC](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

5. **Número de LPS**. Verifique se a VM tem mais de um processador lógico \(LP @ no__t-1. o vRSS se baseia em RSS na VM ou no host Hyper-V para balancear a carga do tráfego recebido para vários LPs para processamento paralelo. Você pode observar quantos LPs sua VM tem ao executar o comando **Get-VMProcessor** do Windows PowerShell no host. Depois de executar o comando, você pode observar a entrada de coluna de contagem para o número de LPs.

O host vNIC sempre tem acesso a todos os processadores físicos; para configurar o host vNIC para usar um número específico de processadores, use as configurações **-BaseProcessorNumber** e **-MaxProcessors** ao executar o comando **set-NetAdapterRss** do Windows PowerShell.

---