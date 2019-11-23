---
title: Migrar o servidor proxy de Federação AD FS 2,0
description: Fornece informações sobre como migrar um servidor proxy AD FS para o Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 57367cadd3c7ce3d031c6eb3a53c333422543dae
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359375"
---
# <a name="migrate-the-active-directory-federation-services-proxy-server-to-windows-server-2012-r2"></a>Migrar o servidor proxy Serviços de Federação do Active Directory (AD FS) para o Windows Server 2012 R2

No Serviços de Federação do Active Directory (AD FS) (AD FS) no Windows Server 2012 R2, a função de um proxy de servidor de Federação é tratada por um novo serviço de função de acesso remoto chamado proxy de aplicativo Web. No Windows Server 2012 R2, para habilitar sua AD FS para acessibilidade de fora da rede corporativa, você pode implantar um ou mais proxies de aplicativo Web. No entanto, você não pode migrar um proxy de servidor de Federação em execução no Windows Server 2008 R2 ou no Windows Server 2012 para um proxy de aplicativo Web em execução no Windows Server 2012 R2.  
  
> [!IMPORTANT]
>  Não há suporte para a migração de um proxy de servidor de Federação em execução no Windows Server 2008, no Windows Server 2008 R2 ou no Windows Server 2012 para um proxy de aplicativo Web em execução no Windows Server 2012 R2.  
  
Se desejar configurar AD FS em um farm migrado do Windows Server 2012 R2 para acesso à extranet, você deverá executar uma implantação nova de um ou mais computadores de proxy de aplicativo Web como parte de sua infraestrutura de AD FS.  
  
Para planejar a implantação do Proxy de Aplicativo Web, é possível analisar as informações nos tópicos a seguir:  
  
- [Planejar a infraestrutura do proxy de aplicativo Web](https://technet.microsoft.com/library/dn383648.aspx)  
  
- [Planejar o servidor proxy de aplicativo Web](https://technet.microsoft.com/library/dn383647.aspx)  
  
  Para implantar o Proxy de Aplicativo Web, você pode seguir os procedimentos dos tópicos a seguir:  
  
- [Configurar a infraestrutura de proxy de aplicativo Web](https://technet.microsoft.com/library/dn383644.aspx)  
  
- [Instalar e configurar o servidor proxy de aplicativo Web](https://technet.microsoft.com/library/dn383662.aspx)  
  
## <a name="next-steps"></a>Próximas etapas
 [Migrar serviços de Federação do Active Directory (AD FS) serviços de função para o Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Preparando para migrar o servidor de federação AD FS](prepare-migrate-ad-fs-server-r2.md)   
 [Migrando o servidor de federação AD FS](migrate-ad-fs-fed-server-r2.md)    
 [Verificando a migração de AD FS para o Windows Server 2012 R2](verify-ad-fs-migration.md)

