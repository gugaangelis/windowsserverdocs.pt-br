---
title: Etapa 4 verificar a implantação multissite
description: Este tópico faz parte do guia de implantar vários servidores de acesso remoto em uma implantação multissite no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 345b676a-a397-4d51-9973-8b25bc05fa55
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 17b944bd0e2c13f9a3d324eeda09c67b110ce49d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821847"
---
# <a name="step-4-verify-the-multisite-deployment"></a>Etapa 4 verificar a implantação multissite

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico descreve como verificar que você configurou sua implantação do acesso remoto multissite corretamente.  
  
### <a name="to-verify-access-to-internal-resources-through-the-multisite-deployment"></a>Para verificar o acesso aos recursos internos por meio da implantação multissite  
  
1.  Conecte um computador cliente do DirectAccess à rede corporativa e obtenha a política de grupo.  
  
2.  Conecte o computador cliente à rede externa e tentar acessar os recursos internos.  
  
    Você deve conseguir acessar todos os recursos corporativos.  
  
3.  Teste a conectividade por meio de cada servidor na implantação multissite desativar ou desconexão da rede externa, todos, exceto um dos servidores de acesso remoto. No computador cliente, tente acessar recursos corporativos. Repita o teste em um servidor diferente de multissite. Ele pode levar até 10 minutos para o computador cliente para se conectar ao novo ponto de entrada. Isso ocorre porque a investigação é desativada para 10 minutos para que um ponto de entrada depois que ele é considerado não acessível, a fim de otimizar a vida útil da bateria e largura de banda. Como alternativa, você pode alternar entre os vários pontos de entrada manualmente, escolhendo o ponto de entrada desejado na caixa de combinação mostrada ao executar **daprop.exe**.  
  
    Você deve ser capaz de acessar todos os recursos corporativos por meio de cada servidor multissite.  
  
4.  Conectar-se o Windows 7&reg; computador cliente para a corporação de rede e obter a política de grupo.  
  
5.  Conectar o computador de cliente do Windows 7 para a rede externa e tentar acessar os recursos internos.  
  
    Você deve conseguir acessar todos os recursos corporativos.  
  
6.  Testar a conectividade para clientes do Windows 7 por meio de cada servidor na implantação multissite, acessar o console do Active Directory Users and Computers e mover o computador cliente para o grupo de segurança que corresponde a cada servidor. Depois que as alterações foram replicadas em todo o domínio, reinicie o computador cliente, enquanto estiver conectado à rede corporativa para obter a nova política de grupo. Tentativa de acessar recursos corporativos. Repita o teste em um servidor diferente de multissite.  
  
    Você deve ser capaz de acessar todos os recursos corporativos por meio de cada servidor multissite.  
  
    Em um ambiente de produção esse método pode não ser viável devido à quantidade de tempo necessária para que as alterações sejam replicados em todo o domínio. Talvez você queira forçar a replicação sempre que possível. Teste também pode ser feito de vários computadores cliente Windows 7 diferentes que já são membros de diferentes grupos de segurança do Windows 7 na implantação multissite.  
  


