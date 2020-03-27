---
title: Planejar uma implantação multissite
description: Este tópico faz parte do guia implantar vários servidores de acesso remoto em uma implantação multissite no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8387eabe-7363-4367-b5b1-03c67baa2933
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d5c189969c31ef9627dd7daee0e09162ea9aec39
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313870"
---
# <a name="plan-a-multisite-deployment"></a>Planejar uma implantação multissite

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

 O Windows Server 2016, Windows Server 2012 combina DirectAccess e roteamento e VPN RRAS (serviço de acesso remoto) em uma única função de acesso remoto. Esta visão geral fornece uma introdução às etapas de planejamento necessárias para implantar o acesso remoto do Windows Server 2016 ou do Windows Server 2012 em uma configuração multissite.  
  
1.  [Implante um único servidor DirectAccess com configurações avançadas](https://technet.microsoft.com/library/hh831436(v=ws.11).aspx). Esta etapa inclui o planejamento da infraestrutura necessária para implantar um único servidor. Ele inclui o planejamento de configurações de rede e servidor, requisitos de certificado, configurações de DNS, implantação de servidor de local de rede, servidores de gerenciamento do DirectAccess, configurações de Active Directory e objetos de Política de Grupo (GPOs).  
  
2.  [Etapa 2 planejar a infraestrutura multissite](Step-2-Plan-the-Multisite-Infrastructure.md). Esta etapa inclui o planejamento de Active Directory e GPO e a configuração de DNS.  
  
3.  [Etapa 3 planejar a implantação multissite](Step-3-Plan-the-Multisite-Deployment.md). Esta etapa inclui o planejamento de configurações de certificado, configuração de servidor de local de rede, configurações de ponto de entrada de cliente, configurações de prefixo IPv6 e, opcionalmente, configurações de balanceamento de carga global.  
  
> [!NOTE]  
> Registre suas decisões de planejamento para a implantação avançada de acesso remoto. Esse registro pode ser usado como um auxílio de trabalho para todos os envolvidos na conclusão das etapas de implantação.  
  
Depois de concluir essas etapas de planejamento, consulte [Configurando uma implantação multissite](../configure/Configure-a-Multisite-Deployment.md).  
  


