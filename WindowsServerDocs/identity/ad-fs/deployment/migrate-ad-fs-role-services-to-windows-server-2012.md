---
title: "Migrar Serviços de função de serviços de Federação do Active Directory para Windows Server 2012"
description: "Fornece instruções para migrar o serviço do AD FS para o Windows Server 2012."
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2b44ed504c2b3dc8a8ac0ca9648f1db9e362e075
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="migrate-active-directory-federation-services-role-services-to-windows-server-2012"></a>Migrar Serviços de função de serviços de Federação do Active Directory para Windows Server 2012

A seguir fornece instruções sobre como migrar os seguintes serviços de função para serviços de Federação do Active Directory (AD FS) no Windows Server 2012:  
  
-   Agente baseado em token do AD FS 1.1 Windows e o AD FS 1.1 agente compatível com declarações instalado com o Windows Server 2008 ou Windows Server 2008 R2  
  
-   Servidor de Federação do AD FS 2.0 e o proxy do servidor de Federação 2.0 AD FS instalada no Windows Server 2008 ou Windows Server 2008 R2    
  
## <a name="supported-migration-scenarios"></a>Cenários de migração com suporte  
 As instruções de migração contém as seguintes tarefas:  
  
-   Exportando os dados de configuração 2.0 AD FS do seu servidor que está executando o Windows Server 2008 ou Windows Server 2008 R2  
  
-   Executando uma atualização in-loco do sistema operacional do servidor do Windows Server 2008 ou Windows Server 2008 R2 para o Windows Server 2012.
  
-   Recriar a configuração original do AD FS e restaurando o restante do AD FS serviço configurações nesse servidor, que agora está executando a função de servidor do AD FS que é instalada com o Windows Server 2012.  
  
 Este guia não inclui instruções para migrar um servidor que executa várias funções. Se o servidor está executando várias funções, recomendamos que você projete um processo de migração personalizados específico para o seu ambiente de servidor, com base nas informações fornecidas nas outros guias de migração de função. Guias de migração para funções adicionais estão disponíveis no [Portal de migração do Windows Server](https://go.microsoft.com/fwlink/?LinkId=247608).  
  
## <a name="supported-operating-systems"></a>Sistemas operacionais com suporte  
 **Sistema de operacional de servidor de destino:**  
  

-  Windows Server 2012 ou Windows Server 2008 R2 (Server Core e opções de instalação completa)  
  
 **Processador de servidor de destino:**  
  

-  Com base em x64  
  
|Processador de servidor de origem|Sistema de operacional de servidor de origem|  
|-----|-----|  
|x86-ou x64-com base em|Windows Server 2003 com Service Pack 2|  
|x86-ou x64-com base em|Windows Server 2003 R2|  
|x86-ou x64-com base em|Windows Server 2008, completo e opções de instalação Server Core|  
|Com base em x64|Windows Server 2008 R2|  
|Com base em x64|Opção de instalação Server Core do Windows Server 2008 R2|  
|Com base em x64|Opções de instalação completa do Windows Server 2012 e núcleo do servidor|  
  
> [!NOTE]
>  -   As versões dos sistemas operacionais que estão listados na tabela anterior são mais antigas combinações de sistemas operacionais e service packs com suporte.  
> -   As edições Foundation, Standard, Enterprise e Datacenter do sistema operacional Windows Server são aceitas como a origem ou o servidor de destino.  
> -   Migrações entre sistemas operacionais físicos e virtuais sistemas operacionais são compatíveis.  
  
### <a name="supported-ad-fs-role-services-and-features"></a>Recursos e serviços de função do AD FS com suporte  
 A tabela a seguir descreve os cenários de migração de serviços de função do AD FS e as respectivas configurações descritas neste guia.  
  
|De|Para o AD FS instalada com o Windows Server 2012|  
|----------|-----|  
|Servidor de Federação do AD FS 1.0 instalado com o Windows Server 2003 R2|Não há suporte para migração|  
|Proxy de servidor de Federação AD FS 1.0 instalado com o Windows Server 2003 R2|Não há suporte para migração|  
|AD FS 1.0 baseado em token agente do Windows instalado com o Windows Server 2003 R2|Não há suporte para migração|  
|Agente de reconhecimento de declarações de 1.0 FS AD instalado com o Windows Server 2003 R2)|Não há suporte para migração|  
|Servidor de Federação do AD FS 1.1 instalado com o Windows Server 2008 ou Windows Server 2008 R2|Não há suporte para migração|  
|Proxy de servidor de Federação AD FS 1.1 instalado com o Windows Server 2008 ou Windows Server 2008 R2|Não há suporte para migração|  
|AD FS 1.1 baseado em token agente do Windows instalado com o Windows Server 2008 ou Windows Server 2008 R2|Migração no mesmo servidor é suportada, mas o agente baseado em token migrado do AD FS Windows funcionarão somente com um serviço de Federação 1.1 AD FS instalado com o Windows Server 2008 ou Windows Server 2008 R2. Para obter mais informações, consulte:<br /><br /> [Migrar os agentes do AD FS Web 1.1](migrate-the-ad-fs-web-agent.md)<br /><br /> [Interoperando com o AD FS 1. x](Interoperating-with-AD-FS-1.x.md)|  
|AD FS 1.1 reconhecimento de declarações agente do instalado com o Windows Server 2008 ou Windows Server 2008 R2)|Há suporte para migração no mesmo servidor. O agente de web com reconhecimento de declarações 1.1 migrado do AD FS funcionarão com o seguinte:<br /><br /> Serviço de Federação do AD FS 1.1 instalado com o Windows Server 2008 ou Windows Server 2008 R2<br /><br /> Serviço de Federação 2.0 do AD FS instalado no Windows Server 2008 ou Windows Server 2008 R2<br /><br /> Serviço de Federação do AD FS instalada com o Windows Server 2012<br /><br /> Para obter mais informações, consulte:<br /><br /> [Migrar os agentes do AD FS Web 1.1](migrate-the-ad-fs-web-agent.md)<br /><br /> [Interoperando com o AD FS 1. x](Interoperating-with-AD-FS-1.x.md)|  
|Servidor de Federação do AD FS 2.0 instalado no Windows Server 2008 ou Windows Server 2008 R2|Há suporte para migração no mesmo servidor. Para obter mais informações, consulte:<br /><br /> [Preparar para migrar o servidor de Federação 2.0 do AD FS](prepare-to-migrate-ad-fs-fed-server.md)<br /><br /> [Migrar o servidor de Federação 2.0 do AD FS](migrate-the-ad-fs-fed-server.md)|  
|Proxy de servidor de Federação AD FS 2.0 instalado no Windows Server 2008 ou Windows Server 2008 R2|Há suporte para migração no mesmo servidor.  Para obter mais informações, consulte:<br /><br /> [Preparar para migrar o Proxy de servidor de Federação 2.0 do AD FS](prepare-to-migrate-ad-fs-fed-proxy.md)<br /><br /> [Migrar o Proxy de servidor de Federação 2.0 do AD FS](migrate-the-ad-fs-2-fed-server-proxy.md)|  
  
## <a name="see-also"></a>Consulte também  
 [Preparar para migrar o servidor de Federação 2.0 do AD FS](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparar para migrar o Proxy de servidor de Federação 2.0 do AD FS](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar o servidor de Federação 2.0 do AD FS](migrate-the-ad-fs-fed-server.md)   
 [Migrar o Proxy de servidor de Federação 2.0 do AD FS](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar os agentes do AD FS Web 1.1](migrate-the-ad-fs-web-agent.md)