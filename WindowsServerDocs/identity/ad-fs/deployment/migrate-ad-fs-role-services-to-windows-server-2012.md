---
title: Migrar a Função Serviços de Federação do Active Directory para o Windows Server 2012
description: Fornece instruções sobre como migrar o serviço AD FS para o Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2b44ed504c2b3dc8a8ac0ca9648f1db9e362e075
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850297"
---
# <a name="migrate-active-directory-federation-services-role-services-to-windows-server-2012"></a>Migrar a Função Serviços de Federação do Active Directory para o Windows Server 2012

O exemplo a seguir fornece instruções sobre como migrar os seguintes serviços de função Serviços de Federação do Active Directory (AD FS) no Windows Server 2012:  
  
-   Agente baseado em token do AD FS 1.1 Windows e o AD FS 1.1 agente de reconhecimento de declarações instalado com o Windows Server 2008 ou Windows Server 2008 R2  
  
-   Servidor de Federação do AD FS 2.0 e proxy do servidor de Federação 2.0 do AD FS instalado no Windows Server 2008 ou Windows Server 2008 R2    
  
## <a name="supported-migration-scenarios"></a>Cenários de migração com suporte  
 As instruções de migração contém as seguintes tarefas:  
  
-   Exportando os dados do AD FS 2.0 de configuração do seu servidor que está executando o Windows Server 2008 ou Windows Server 2008 R2  
  
-   Executando uma atualização in-loco do sistema operacional desse servidor do Windows Server 2008 ou Windows Server 2008 R2 para o Windows Server 2012.
  
-   Recriar a configuração original do AD FS e restaurar o restante do AD FS do serviço configurações nesse servidor, que agora está executando a função de servidor do AD FS é instalada com o Windows Server 2012.  
  
 Este guia não inclui instruções para migrar um servidor que está executando várias funções. Se o servidor executar várias funções, é recomendável criar um processo de migração personalizado específico para o ambiente do servidor, com base nas informações fornecidas em outros guias de migração de função. Guias de migração de funções adicionais estão disponíveis no [Portal de Migração do Windows Server](https://go.microsoft.com/fwlink/?LinkId=247608).  
  
## <a name="supported-operating-systems"></a>Sistemas operacionais compatíveis  
 **Sistema de operacional de servidor de destino:**  
  

-  Windows Server 2012 ou Windows Server 2008 R2 (opções de instalação completa e Server Core)  
  
 **Processador do servidor de destino:**  
  

-  Com base em x64  
  
|Processador do servidor de origem|Sistema operacional do servidor de origem|  
|-----|-----|  
|Com base em x86 ou x64|Windows Server 2003 com Service Pack 2|  
|Com base em x86 ou x64|Windows Server 2003 R2|  
|Com base em x86 ou x64|Windows Server 2008, completo e as opções de instalação do Server Core|  
|Com base em x64|Windows Server 2008 R2|  
|Com base em x64|Opção de instalação do Server Core do Windows Server 2008 R2|  
|Com base em x64|Opções de instalação completa do Windows Server 2012 e Server Core|  
  
> [!NOTE]
>  -   As versões dos sistemas operacionais mostradas na tabela anterior são as combinações mais antigas de sistemas operacionais e service packs com suporte.  
> -   As edições Foundation, Standard, Enterprise e Datacenter do sistema operacional Windows Server têm suporte como a origem ou o servidor de destino.  
> -   Também há suporte para migrações entre sistemas operacionais físicos e virtuais.  
  
### <a name="supported-ad-fs-role-services-and-features"></a>Serviços de função do AD FS e recursos com suporte  
 A tabela a seguir descreve os cenários de migração de serviços de função do AD FS e as respectivas configurações descritas neste guia.  
  
|De|Para o AD FS instalado com o Windows Server 2012|  
|----------|-----|  
|Servidor de Federação do AD FS 1.0 instalado com o Windows Server 2003 R2|Não há suporte para migração|  
|Proxy do servidor AD FS 1.0 federação instalado com o Windows Server 2003 R2|Não há suporte para migração|  
|AD FS 1.0 Windows baseada em token agente do instalado com o Windows Server 2003 R2|Não há suporte para migração|  
|Agente de reconhecimento de declarações de FS 1.0 AD instalado com o Windows Server 2003 R2)|Não há suporte para migração|  
|Servidor de Federação do AD FS 1.1 instalado com o Windows Server 2008 ou Windows Server 2008 R2|Não há suporte para migração|  
|Proxy do servidor AD FS 1.1 federação instalado com o Windows Server 2008 ou Windows Server 2008 R2|Não há suporte para migração|  
|AD FS 1.1 Windows baseada em token agente do instalado com o Windows Server 2008 ou Windows Server 2008 R2|Há suporte para migração no mesmo servidor, mas o agente de baseada em token do Windows do AD FS migrado funcionará apenas com um serviço de Federação 1.1 do AD FS instalado com o Windows Server 2008 ou Windows Server 2008 R2. Para obter mais informações, consulte:<br /><br /> [Migrar os AD FS agentes Web 1.1](migrate-the-ad-fs-web-agent.md)<br /><br /> [Interoperação com o AD FS 1.x](Interoperating-with-AD-FS-1.x.md)|  
|Agente AD FS 1.1 compatível com declarações instalado com o Windows Server 2008 ou Windows Server 2008 R2)|Há suporte para migração no mesmo servidor. O agente de 1.1 web com reconhecimento de declarações do AD FS migrado funcionará com o seguinte:<br /><br /> Serviço de Federação do AD FS 1.1 instalado com o Windows Server 2008 ou Windows Server 2008 R2<br /><br /> Serviço do AD FS 2.0 federation instalado no Windows Server 2008 ou Windows Server 2008 R2<br /><br /> Serviço de Federação do AD FS instalado com o Windows Server 2012<br /><br /> Para obter mais informações, consulte:<br /><br /> [Migrar os AD FS agentes Web 1.1](migrate-the-ad-fs-web-agent.md)<br /><br /> [Interoperação com o AD FS 1.x](Interoperating-with-AD-FS-1.x.md)|  
|Servidor de Federação do AD FS 2.0 instalado no Windows Server 2008 ou Windows Server 2008 R2|Há suporte para migração no mesmo servidor. Para obter mais informações, consulte:<br /><br /> [Preparar para migrar o servidor do AD FS 2.0 Federation](prepare-to-migrate-ad-fs-fed-server.md)<br /><br /> [Migrar o servidor do AD FS 2.0 Federation](migrate-the-ad-fs-fed-server.md)|  
|Proxy do AD FS 2.0 federation server instalado no Windows Server 2008 ou Windows Server 2008 R2|Há suporte para migração no mesmo servidor.  Para saber mais, confira:<br /><br /> [Preparar para migrar o Proxy do AD FS 2.0 Federation Server](prepare-to-migrate-ad-fs-fed-proxy.md)<br /><br /> [Migrar o Proxy do AD FS 2.0 Federation Server](migrate-the-ad-fs-2-fed-server-proxy.md)|  
  
## <a name="see-also"></a>Consulte também  
 [Preparar para migrar o servidor do AD FS 2.0 Federation](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparar para migrar o Proxy do AD FS 2.0 Federation Server](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar o servidor do AD FS 2.0 Federation](migrate-the-ad-fs-fed-server.md)   
 [Migrar o Proxy do AD FS 2.0 Federation Server](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar os AD FS agentes Web 1.1](migrate-the-ad-fs-web-agent.md)