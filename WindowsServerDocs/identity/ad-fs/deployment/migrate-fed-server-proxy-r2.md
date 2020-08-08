---
title: Migrar o servidor proxy de Federação AD FS 2,0
description: Fornece informações sobre como migrar um servidor proxy AD FS para o Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.openlocfilehash: a8345f587e7fe4119e46dc390e9240dee6071ce6
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87940829"
---
# <a name="migrate-the-active-directory-federation-services-proxy-server-to-windows-server-2012-r2"></a>Migrar o servidor proxy Serviços de Federação do Active Directory (AD FS) para o Windows Server 2012 R2

No Serviços de Federação do Active Directory (AD FS) (AD FS) no Windows Server 2012 R2, a função de um proxy de servidor de Federação é tratada por um novo serviço de função de acesso remoto chamado proxy de aplicativo Web. No Windows Server 2012 R2, para habilitar sua AD FS para acessibilidade de fora da rede corporativa, você pode implantar um ou mais proxies de aplicativo Web. No entanto, você não pode migrar um proxy de servidor de Federação em execução no Windows Server 2008 R2 ou no Windows Server 2012 para um proxy de aplicativo Web em execução no Windows Server 2012 R2.

> [!IMPORTANT]
>  Não há suporte para a migração de um proxy de servidor de Federação em execução no Windows Server 2008, no Windows Server 2008 R2 ou no Windows Server 2012 para um proxy de aplicativo Web em execução no Windows Server 2012 R2.

Se desejar configurar AD FS em um farm migrado do Windows Server 2012 R2 para acesso à extranet, você deverá executar uma implantação nova de um ou mais computadores de proxy de aplicativo Web como parte de sua infraestrutura de AD FS.

Para planejar a implantação do Proxy de Aplicativo Web, é possível analisar as informações nos tópicos a seguir:

- [Planejar a infraestrutura do proxy de aplicativo Web](/previous-versions/orphan-topics/ws.11/dn383648(v=ws.11))

- [planejar o Servidor Proxy de Aplicativo Web](/previous-versions/orphan-topics/ws.11/dn383647(v=ws.11))

  Para implantar o Proxy de Aplicativo Web, você pode seguir os procedimentos dos tópicos a seguir:

- [configurar a infraestrutura do Proxy de Aplicativo Web](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn383644(v=ws.11))

- [instalar e configurar o servidor do proxy de aplicativo Web](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn383662(v=ws.11))

## <a name="next-steps"></a>Próximas etapas
 [Migre serviços de Federação do Active Directory (AD FS) serviços de função para o Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md) [preparando-se para migrar o servidor de Federação AD FS](prepare-migrate-ad-fs-server-r2.md) [migrando o AD FS servidor de Federação](migrate-ad-fs-fed-server-r2.md) [verificando a migração de AD FS para o Windows Server 2012 R2](verify-ad-fs-migration.md)
