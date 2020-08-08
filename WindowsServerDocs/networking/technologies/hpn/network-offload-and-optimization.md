---
title: Tecnologias de descarregamento e otimização de rede
description: Este tópico fornece uma visão geral das tecnologias de descarregamento e otimização no Windows Server 2016 e inclui links para diretrizes adicionais sobre essas tecnologias.
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/20/2018
ms.openlocfilehash: e6f53fa8462a51e23fa9fc00aad89d8a8a3ee445
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87947000"
---
# <a name="network-offload-and-optimization-technologies"></a>Tecnologias de descarregamento e otimização de rede

Neste tópico, fornecemos uma visão geral dos diferentes recursos de descarregamento de rede e otimização disponíveis no Windows Server 2016 e discutem como eles ajudam a tornar a rede mais eficiente. Essas tecnologias incluem apenas software (SO) recursos e tecnologias, software e hardware (SH) recursos integrados e tecnologias, além de tecnologias e recursos de hardware apenas (HO).

As três categorias de recursos de rede disponíveis no Windows Server 2016 são:

1.  [Recursos e tecnologias apenas de software](hpn-software-only-features.md): esses recursos são implementados como parte do sistema operacional e são independentes das NICs subjacentes. Às vezes, esses recursos exigirão algum ajuste da NIC para uma operação ideal. Exemplos disso incluem recursos do Hyper-v, como vmQoS, ACLs e recursos não Hyper-V como agrupamento NIC.

2.  [Tecnologias e recursos integrados de software e hardware (SH)](hpn-software-hardware-features.md): esses recursos têm componentes de software e hardware. O software está intimamente ligado a recursos de hardware que são necessários para que o recurso funcione. Exemplos desses incluem VMMQ, VMQ, descarregamento de soma de verificação do IPv4 do lado do envio e RSS.

3.  [Tecnologias e recursos de hardware apenas (Ho)](hpn-hardware-only-features.md): essas acelerações de hardware melhoram o desempenho de rede em conjunto com o software, mas não fazem parte de nenhum recurso de software. Exemplos desses incluem moderação de interrupção, controle de fluxo e descarregamento de soma de verificação de IPv4 no lado do recebimento.

4. [Propriedades avançadas de NIC](hpn-nic-advanced-properties.md): você pode gerenciar NICs e todos os recursos por meio do Windows PowerShell usando o cmdlet do netadapter.  Você também pode gerenciar NICs e todos os recursos usando o painel de controle de rede (ncpa.cpl).

>[!TIP]
>- ASSIM, os recursos e as tecnologias estão disponíveis em todas as arquiteturas de hardware, independentemente da velocidade da NIC ou dos recursos da NIC.
>
>- Os recursos SH e HO estão disponíveis somente quando o adaptador de rede dá suporte a recursos ou tecnologias.

---