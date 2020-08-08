---
author: eross-msft
ms.author: lizross
ms.date: 10/02/2018
ms:topic: include
ms.openlocfilehash: 43a4149d44bd24a7d8b3d68ed9e3c2b68c99e285
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87952809"
---
Com a porta Hyper-V, as equipes NIC configuradas em hosts Hyper-V fornecem endereços MAC independentes de VMs.  O endereço MAC de VMs ou a VM portada conectada ao comutador Hyper-V podem ser usados para dividir o tráfego de rede entre os membros da equipe NIC. Não é possível configurar as equipes NIC que você cria nas VMs com o modo de balanceamento de carga da porta Hyper-V. Em vez disso, use o modo de hash de endereço.