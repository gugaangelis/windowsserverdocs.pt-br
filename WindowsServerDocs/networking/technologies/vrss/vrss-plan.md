---
title: Planejar o uso de vRSS
description: Você pode usar este tópico para preparar sua máquina virtual e o host Hyper-V para usar o vRSS no Windows Server 2016.
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850437"
---
# <a name="plan-the-use-of-vrss"></a>Planejar o uso de vRSS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

No Windows Server 2016, o vRSS é ativado por padrão, no entanto, você deve preparar seu ambiente para permitir que o vRSS funcionar corretamente em uma máquina virtual \(VM\) ou em um adaptador virtual do host \(vNIC\). No Windows Server 2012 R2, o vRSS foi desabilitado por padrão.

Quando você planejar e preparar o uso de vRSS, certifique-se de que:

- O adaptador de rede física é compatível com a fila de máquina Virtual \(VMQ\) e tem uma velocidade de link de 10 Gbps ou mais.
- VMQ está ativada, a NIC física e o Hyper\-porta do comutador Virtual V
- Não há nenhuma entrada de raiz única\-Output Virtualization \(SR\-IOV\) configurado para a VM.
- Agrupamento NIC está configurado corretamente.
- A máquina virtual tem vários processadores lógicos \(LPs\).

>[!NOTE]
>o vRSS também é habilitado por padrão para qualquer vNICs do host que têm o RSS habilitado.

Veja a seguir informações adicionais necessárias para concluir essas etapas de preparação.
  
1. **Capacidade do adaptador de rede**. Verifique se o adaptador de rede é compatível com a fila de máquina Virtual \(VMQ\) e tem uma velocidade de link de 10 Gbps ou mais. Se a velocidade do link é menor que 10 Gbps, o Hyper\-V Virtual Switch desabilita a VMQ por padrão, mesmo que ele ainda mostrará a VMQ, conforme habilitado nos resultados do comando Windows PowerShell **Get-NetAdapterVmq**. Um método que você pode usar para verificar se a VMQ está habilitada ou desabilitada é usar o comando **Get-NetAdapterVmqQueue**.  Se a VMQ está desabilitada, os resultados desse comando mostram que não há nenhum QueueID atribuída ao adaptador de rede virtual de host ou VM. 
  
2. **Habilitar VMQ**. Verifique se a VMQ está ativada no computador host. o vRSS não funcionará se o host não oferece suporte a VMQ. Você pode verificar a VMQ está ativada, executando **Get-VMSwitch** e encontrando o adaptador que o comutador virtual está usando. Em seguida, execute **Get-NetAdapterVmq** e assegure-se de que o adaptador é exibido nos resultados e que possui a VMQ habilitada.
  
3. **Ausência de SR\-IOV**. Verifique se que uma única raiz entrada\-Output Virtualization \(SR\-IOV\) função Virtual \(VF\) driver não está anexado ao adaptador de rede VM. Você pode verificar isso usando o **Get-NetAdapterSriov** comando. Se um driver VF estiver carregado, o RSS usa as configurações de escala desse driver em vez das configuradas pelo vRSS. Se o driver VF não oferece suporte a RSS, o vRSS está desabilitado.
  
4. **Configuração de agrupamento NIC**. Se você estiver usando o agrupamento NIC, é importante configurar corretamente a VMQ para trabalhar com as configurações de agrupamento NIC. Para obter informações detalhadas sobre o gerenciamento e implantação do agrupamento NIC, consulte [agrupamento NIC](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

5. **Número de LPs**. Verifique se a VM tem mais de um processador lógico \(LP\). o vRSS baseia-se no RSS em VM ou no host Hyper-V para balancear recebida o tráfego para vários LPs para processamento paralelo. Você pode observar quantas LPs sua VM tem, executando o comando do Windows PowerShell **Get-VMProcessor** no host. Após executar o comando, você pode observar a entrada da coluna contagem do número de LPs.

O vNIC do host sempre tem acesso a todos os processadores físicos; Para configurar o vNIC do host para usar um número específico de processadores, use as configurações **- BaseProcessorNumber** e **- MaxProcessors** quando você executa o **Set-NetAdapterRss** Comando do Windows PowerShell.

---