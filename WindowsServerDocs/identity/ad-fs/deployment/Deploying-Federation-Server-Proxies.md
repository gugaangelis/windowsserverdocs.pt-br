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
ms.openlocfilehash: 02311522ee229eeaf0b27ce8d39090a9529b99ae
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192205"
---
# <a name="deploying-federation-server-proxies"></a>Implantando proxies de servidor de federação

Para implantar os proxies de servidor de federação nos serviços de Federação do Active Directory \(do AD FS\), preencha cada uma das tarefas no [lista de verificação: Como configurar um Proxy do servidor de Federação](Checklist--Setting-Up-a-Federation-Server-Proxy.md).  
  
> [!NOTE]  
> Quando você usa essa lista de verificação, é recomendável que você leia primeiro as referências ao proxy do servidor de Federação diretrizes de planejamento a [guia de Design do AD FS no Windows Server 2012](https://technet.microsoft.com/library/dd807036.aspx) antes de começar os procedimentos para configurar os servidores. A lista de verificação a seguir fornece uma melhor compreensão do processo de design e implantação para a federação de proxies de servidor.  
  
## <a name="about-federation-server-proxies"></a>Sobre proxies de servidor de Federação  
Proxies de servidor de Federação são computadores que executam o Windows Server® 2012 e o software AD FS que foram configurados manualmente para atuar na função de proxy. Você pode usar proxies de servidor de federação em sua organização para fornecer serviços intermediários entre um cliente de Internet e um servidor de federação por trás de um firewall em sua rede corporativa.  
  
> [!NOTE]  
> Embora o servidor de Federação e as funções de proxy de servidor de Federação não podem ser instaladas no mesmo computador, um servidor de Federação pode executar funções de proxy do servidor de Federação. Para obter mais informações, consulte [When to Create a Federation Server](https://technet.microsoft.com/library/dd807101.aspx).  
  
O ato de instalar o software AD FS em um computador Windows Server® 2012 e configurá-lo para servir na função proxy de torna esse computador um proxy do servidor de Federação.  
  

