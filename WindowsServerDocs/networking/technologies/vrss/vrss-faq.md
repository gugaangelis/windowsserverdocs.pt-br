---
title: Perguntas frequentes sobre o vRSS
description: Neste tópico, você encontrará que alguns frequentes perguntas e respostas sobre como usar o vRSS.
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840237"
---
# <a name="vrss-frequently-asked-questions"></a>Perguntas frequentes sobre o vRSS

Neste tópico, você encontrará que alguns frequentes perguntas e respostas sobre como usar o vRSS.

## <a name="what-are-the-requirements-for-the-physical-network-adapters-that-i-use-with-vrss"></a>Quais são os requisitos para os adaptadores de rede física que posso usar com o vRSS?

Adaptadores de rede devem ser compatíveis com a fila de máquina Virtual \(VMQ\) e deve ter uma velocidade de link de 10 Gbps ou mais.

Para obter mais informações, consulte [planejar o uso de vRSS](vrss-plan.md).

## <a name="does-vrss-work-with-hyper-threaded-processor-cores"></a>O vRSS funciona com hyper\-threaded núcleos de processador?

Nenhum. O vRSS e VMQ ignoram hyper\-threaded núcleos de processador.

## <a name="does-vrss-work-for-host-virtual-nics-vnics"></a>O vRSS funciona para NICs de host virtual \(vNICs\)?

Sim. Use o **- ManagementOS** parâmetro em vez da máquina virtual \(VM\) nome o **Set-VMNetworkAdapter** comando do Windows PowerShell, e  **Enable-NetAdapterRss** na vNIC do host.

Para obter mais informações, consulte [comandos do Windows PowerShell para RSS e vRSS](vrss-wps.md).

## <a name="how-many-logical-processors-does-a-vm-need-to-use-vrss"></a>Número de processadores lógico uma máquina virtual precisa usar o vRSS?

As VMs que precisam de duas ou mais processadores lógicos \(LPs\) para poder usar o vRSS.

Para obter mais informações, consulte [planejar o uso de vRSS](vrss-plan.md).

## <a name="is-vrss-compatible-with-nic-teaming"></a>O vRSS é compatível com agrupamento NIC?

Sim. Se você estiver usando o agrupamento NIC, é importante configurar corretamente a VMQ para trabalhar com as configurações de agrupamento NIC. Para obter informações detalhadas sobre o gerenciamento e implantação do agrupamento NIC, consulte [agrupamento NIC](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

## <a name="vrss-is-enabled-but-how-do-i-know-if-it-is-working"></a>o vRSS é ativado, mas como saber se ele está funcionando? 

Você poderá informar o vRSS está funcionando abrindo o Gerenciador de tarefas em sua VM e exibindo a utilização do processador virtual. Se houver várias conexões estabelecidas com a VM, você pode ver mais de um núcleo acima de 0% de utilização.

Como uma única sessão TCP não pode ser balanceado entre vários núcleos de processador lógico, sua VM deve estar recebendo TCP várias sessões antes de você pode observar se ou não o vRSS está funcionando.

Se a VM estiver recebendo várias sessões TCP, mas você não vir mais de um núcleo de LP acima de 0% de utilização, certifique-se de que você tenha concluído todas as etapas de preparação no tópico [planejar o uso de vRSS](vrss-plan.md).

## <a name="im-looking-at-the-host-and-not-all-of-the-processors-are-being-used-it-looks-like-every-other-one-is-being-skipped"></a>Estou olhando um host e nem todos os processadores estão sendo usados. Parece que um sim, outro não estão sendo ignorados.
  
Verifique se a tecnologia hyper threading está habilitada. VMQ e o vRSS é projetado para ignorar hyper\-threaded núcleos.

## <a name="are-there-different-windows-powershell-commands-for-rss-and-vrss"></a>Há comandos diferentes do Windows PowerShell para RSS e vRSS?

Sim e não. Enquanto você usa os mesmos comandos para o RSS em hosts nativos e o RSS em máquinas virtuais, o vRSS também requer VMQ esteja habilitado na NIC físico – e para a VM e o vRSS para ser habilitado na porta do comutador.

Para obter mais informações, consulte [comandos do Windows PowerShell para RSS e vRSS](vrss-wps.md).

---
