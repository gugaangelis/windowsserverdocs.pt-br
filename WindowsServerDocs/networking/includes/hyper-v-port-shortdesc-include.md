---
author: eross-msft
ms.author: lizross
ms.date: 10/02/2018
ms.prod: windows-server
ms:topic: include
ms.openlocfilehash: 47a91c86ac75aedf532289055c94b34899fa01df
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316470"
---
Com a porta Hyper-V, as equipes NIC configuradas em hosts Hyper-V fornecem endereços MAC independentes de VMs.  O endereço MAC de VMs ou a VM portada conectada ao comutador Hyper-V podem ser usados para dividir o tráfego de rede entre os membros da equipe NIC. Não é possível configurar as equipes NIC que você cria nas VMs com o modo de balanceamento de carga da porta Hyper-V. Em vez disso, use o modo de hash de endereço. 