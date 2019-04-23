---
title: Migrar o servidor do AD FS 2.0 federation
description: Fornece informações sobre como migrar um servidor do AD FS para Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0a3e8e444f715fe2ae0f0ccd858d90e8664be00c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877767"
---
# <a name="verify-the-ad-fs-20-migration-to-windows-server-2012-r2"></a>Verifique se o AD FS 2.0 migração para o Windows Server 2012 R2

Depois de concluir a migração do mesmo servidor do farm do serviço de Federação do Active Directory (AD FS) para Windows Server 2012 R2, você pode usar o procedimento a seguir para verificar se os servidores de federação no farm estão operacionais; ou seja, se qualquer cliente na mesma rede pode acessar seus servidores federtation.  
  
Uma associação em **Usuários**, **Operadores de Backup**, **Usuários Avançados**, **Administradores** ou equivalente no computador local é o mínimo necessário para concluir este procedimento.
  
### <a name="to-verify-that-a-federation-server-is-operational"></a>Para verificar se um servidor de federação está operacional  
  
1.  Abra uma janela do navegador na barra de endereços, digite o nome de servidores de Federação e, em seguida, acrescente `federationmetadata/2007-06/federationmetadata.xml` para navegar até o ponto de extremidade de metadados de serviço de Federação. Por exemplo, `https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml` .  
  
Se na janela do seu navegador você conseguir ver os metadados do servidor de federação sem nenhum aviso ou erro de SSL, o servidor de federação estará operacional.  
  
2.  Você também poderá navegar até a página de entrada do AD FS (o nome do seu serviço de federação acrescentado de `adfs/ls/idpinitiatedsignon.htm`, por exemplo, `https://fs.contoso.com/adfs/ls/idpinitiatedsignon.htm`).  Esse endereço exibe a página de entrada do AD FS, onde você poderá entrar com credenciais de administrador de domínio.  
  
> [!IMPORTANT]
>  Defina as configurações do navegador para confiar na função do servidor de federação, adicionando o nome do serviço de federação (por exemplo, `https://fs.contoso.com`) à zona de intranet local do navegador.  
  
## <a name="next-steps"></a>Próximas etapas
 [Migrar Serviços de função de serviços de Federação do Active Directory para o Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Preparando para migrar o servidor de Federação do AD FS](prepare-migrate-ad-fs-server-r2.md)  
 [Migrando o servidor de Federação do AD FS](migrate-ad-fs-fed-server-r2.md)   
 [Migrar o Proxy de servidor de Federação do AD FS](migrate-fed-server-proxy-r2.md)   
