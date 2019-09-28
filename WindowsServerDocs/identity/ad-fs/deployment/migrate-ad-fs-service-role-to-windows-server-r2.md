---
title: Migrar Serviços de função de Serviços de Federação do Active Directory (AD FS) para o Windows Server 2012 R2
description: Fornece instruções para migrar o serviço de AD FS para o Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 2a3cf6cd523f5cfd69785104fed7aa3938d79525
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359394"
---
# <a name="migrate-active-directory-federation-services-role-services-to-windows-server-2012-r2"></a>Migrar Serviços de função de Serviços de Federação do Active Directory (AD FS) para o Windows Server 2012 R2
 Este documento fornece instruções para migrar os seguintes serviços de função para Serviços de Federação do Active Directory (AD FS) (AD FS) instalado com o Windows Server 2012 R2:  
  
-   AD FS servidor de Federação 2,0 instalado no Windows Server 2008 ou no Windows Server 2008 R2  
  
-   AD FS servidor de Federação instalado no Windows Server 2012  
  
## <a name="supported-migration-scenarios"></a>Cenários de migração com suporte  
 As instruções de migração deste guia consistem nas seguintes tarefas:  
  
- Exportando os dados de configuração do AD FS 2,0 do servidor que está executando o Windows Server 2008, o Windows Server 2008 R2 ou o Windows Server 2012  
  
- Executando uma atualização in-loco do sistema operacional desse servidor do Windows Server 2008, do Windows Server 2008 R2 ou do Windows Server 2012 para o Windows Server 2012 R2. 
  
- Recriar a configuração de AD FS original e restaurar as configurações de serviço AD FS restantes nesse servidor, que agora está executando a função de servidor AD FS que é instalada com o Windows Server 2012 R2.  
  
  Este guia não inclui instruções para migrar um servidor que está executando várias funções. Se o servidor executar várias funções, é recomendável criar um processo de migração personalizado específico para o ambiente do servidor, com base nas informações fornecidas em outros guias de migração de função. Guias de migração de funções adicionais estão disponíveis no [Portal de Migração do Windows Server](https://go.microsoft.com/fwlink/?LinkId=247608).  
  
### <a name="supported-operating-systems"></a>Sistemas operacionais com suporte  
 Sistema operacional do servidor de destino:  
  
 Windows Server 2012 R2 (Server Core e opções de instalação completa)  
  
 Processador do servidor de destino:  
  
 Com base em x64  
  
|Processador do servidor de origem|Sistema operacional do servidor de origem|  
|-----------------------------|------------------------------------|  
|Com base em x86 ou x64| Windows Server 2008, opções de instalação completa e Server Core|  
|Com base em x64|Windows Server 2008 R2|  
|Com base em x64|Opção de instalação Server Core do Windows Server 2008 R2|  
|Com base em x64|Server Core e opções de instalação completa do Windows Server 2012|  
  
> [!NOTE]
> - As versões dos sistemas operacionais mostradas na tabela anterior são as combinações mais antigas de sistemas operacionais e service packs com suporte.  
>   -   As edições Foundation, Standard, Enterprise e Datacenter do sistema operacional Windows Server têm suporte como a origem ou o servidor de destino.  
>   -   Também há suporte para migrações entre sistemas operacionais físicos e virtuais.  
  
### <a name="supported-ad-fs-role-services-and-features"></a>Recursos e serviços de função AD FS com suporte  
 A tabela a seguir descreve os cenários de migração do AD FS serviços de função e suas respectivas configurações descritas neste guia.  
  
|De|Para AD FS instalado com o Windows Server 2012 R2|  
|----------|----------------------------------------------------------------------------------------------|  
|AD FS servidor de Federação 2,0 instalado no Windows Server 2008 ou no Windows Server 2008 R2|Há suporte para migração no mesmo servidor. Para obter mais informações, consulte:<br /><br /> [Preparando para migrar o servidor de Federação AD FS](prepare-migrate-ad-fs-server-r2.md)<br /><br /> [Migrando o servidor de Federação de AD FS](migrate-ad-fs-fed-server-r2.md)|  
|AD FS servidor de Federação instalado no Windows Server 2012|Há suporte para migração no mesmo servidor.  Para saber mais, confira:<br /><br /> [Preparando para migrar o servidor de Federação AD FS](prepare-migrate-ad-fs-server-r2.md)<br /><br /> [Migrando o servidor de Federação de AD FS](migrate-ad-fs-fed-server-r2.md)|  
  
## <a name="next-steps"></a>Próximas etapas
 [Preparando para migrar o servidor de Federação AD FS](prepare-migrate-ad-fs-server-r2.md)   
 [Migrando o servidor de Federação de AD FS](migrate-ad-fs-fed-server-r2.md)   
 [Migrando o proxy do servidor de Federação AD FS](migrate-fed-server-proxy-r2.md)   
 [Verificando a migração de AD FS para o Windows Server 2012 R2](verify-ad-fs-migration.md)