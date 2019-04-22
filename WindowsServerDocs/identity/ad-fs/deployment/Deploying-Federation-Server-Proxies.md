---
ms.assetid: 1b21b0a9-1fe6-4fd1-8a25-92e578d774ed
title: Implantando proxies de servidor de federação
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: b914141a0445febd3961b688aadc2f444b2eee7b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59816257"
---
# <a name="deploying-federation-server-proxies"></a>Implantando proxies de servidor de federação

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Para implantar os proxies de servidor de federação nos serviços de Federação do Active Directory \(do AD FS\), preencha cada uma das tarefas no [lista de verificação: Como configurar um Proxy do servidor de Federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md).  
  
> [!NOTE]  
> Quando você usa essa lista de verificação, é recomendável que você leia primeiro as referências ao proxy do servidor de Federação diretrizes de planejamento a [guia de Design do AD FS no Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) antes de começar os procedimentos para configurar os servidores. A lista de verificação a seguir fornece uma melhor compreensão do processo de design e implantação para a federação de proxies de servidor.  
  
## <a name="about-federation-server-proxies"></a>Sobre proxies de servidor de Federação  
Proxies de servidor de Federação são computadores que executam o Windows Server® 2012 e o software AD FS que foram configurados manualmente para atuar na função de proxy. Você pode usar proxies de servidor de federação em sua organização para fornecer serviços intermediários entre um cliente de Internet e um servidor de federação por trás de um firewall em sua rede corporativa.  
  
> [!NOTE]  
> Embora o servidor de Federação e as funções de proxy de servidor de Federação não podem ser instaladas no mesmo computador, um servidor de Federação pode executar funções de proxy do servidor de Federação. Para obter mais informações, consulte [When to Create a Federation Server](https://technet.microsoft.com/library/dd807101.aspx).  
  
O ato de instalar o software AD FS em um computador Windows Server® 2012 e configurá-lo para servir na função proxy de torna esse computador um proxy do servidor de Federação.  
  

