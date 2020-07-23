---
ms.assetid: 222e9f93-7c41-4527-8a98-8f7fbc7a58af
title: Implantação de proxies de servidores de federação
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 445f3b6fef526cb5dd308be8aafe49a3ecddd6a4
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86965968"
---
# <a name="deploying-federation-server-proxies"></a>Implantação de proxies de servidores de federação

No Serviços de Federação do Active Directory (AD FS) \( AD FS \) no Windows Server 2012 R2, a função de um proxy de servidor de Federação é tratada por um novo serviço de função de acesso remoto chamado proxy de aplicativo Web. Para habilitar sua AD FS para acessibilidade de fora da rede corporativa, que foi a finalidade de implantar um proxy de servidor de Federação em versões herdadas do AD FS, como AD FS 2,0 e AD FS no Windows Server 2012, você pode implantar um ou mais proxies de aplicativo Web para AD FS no Windows Server 2012 R2.  
  
No contexto de AD FS, o proxy de aplicativo Web funciona como um proxy de servidor de Federação AD FS. Além disso, o Proxy de Aplicativo Web fornece funcionalidade de proxy reverso para aplicativos Web dentro da rede corporativa. Isso permite aos usuários acessá-los de qualquer dispositivo fora da rede corporativa. Para obter mais informações sobre o serviço da função Proxy de Aplicativo Web, consulte Visão geral de proxy de aplicativo Web.  
  
Para planejar a implantação do proxy de Aplicativo Web, é possível examinar as informações nos tópicos a seguir:  
  
-   [Planejar a WAP (infraestrutura de proxy de aplicativo Web)](/previous-versions/orphan-topics/ws.11/dn383648(v=ws.11))  
  
-   [planejar o Servidor Proxy de Aplicativo Web](/previous-versions/orphan-topics/ws.11/dn383647(v=ws.11))  
  
Para implantar o Proxy de Aplicativo Web, você pode seguir os procedimentos dos tópicos a seguir:  
  
-   [configurar a infraestrutura do Proxy de Aplicativo Web](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn383644(v=ws.11))  
  
-   [instalar e configurar o servidor do proxy de aplicativo Web](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn383662(v=ws.11))  
  
 
## <a name="see-also"></a>Consulte Também 

[Implantação do AD FS](../../ad-fs/AD-FS-Deployment.md)  

[Guia de Implantação do AD FS do Windows Server 2012 R2](../../ad-fs/deployment/Windows-Server-2012-R2-AD-FS-Deployment-Guide.md)  
 
[Como implantar um farm de servidores de federação](../../ad-fs/deployment/Deploying-a-Federation-Server-Farm.md)  
  
