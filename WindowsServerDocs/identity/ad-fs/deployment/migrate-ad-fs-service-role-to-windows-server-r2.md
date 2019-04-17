---
title: "Migrar Serviços de função de serviços de Federação do Active Directory para Windows Server 2012 R2"
description: "Fornece instruções para migrar o serviço do AD FS para o Windows Server 2012 R2."
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 478729a7b6560beba5f04a1a15ad035561ad31f0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="migrate-active-directory-federation-services-role-services-to-windows-server-2012-r2"></a>Migrar Serviços de função de serviços de Federação do Active Directory para Windows Server 2012 R2
 Este documento fornece instruções para migrar os seguintes serviços de função Serviços de Federação do Active Directory (AD FS) que é instalada com o Windows Server 2012 R2:  
  
-   Servidor de Federação do AD FS 2.0 instalado no Windows Server 2008 ou Windows Server 2008 R2  
  
-   Servidor de Federação do AD FS instalada no Windows Server 2012  
  
## <a name="supported-migration-scenarios"></a>Cenários de migração com suporte  
 As instruções de migração neste guia consistem as seguintes tarefas:  
  
-   Exportando os dados de configuração 2.0 AD FS do seu servidor que está executando o Windows Server 2008, Windows Server 2008 R2 ou Windows Server 2012  
  
-   Executando uma atualização in-loco do sistema operacional do servidor do Windows Server 2008, Windows Server 2008 R2 ou Windows Server 2012 para Windows Server 2012 R2. 
  
-   Recriar a configuração original do AD FS e restaurando o restante do AD FS serviço configurações nesse servidor, que agora está executando a função de servidor do AD FS que é instalada com o Windows Server 2012 R2.  
  
 Este guia não inclui instruções para migrar um servidor que executa várias funções. Se o servidor está executando várias funções, recomendamos que você projete um processo de migração personalizados específico para o seu ambiente de servidor, com base nas informações fornecidas nas outros guias de migração de função. Guias de migração para funções adicionais estão disponíveis no [Portal de migração do Windows Server](https://go.microsoft.com/fwlink/?LinkId=247608).  
  
### <a name="supported-operating-systems"></a>Sistemas operacionais com suporte  
 Sistema de operacional de servidor de destino:  
  
 Windows Server 2012 R2 (Server Core e opções de instalação completa)  
  
 Processador de servidor de destino:  
  
 Com base em x64  
  
|Processador de servidor de origem|Sistema de operacional de servidor de origem|  
|-----------------------------|------------------------------------|  
|x86-ou x64-com base em| Windows Server 2008, completo e opções de instalação Server Core|  
|Com base em x64|Windows Server 2008 R2|  
|Com base em x64|Opção de instalação Server Core do Windows Server 2008 R2|  
|Com base em x64|Opções de instalação completa do Windows Server 2012 e núcleo do servidor|  
  
> [!NOTE]
>  -   As versões dos sistemas operacionais que estão listados na tabela anterior são mais antigas combinações de sistemas operacionais e service packs com suporte.  
> -   As edições Foundation, Standard, Enterprise e Datacenter do sistema operacional Windows Server são aceitas como a origem ou o servidor de destino.  
> -   Migrações entre sistemas operacionais físicos e virtuais sistemas operacionais são compatíveis.  
  
### <a name="supported-ad-fs-role-services-and-features"></a>Recursos e serviços de função do AD FS com suporte  
 A tabela a seguir descreve os cenários de migração de serviços de função do AD FS e as respectivas configurações descritas neste guia.  
  
|De|Para o AD FS instalada com o Windows Server 2012 R2|  
|----------|----------------------------------------------------------------------------------------------|  
|Servidor de Federação do AD FS 2.0 instalado no Windows Server 2008 ou Windows Server 2008 R2|Há suporte para migração no mesmo servidor. Para obter mais informações, consulte:<br /><br /> [Preparando para migrar o servidor de Federação do AD FS](prepare-migrate-ad-fs-server-r2.md)<br /><br /> [Migrando o servidor de Federação do AD FS](migrate-ad-fs-fed-server-r2.md)|  
|Servidor de Federação do AD FS instalada no Windows Server 2012|Há suporte para migração no mesmo servidor.  Para obter mais informações, consulte:<br /><br /> [Preparando para migrar o servidor de Federação do AD FS](prepare-migrate-ad-fs-server-r2.md)<br /><br /> [Migrando o servidor de Federação do AD FS](migrate-ad-fs-fed-server-r2.md)|  
  
## <a name="next-steps"></a>Próximas etapas
 [Preparando para migrar o servidor de Federação do AD FS](prepare-migrate-ad-fs-server-r2.md)   
 [Migrando o servidor de Federação do AD FS](migrate-ad-fs-fed-server-r2.md)   
 [Migrando o Proxy de servidor de Federação do AD FS](migrate-fed-server-proxy-r2.md)   
 [Verificando o AD FS migração ao Windows Server 2012 R2](verify-ad-fs-migration.md)