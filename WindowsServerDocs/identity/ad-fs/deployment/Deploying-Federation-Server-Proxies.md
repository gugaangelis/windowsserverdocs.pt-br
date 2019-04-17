---
ms.assetid: 1b21b0a9-1fe6-4fd1-8a25-92e578d774ed
title: "Implantando Proxies de servidor de Federação"
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: b914141a0445febd3961b688aadc2f444b2eee7b
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="deploying-federation-server-proxies"></a>Implantando Proxies de servidor de Federação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para implantar proxies de servidor de Federação em serviços de Federação do Active Directory \(AD FS\), concluir cada uma das tarefas em [lista de verificação: configuração de backup de Proxy de servidor de Federação um](Checklist--Setting-Up-a-Federation-Server-Proxy.md).  
  
> [!NOTE]  
> Quando você usa essa lista de verificação, recomendamos que você leia primeiro as referências ao proxy do servidor de Federação diretrizes de planejamento do [guia de Design do AD FS no Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) antes de começar os procedimentos para configurar os servidores. A lista de verificação a seguir fornece uma melhor compreensão do processo de design e implantação de Federação proxies de servidor.  
  
## <a name="about-federation-server-proxies"></a>Sobre proxies de servidor de Federação  
Proxies de servidor de Federação são computadores que executam o Windows Server® 2012 e do AD FS software que foram configurados manualmente para agir na função de proxy. Você pode usar proxies de servidor de Federação em sua organização para fornecer serviços intermediários entre um cliente de Internet e um servidor de Federação está atrás de um firewall em sua rede corporativa.  
  
> [!NOTE]  
> Embora o servidor de Federação e as funções de proxy de servidor de Federação não podem ser instaladas no mesmo computador, um servidor de Federação pode executar funções de proxy do servidor de Federação. Para obter mais informações, consulte [quando criar um servidor de Federação](https://technet.microsoft.com/library/dd807101.aspx).  
  
O ato de instalar o software do AD FS em um computador Windows Server® 2012 e configurá-lo para servir na função proxy torna esse computador um proxy de servidor de Federação.  
  

