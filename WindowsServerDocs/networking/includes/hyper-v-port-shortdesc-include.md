---
author: shortpatti
ms.author: pashort
ms.date: 10/02/2018
ms.prod: windows-server-threshold
ms:topic: include
ms.openlocfilehash: 761deb136ebd4ec22dfeebc47b4eeb7650594d89
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863707"
---
Com a porta do Hyper-V, agrupamentos NIC configurado em hosts do Hyper-V fornecem endereços MAC independentes de VMs.  O endereço MAC de VMs ou a VM com portas conectadas ao comutador do Hyper-V, pode ser usada para dividir o tráfego de rede entre os membros da equipe NIC. Não é possível configurar os agrupamentos NIC que você criar em máquinas virtuais com o modo de balanceamento de carga de porta Hyper-V. Em vez disso, use o modo de Hash de endereço. 