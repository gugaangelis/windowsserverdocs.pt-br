---
ms.assetid: 222e9f93-7c41-4527-8a98-8f7fbc7a58af
title: Implantando proxies de servidor de federação
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 330214e83b6da5bf711c36995306f8f1a098fa24
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192207"
---
# <a name="deploying-federation-server-proxies"></a>Implantando proxies de servidor de federação

Nos serviços de Federação do Active Directory \(do AD FS\) no Windows Server 2012 R2, a função de um proxy do servidor de Federação é tratada por um novo serviço de função acesso remoto chamado Proxy de aplicativo Web. Para habilitar o AD FS para acessibilidade de fora da rede corporativa, o que era o propósito de implantar um proxy do servidor de Federação em versões herdadas do AD FS, como o AD FS 2.0 e AD FS no Windows Server 2012, você pode implantar um ou mais proxies de aplicativo web para um D FS no Windows Server 2012 R2.  
  
No contexto do AD FS, Proxy de aplicativo Web funciona como um proxy do servidor de Federação do AD FS. Além disso, o Proxy de Aplicativo Web fornece funcionalidade de proxy reverso para aplicativos Web dentro da rede corporativa. Isso permite aos usuários acessá-los de qualquer dispositivo fora da rede corporativa. Para obter mais informações sobre o serviço da função Proxy de Aplicativo Web, consulte Visão geral de proxy de aplicativo Web.  
  
Para planejar a implantação do proxy de Aplicativo Web, é possível examinar as informações nos tópicos a seguir:  
  
-   [Planejar a infraestrutura de Proxy de aplicativo Web (WAP)](https://technet.microsoft.com/library/dn383648.aspx)  
  
-   [Planejar o servidor de Proxy de aplicativo Web](https://technet.microsoft.com/library/dn383647.aspx)  
  
Para implantar o Proxy de Aplicativo Web, você pode seguir os procedimentos dos tópicos a seguir:  
  
-   [Configurar a infraestrutura de Proxy de aplicativo Web](https://technet.microsoft.com/library/dn383644.aspx)  
  
-   [Instalar e configurar o servidor de Proxy de aplicativo Web](https://technet.microsoft.com/library/dn383662.aspx)  
  
 
## <a name="see-also"></a>Consulte também 

[Implantação do AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guia de implantação do Windows Server 2012 R2 AD FS](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Como implantar um farm de servidores de federação](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  

