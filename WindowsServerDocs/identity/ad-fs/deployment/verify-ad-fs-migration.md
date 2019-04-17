---
title: "Migrar o servidor de Federação 2.0 do AD FS"
description: "Fornece informações sobre como migrar um servidor do AD FS para Windows Server 2012 R2."
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0a3e8e444f715fe2ae0f0ccd858d90e8664be00c
ms.sourcegitcommit: 03ce78a1624dbd7f4e6abf2ec1ef185b55de29a1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/12/2017
---
# <a name="verify-the-ad-fs-20-migration-to-windows-server-2012-r2"></a>Verifique se o AD FS 2.0 migração para o Windows Server 2012 R2

Depois de concluir a migração mesmo do servidor do seu farm do serviço de Federação do Active Directory (AD FS) para Windows Server 2012 R2, você pode usar o procedimento a seguir para verificar se os servidores de Federação do farm estão operacionais; ou seja, que qualquer cliente na mesma rede pode acessar seus servidores federtation.  
  
A associação ao grupo **usuários**, **operadores de Backup**, **usuários avançados**, **administradores** ou equivalente, no computador local é o requisito mínimo para concluir este procedimento.
  
### <a name="to-verify-that-a-federation-server-is-operational"></a>Para verificar se um servidor de Federação está operacional  
  
1.  Abra uma janela do navegador e na barra de endereços, digite o nome de servidores de Federação e acrescentá-lo com `federationmetadata/2007-06/federationmetadata.xml` para navegar até o ponto de extremidade de metadados de serviço de Federação. Por exemplo, `https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml` .  
  
Se a janela do navegador, você pode ver os metadados de servidor de Federação sem SSL erros ou avisos, o servidor de Federação está operacional.  
  
2.  Você também pode navegar para a página de entrada do AD FS (seu nome de serviço de Federação junto com `adfs/ls/idpinitiatedsignon.htm`, por exemplo, `https://fs.contoso.com/adfs/ls/idpinitiatedsignon.htm`).  Isso exibe a AD FS entrada na página onde você pode entrar com credenciais de administrador do domínio.  
  
> [!IMPORTANT]
>  Certifique-se de definir as configurações do navegador para confiar a função de servidor de Federação adicionando seu nome de serviço de Federação (por exemplo, `https://fs.contoso.com`) à zona de intranet local do navegador.  
  
## <a name="next-steps"></a>Próximas etapas
 [Migrar Serviços de função de serviços de Federação do Active Directory para Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Preparando para migrar o servidor de Federação do AD FS](prepare-migrate-ad-fs-server-r2.md)  
 [Migrando o servidor de Federação do AD FS](migrate-ad-fs-fed-server-r2.md)   
 [Migrando o Proxy de servidor de Federação do AD FS](migrate-fed-server-proxy-r2.md)   
