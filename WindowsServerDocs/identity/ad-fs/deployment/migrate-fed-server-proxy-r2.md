---
title: "Migrar o servidor de proxy de Federação 2.0 do AD FS"
description: "Fornece informações sobre como migrar um servidor de proxy do AD FS para Windows Server 2012 R2."
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 33ab29fd5efdb0bdd1fe25580e3f4434071e1c7d
ms.sourcegitcommit: 03ce78a1624dbd7f4e6abf2ec1ef185b55de29a1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/12/2017
---
# <a name="migrate-the-active-directory-federation-services-proxy-server-to-windows-server-2012-r2"></a>Migrar o servidor de Proxy de serviços de Federação do Active Directory para Windows Server 2012 R2

No Active Directory serviços de Federação (AD FS) no Windows Server 2012 R2, a função de um proxy de servidor de Federação é manipulada por um novo serviço de função de acesso remoto chamado Proxy de aplicativo Web. No Windows Server 2012 R2, para habilitar o AD FS para acessibilidade de fora da rede corporativa, você pode implantar um ou mais Proxies de aplicativo Web. No entanto, você não pode migrar um proxy de servidor de Federação em execução no Windows Server 2008 R2 ou Windows Server 2012 para um Proxy de aplicativo Web em execução no Windows Server 2012 R2.  
  
> [!IMPORTANT]
>  Não há suporte para a migração de um proxy de servidor de Federação em execução no Windows Server 2008, Windows Server 2008 R2 ou Windows Server 2012 para um Proxy de aplicativo Web em execução no Windows Server 2012 R2.  
  
Se você quiser configurar o AD FS em um farm migrado do Windows Server 2012 R2 para acesso à extranet, você deve executar uma implantação atualizada de um ou mais computadores de Proxy de aplicativo Web como parte de sua infraestrutura do AD FS.  
  
Para planejar a implantação do Proxy de aplicativo Web, você pode examinar as informações nos tópicos a seguir:  
  
-   [Planejar a infraestrutura de Proxy de aplicativo Web](https://technet.microsoft.com/en-us/library/dn383648.aspx)  
  
-   [Planeje o servidor de Proxy de aplicativo Web](https://technet.microsoft.com/en-us/library/dn383647.aspx)  
  
 Para implantar o proxy de aplicativo Web, você pode seguir os procedimentos nos tópicos a seguir:  
  
-   [Configurar a infraestrutura de Proxy de aplicativo Web](https://technet.microsoft.com/en-us/library/dn383644.aspx)  
  
-   [Instalar e configurar o servidor de Proxy de aplicativo Web](https://technet.microsoft.com/en-us/library/dn383662.aspx)  
  
## <a name="next-steps"></a>Próximas etapas
 [Migrar Serviços de função de serviços de Federação do Active Directory para Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Preparando para migrar o servidor de Federação do AD FS](prepare-migrate-ad-fs-server-r2.md)   
 [Migrando o servidor de Federação do AD FS](migrate-ad-fs-fed-server-r2.md)    
 [Verificando o AD FS migração ao Windows Server 2012 R2](verify-ad-fs-migration.md)

