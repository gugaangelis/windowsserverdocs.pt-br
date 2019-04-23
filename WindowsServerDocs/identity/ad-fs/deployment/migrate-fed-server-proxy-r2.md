---
title: Migrar o servidor de proxy do AD FS 2.0 federation
description: Fornece informações sobre como migrar um servidor de proxy do AD FS para Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 18ce084ec7d1b602dfca913372d6a0e279671a6e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867407"
---
# <a name="migrate-the-active-directory-federation-services-proxy-server-to-windows-server-2012-r2"></a>Migrar o servidor de Proxy de serviços de Federação do Active Directory para o Windows Server 2012 R2

No Active Directory Federation Services (AD FS) no Windows Server 2012 R2, a função de um proxy do servidor de Federação é tratada por um novo serviço de função acesso remoto chamado Proxy de aplicativo Web. No Windows Server 2012 R2, para habilitar o AD FS para acessibilidade de fora da rede corporativa, você pode implantar um ou mais Proxies de aplicativo Web. No entanto, você não pode migrar um proxy do servidor de Federação em execução no Windows Server 2008 R2 ou Windows Server 2012 para um Proxy de aplicativo Web em execução no Windows Server 2012 R2.  
  
> [!IMPORTANT]
>  Não há suporte para a migração de um proxy do servidor de Federação em execução no Windows Server 2008, Windows Server 2008 R2 ou Windows Server 2012 para um Proxy de aplicativo Web em execução no Windows Server 2012 R2.  
  
Se você quiser configurar o AD FS em um farm migrado do Windows Server 2012 R2 para acesso à extranet, você deve executar uma implantação nova de um ou mais computadores de Proxy de aplicativo Web como parte da infraestrutura do AD FS.  
  
Para planejar a implantação do Proxy de Aplicativo Web, é possível analisar as informações nos tópicos a seguir:  
  
-   [Planejar a infraestrutura de Proxy de aplicativo Web](https://technet.microsoft.com/library/dn383648.aspx)  
  
-   [Planejar o servidor de Proxy de aplicativo Web](https://technet.microsoft.com/library/dn383647.aspx)  
  
 Para implantar o Proxy de Aplicativo Web, você pode seguir os procedimentos dos tópicos a seguir:  
  
-   [Configurar a infraestrutura de Proxy de aplicativo Web](https://technet.microsoft.com/library/dn383644.aspx)  
  
-   [Instalar e configurar o servidor de Proxy de aplicativo Web](https://technet.microsoft.com/library/dn383662.aspx)  
  
## <a name="next-steps"></a>Próximas etapas
 [Migrar Serviços de função de serviços de Federação do Active Directory para o Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Preparando para migrar o servidor de Federação do AD FS](prepare-migrate-ad-fs-server-r2.md)   
 [Migrando o servidor de Federação do AD FS](migrate-ad-fs-fed-server-r2.md)    
 [Verificando a migração do AD FS para o Windows Server 2012 R2](verify-ad-fs-migration.md)

