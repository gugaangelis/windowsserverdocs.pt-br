---
author: shortpatti
ms.author: pashort
ms.date: 10/02/2018
ms.prod: windows-server
ms:topic: include
ms.openlocfilehash: 01f231ca730a19ac0e7e868bcb7180377830afe1
ms.sourcegitcommit: 73898afec450fb3c2f429ca373f6b48a74b19390
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/03/2019
ms.locfileid: "71935070"
---
Com a porta Hyper-V, as equipes NIC configuradas em hosts Hyper-V fornecem endereços MAC independentes de VMs.  O endereço MAC de VMs ou a VM portada conectada ao comutador Hyper-V podem ser usados para dividir o tráfego de rede entre os membros da equipe NIC. Não é possível configurar as equipes NIC que você cria nas VMs com o modo de balanceamento de carga da porta Hyper-V. Em vez disso, use o modo de hash de endereço. 