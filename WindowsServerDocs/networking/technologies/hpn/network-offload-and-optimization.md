---
title: Tecnologias de descarregamento e otimização de rede
description: Este tópico fornece uma visão geral do descarregamento e tecnologias de otimização no Windows Server 2016 e inclui links para diretrizes adicionais sobre essas tecnologias.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/20/2018
ms.openlocfilehash: 9cd63ae557eab6e8e4f69fedd20619d4e7af9184
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847847"
---
# <a name="network-offload-and-optimization-technologies"></a>Tecnologias de descarregamento e otimização de rede

Neste tópico, podemos lhe dar uma visão geral de rede diferente descarregamento e a otimização de recursos disponíveis no Windows Server 2016 e discutir como eles ajudam a tornar o sistema de rede mais eficiente. Essas tecnologias incluem recursos de Software apenas (SO) e tecnologias de Software e Hardware (SH) integrado de recursos e tecnologias e os recursos de Hardware apenas (HO) e tecnologias.

As três categorias de recursos de rede disponíveis no Windows Server 2016 são: 

1.  [Apenas software (SO) recursos e tecnologias](hpn-software-only-features.md): Esses recursos são implementados como parte do sistema operacional e são independentes do NIC subjacente (s). Às vezes, esses recursos serão exigem algum ajuste da NIC para a operação ideal. Esses exemplos de recursos do Hyper-v como vmQoS, ACLs e recursos de não-Hyper-V como agrupamento NIC.   

2.  [Software e Hardware (SH) integrado de recursos e tecnologias](hpn-software-hardware-features.md): Esses recursos têm componentes de hardware e software. O software está intimamente relacionado a recursos de hardware que são necessários para o recurso funcione. Exemplos disso incluem VMMQ, VMQ, descarregamento de soma de verificação do lado de envio IPv4 e RSS.   

3.  [Tecnologias e recursos do hardware apenas (HO)](hpn-hardware-only-features.md): Esses acelerações de hardware melhorar o desempenho de rede em conjunto com o software, mas intimamente não fazem parte de qualquer recurso de software. Exemplos disso incluem descarregamento de soma de verificação do lado de recebimento IPv4, controle de fluxo e moderação de interrupção. 

4. [Propriedades avançadas de NIC](hpn-nic-advanced-properties.md): Você pode gerenciar as NICs e todos os recursos por meio do PowerShell do Windows usando o cmdlet NetAdapter.  Você também pode gerenciar as NICs e todos os recursos usando o painel de controle de rede (ncpa. cpl). 

>[!TIP]
>- Portanto, recursos e tecnologias estão disponíveis em todas as arquiteturas de hardware, independentemente da velocidade do NIC ou recursos NIC.
>
>- Recursos SH e HO estão disponíveis somente quando o adaptador de rede oferece suporte a recursos ou tecnologias.

---