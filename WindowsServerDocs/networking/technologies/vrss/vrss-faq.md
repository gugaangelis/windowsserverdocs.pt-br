---
title: Perguntas frequentes sobre o vRSS
description: Neste tópico, você encontrará algumas perguntas e respostas frequentes sobre como usar o vRSS.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 61ae242e-82a8-430d-b07d-52b86c01e686
ms.localizationpriority: medium
manager: dougkim
ms.date: 09/05/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 5c7feb696c6ee9014032229543a4f43fb5884527
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395852"
---
# <a name="vrss-frequently-asked-questions"></a>Perguntas frequentes sobre o vRSS

Neste tópico, você encontrará algumas perguntas e respostas frequentes sobre como usar o vRSS.

## <a name="what-are-the-requirements-for-the-physical-network-adapters-that-i-use-with-vrss"></a>Quais são os requisitos para os adaptadores de rede física que uso com o vRSS?

Os adaptadores de rede devem ser \(compatíveis\) com a VMQ fila de máquina virtual e devem ter uma velocidade de link de 10 Gbps ou mais.

Para obter mais informações, consulte [planejar o uso de vRSS](vrss-plan.md).

## <a name="does-vrss-work-with-hyper-threaded-processor-cores"></a>O vRSS funciona com núcleos de processador Hyper\--Threading?

Nº O vRSS e a VMQ ignoram os núcleos de processador Hyper\--Threading.

## <a name="does-vrss-work-for-host-virtual-nics-vnics"></a>O vRSS funciona para NICs \(virtuais do host vNICs?\)

Sim. Use o parâmetro **-ManagementOS** em vez do nome da \(VM\) da máquina virtual no comando **set-VMNetworkAdapter** do Windows PowerShell e **Enable-NetAdapterRss** no host vNIC.

Para obter mais informações, consulte [comandos do Windows PowerShell para RSS e vRSS](vrss-wps.md).

## <a name="how-many-logical-processors-does-a-vm-need-to-use-vrss"></a>Quantos processadores lógicos uma VM precisa para usar o vRSS?

As VMs precisam de dois ou mais \(processadores\) lógicos LPs para poder usar o vRSS.

Para obter mais informações, consulte [planejar o uso de vRSS](vrss-plan.md).

## <a name="is-vrss-compatible-with-nic-teaming"></a>O vRSS é compatível com o agrupamento NIC?

Sim. Se você estiver usando o agrupamento NIC, é importante que você configure corretamente a VMQ para trabalhar com as configurações de agrupamento NIC. Para obter informações detalhadas sobre implantação e gerenciamento de agrupamento NIC, consulte [agrupamento NIC](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

## <a name="vrss-is-enabled-but-how-do-i-know-if-it-is-working"></a>o vRSS está habilitado, mas como posso saber se ele está funcionando? 

Você poderá dizer que o vRSS está funcionando abrindo o Gerenciador de tarefas em sua VM e exibindo a utilização do processador virtual. Se houver várias conexões estabelecidas com a VM, você poderá ver mais de um núcleo acima de 0% de utilização.

Como uma única sessão TCP não pode ter balanceamento de carga entre vários núcleos de processador lógicos, sua VM deve receber várias sessões TCP para que você possa observar se o vRSS está funcionando ou não.

Se a VM estiver recebendo várias sessões TCP, mas você não vir mais de um núcleo LP acima de 0% de utilização, verifique se você concluiu todas as etapas de preparação no tópico [planejar o uso de vRSS](vrss-plan.md).

## <a name="im-looking-at-the-host-and-not-all-of-the-processors-are-being-used-it-looks-like-every-other-one-is-being-skipped"></a>Estou olhando para o host e nem todos os processadores estão sendo usados. Parece que um sim, outro não estão sendo ignorados.
  
Verifique se a tecnologia hyper threading está habilitada. A VMQ e a vRSS são projetadas para\-ignorar os núcleos hyper-threaded.

## <a name="are-there-different-windows-powershell-commands-for-rss-and-vrss"></a>Há comandos do Windows PowerShell diferentes para RSS e vRSS?

Sim e não. Embora você use os mesmos comandos para RSS em hosts nativos e RSS em VMs, o vRSS também requer que a VMQ seja habilitada na NIC física – e para que a VM e o vRSS sejam habilitados na porta do comutador.

Para obter mais informações, consulte [comandos do Windows PowerShell para RSS e vRSS](vrss-wps.md).

---
