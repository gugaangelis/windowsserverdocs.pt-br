---
title: Migrar Serviços de função de Serviços de Federação do Active Directory (AD FS) para o Windows Server 2012 R2
description: Fornece instruções para migrar o serviço AD FS para Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 478729a7b6560beba5f04a1a15ad035561ad31f0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847947"
---
# <a name="migrate-active-directory-federation-services-role-services-to-windows-server-2012-r2"></a>Migrar Serviços de função de Serviços de Federação do Active Directory (AD FS) para o Windows Server 2012 R2
 Este documento fornece instruções para migrar os seguintes serviços de função para serviços de Federação do Active Directory (AD FS) que é instalado com o Windows Server 2012 R2:  
  
-   Servidor de Federação do AD FS 2.0 instalado no Windows Server 2008 ou Windows Server 2008 R2  
  
-   Servidor de Federação do AD FS instalado no Windows Server 2012  
  
## <a name="supported-migration-scenarios"></a>Cenários de migração com suporte  
 As instruções de migração deste guia consistem nas seguintes tarefas:  
  
-   Exportando os dados do AD FS 2.0 de configuração do seu servidor que está executando o Windows Server 2008, Windows Server 2008 R2 ou Windows Server 2012  
  
-   Executando uma atualização in-loco do sistema operacional desse servidor do Windows Server 2008, Windows Server 2008 R2 ou Windows Server 2012 para Windows Server 2012 R2. 
  
-   Recriar a configuração original do AD FS e restaurar o restante do AD FS do serviço configurações nesse servidor, que agora está executando a função de servidor do AD FS é instalada com o Windows Server 2012 R2.  
  
 Este guia não inclui instruções para migrar um servidor que está executando várias funções. Se o servidor executar várias funções, é recomendável criar um processo de migração personalizado específico para o ambiente do servidor, com base nas informações fornecidas em outros guias de migração de função. Guias de migração de funções adicionais estão disponíveis no [Portal de Migração do Windows Server](https://go.microsoft.com/fwlink/?LinkId=247608).  
  
### <a name="supported-operating-systems"></a>Sistemas operacionais compatíveis  
 Sistema de operacional de servidor de destino:  
  
 Windows Server 2012 R2 (opções de instalação completa e Server Core)  
  
 Processador do servidor de destino:  
  
 Com base em x64  
  
|Processador do servidor de origem|Sistema operacional do servidor de origem|  
|-----------------------------|------------------------------------|  
|Com base em x86 ou x64| Windows Server 2008, completo e as opções de instalação do Server Core|  
|Com base em x64|Windows Server 2008 R2|  
|Com base em x64|Opção de instalação do Server Core do Windows Server 2008 R2|  
|Com base em x64|Opções de instalação completa do Windows Server 2012 e Server Core|  
  
> [!NOTE]
>  -   As versões dos sistemas operacionais mostradas na tabela anterior são as combinações mais antigas de sistemas operacionais e service packs com suporte.  
> -   As edições Foundation, Standard, Enterprise e Datacenter do sistema operacional Windows Server têm suporte como a origem ou o servidor de destino.  
> -   Também há suporte para migrações entre sistemas operacionais físicos e virtuais.  
  
### <a name="supported-ad-fs-role-services-and-features"></a>Serviços de função do AD FS e recursos com suporte  
 A tabela a seguir descreve os cenários de migração de serviços de função do AD FS e as respectivas configurações descritas neste guia.  
  
|De|Para o AD FS instalado com o Windows Server 2012 R2|  
|----------|----------------------------------------------------------------------------------------------|  
|Servidor de Federação do AD FS 2.0 instalado no Windows Server 2008 ou Windows Server 2008 R2|Há suporte para migração no mesmo servidor. Para obter mais informações, consulte:<br /><br /> [Preparando para migrar o servidor de Federação do AD FS](prepare-migrate-ad-fs-server-r2.md)<br /><br /> [Migrando o servidor de Federação do AD FS](migrate-ad-fs-fed-server-r2.md)|  
|Servidor de Federação do AD FS instalado no Windows Server 2012|Há suporte para migração no mesmo servidor.  Para saber mais, confira:<br /><br /> [Preparando para migrar o servidor de Federação do AD FS](prepare-migrate-ad-fs-server-r2.md)<br /><br /> [Migrando o servidor de Federação do AD FS](migrate-ad-fs-fed-server-r2.md)|  
  
## <a name="next-steps"></a>Próximas etapas
 [Preparando para migrar o servidor de Federação do AD FS](prepare-migrate-ad-fs-server-r2.md)   
 [Migrando o servidor de Federação do AD FS](migrate-ad-fs-fed-server-r2.md)   
 [Migrar o Proxy de servidor de Federação do AD FS](migrate-fed-server-proxy-r2.md)   
 [Verificando a migração do AD FS para o Windows Server 2012 R2](verify-ad-fs-migration.md)