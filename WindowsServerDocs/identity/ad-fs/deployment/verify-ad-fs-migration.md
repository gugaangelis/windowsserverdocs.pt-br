---
title: Verifique a migração do AD FS 2,0 para o Windows Server 2012 R2
description: Fornece informações sobre como migrar um servidor de AD FS para o Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.openlocfilehash: 35216baafefc5b304e1fc27b48f99b3afcde32fc
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87940443"
---
# <a name="verify-the-ad-fs-20-migration-to-windows-server-2012-r2"></a>Verifique a migração do AD FS 2,0 para o Windows Server 2012 R2

Depois de concluir a migração do mesmo servidor do seu farm do Active Directory Serviço de Federação (AD FS) para o Windows Server 2012 R2, você pode usar o procedimento a seguir para verificar se os servidores de Federação em seu farm estão operacionais; ou seja, qualquer cliente na mesma rede pode acessar seus servidores federtation.

Uma associação em **Usuários**, **Operadores de Backup**, **Usuários Avançados**, **Administradores** ou equivalente no computador local é o mínimo necessário para concluir este procedimento.

### <a name="to-verify-that-a-federation-server-is-operational"></a>Para verificar se um servidor de federação está operacional

1.  Abra uma janela do navegador e, na barra de endereços, digite o nome dos servidores de Federação e, em seguida, anexe-o `federationmetadata/2007-06/federationmetadata.xml` a para navegar até o ponto de extremidade de metadados do serviço de Federação. Por exemplo, `https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml` .

Se na janela do seu navegador você conseguir ver os metadados do servidor de federação sem nenhum aviso ou erro de SSL, o servidor de federação estará operacional.

2. Você também poderá navegar até a página de entrada do AD FS (o nome do seu serviço de federação acrescentado de `adfs/ls/idpinitiatedsignon.htm`, por exemplo, `https://fs.contoso.com/adfs/ls/idpinitiatedsignon.htm`).  Esse endereço exibe a página de entrada do AD FS, onde você poderá entrar com credenciais de administrador de domínio.

> [!IMPORTANT]
>  Certifique-se de definir as configurações do navegador para confiar na função do servidor de Federação adicionando o nome do serviço de Federação (por exemplo, `https://fs.contoso.com` ) à zona da intranet local do navegador.

## <a name="next-steps"></a>Próximas etapas
 [Migrar serviços de Federação do Active Directory (AD FS) serviços de função para o Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md) [preparando para migrar o servidor de Federação AD FS](prepare-migrate-ad-fs-server-r2.md) [migrando o servidor de Federação AD FS](migrate-ad-fs-fed-server-r2.md) [migrando o proxy do servidor de Federação AD FS](migrate-fed-server-proxy-r2.md)
