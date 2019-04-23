---
title: Planejar uma implantação multissite
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
ms.assetid: 8387eabe-7363-4367-b5b1-03c67baa2933
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c99d226649c51390aa9fadd5eaa014bf3af0873f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863127"
---
# <a name="plan-a-multisite-deployment"></a>Planejar uma implantação multissite

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

 Windows Server 2016, Windows Server 2012 combina o DirectAccess e o roteamento e acesso remoto (RRAS) VPN em uma única função de acesso remoto. Esta visão geral fornece uma introdução às etapas de planejamento necessárias para implantar o Windows Server 2016 ou o acesso remoto do Windows Server 2012 em uma configuração multissite.  
  
1.  [Implantar um único servidor DirectAccess com configurações avançadas](https://technet.microsoft.com/library/hh831436(v=ws.11).aspx). Esta etapa inclui o planejamento da infraestrutura necessária para implantar um único servidor. Ele inclui o planejamento da rede e as configurações do servidor, requisitos de certificado, as configurações de DNS, implantação de servidor de local de rede, servidores de gerenciamento do DirectAccess, configurações do Active Directory e objetos de diretiva de grupo (GPOs).  
  
2.  [Etapa 2 planejar a infraestrutura de multissite](Step-2-Plan-the-Multisite-Infrastructure.md). Essa etapa inclui o Active Directory e o GPO de planejamento e configuração de DNS.  
  
3.  [Etapa 3 planejar uma implantação multissite](Step-3-Plan-the-Multisite-Deployment.md). Esta etapa inclui o planejamento de configurações de certificado, configuração do servidor de local de rede, configurações de ponto de entrada do cliente, configurações de prefixo IPv6 e configurações de balanceamento de carga global opcionalmente.  
  
> [!NOTE]  
> Registre suas decisões de planejamento para implantação avançada de acesso remoto. Esse registro pode ser usado como um auxílio de trabalho para todos os envolvidos na conclusão das etapas de implantação.  
  
Depois de concluir essas etapas de planejamento, consulte [Configurando uma implantação multissite](../configure/Configure-a-Multisite-Deployment.md).  
  


