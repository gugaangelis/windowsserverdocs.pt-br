---
title: Migrar a Função Serviços de Federação do Active Directory para o Windows Server 2012
description: Fornece instruções para migrar o serviço de AD FS para o Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 36d37eb2cc886d9831b995aa8cfdda16765994b8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857509"
---
# <a name="migrate-active-directory-federation-services-role-services-to-windows-server-2012"></a>Migrar a Função Serviços de Federação do Active Directory para o Windows Server 2012

O seguinte fornece instruções sobre como migrar os seguintes serviços de função para Serviços de Federação do Active Directory (AD FS) (AD FS) no Windows Server 2012:  
  
-   AD FS agente baseado em token do Windows 1,1 e AD FS agente com reconhecimento de declaração 1,1 instalado com o Windows Server 2008 ou o Windows Server 2008 R2  
  
-   AD FS servidor de Federação 2,0 e AD FS proxy do servidor de Federação 2,0 instalado no Windows Server 2008 ou no Windows Server 2008 R2    
  
## <a name="supported-migration-scenarios"></a>Cenários de migração com suporte  
 As instruções de migração contêm as seguintes tarefas:  
  
- Exportando os dados de configuração do AD FS 2,0 do servidor que está executando o Windows Server 2008 ou o Windows Server 2008 R2  
  
- Executando uma atualização in-loco do sistema operacional deste servidor do Windows Server 2008 ou do Windows Server 2008 R2 para o Windows Server 2012.
  
- Recriar a configuração de AD FS original e restaurar as configurações de serviço AD FS restantes nesse servidor, que agora está executando a função de servidor AD FS que é instalada com o Windows Server 2012.  
  
  Este guia não inclui instruções para migrar um servidor que está executando várias funções. Se o servidor executar várias funções, é recomendável criar um processo de migração personalizado específico para o ambiente do servidor, com base nas informações fornecidas em outros guias de migração de função. Guias de migração de funções adicionais estão disponíveis no [Portal de Migração do Windows Server](https://go.microsoft.com/fwlink/?LinkId=247608).  
  
## <a name="supported-operating-systems"></a>Sistemas operacionais compatíveis  
 **Sistema operacional do servidor de destino:**  
  

- Windows Server 2012 ou Windows Server 2008 R2 (Server Core e opções de instalação completa)  
  
  **Processador do servidor de destino:**  
  

- Com base em x64  
  
|Processador do servidor de origem|Sistema operacional do servidor de origem|  
|-----|-----|  
|Com base em x86 ou x64|Windows Server 2003 com Service Pack 2|  
|Com base em x86 ou x64|Windows Server 2003 R2|  
|Com base em x86 ou x64|Windows Server 2008, opções de instalação completa e Server Core|  
|Com base em x64|Windows Server 2008 R2|  
|Com base em x64|Opção de instalação Server Core do Windows Server 2008 R2|  
|Com base em x64|Server Core e opções de instalação completa do Windows Server 2012|  
  
> [!NOTE]
> - As versões dos sistemas operacionais mostradas na tabela anterior são as combinações mais antigas de sistemas operacionais e service packs com suporte.  
>   -   As edições Foundation, Standard, Enterprise e Datacenter do sistema operacional Windows Server têm suporte como a origem ou o servidor de destino.  
>   -   Também há suporte para migrações entre sistemas operacionais físicos e virtuais.  
  
### <a name="supported-ad-fs-role-services-and-features"></a>Recursos e serviços de função AD FS com suporte  
 A tabela a seguir descreve os cenários de migração do AD FS serviços de função e suas respectivas configurações descritas neste guia.  
  
|De|Para AD FS instalado com o Windows Server 2012|  
|----------|-----|  
|AD FS servidor de Federação 1,0 instalado com o Windows Server 2003 R2|Não há suporte para migração|  
|AD FS o proxy do servidor de Federação 1,0 instalado com o Windows Server 2003 R2|Não há suporte para migração|  
|AD FS agente baseado em token do Windows 1,0 instalado com o Windows Server 2003 R2|Não há suporte para migração|  
|AD FS agente de reconhecimento de declaração do 1,0 instalado com o Windows Server 2003 R2)|Não há suporte para migração|  
|AD FS servidor de Federação 1,1 instalado com o Windows Server 2008 ou Windows Server 2008 R2|Não há suporte para migração|  
|AD FS o proxy do servidor de Federação 1,1 instalado com o Windows Server 2008 ou o Windows Server 2008 R2|Não há suporte para migração|  
|AD FS agente baseado em token do Windows 1,1 instalado com o Windows Server 2008 ou Windows Server 2008 R2|Há suporte para a migração no mesmo servidor, mas o AD FS migrado do agente baseado em token do Windows funcionará apenas com um serviço de Federação do AD FS 1,1 instalado com o Windows Server 2008 ou o Windows Server 2008 R2. Para obter mais informações, consulte:<p> [Migrar os Agentes Web do AD FS 1.1](migrate-the-ad-fs-web-agent.md)<p> [Como interoperar com o AD FS 1.x](Interoperating-with-AD-FS-1.x.md)|  
|AD FS agente de reconhecimento de declaração do 1,1 instalado com o Windows Server 2008 ou Windows Server 2008 R2)|Há suporte para migração no mesmo servidor. O agente da Web com reconhecimento de declarações 1,1 migrado AD FS funcionará com o seguinte:<p> AD FS serviço de Federação 1,1 instalado com o Windows Server 2008 ou Windows Server 2008 R2<p> AD FS serviço de Federação 2,0 instalado no Windows Server 2008 ou no Windows Server 2008 R2<p> AD FS serviço de Federação instalado com o Windows Server 2012<p> Para obter mais informações, consulte:<p> [Migrar os Agentes Web do AD FS 1.1](migrate-the-ad-fs-web-agent.md)<p> [Como interoperar com o AD FS 1.x](Interoperating-with-AD-FS-1.x.md)|  
|AD FS servidor de Federação 2,0 instalado no Windows Server 2008 ou no Windows Server 2008 R2|Há suporte para migração no mesmo servidor. Para obter mais informações, consulte:<p> [Preparar a migração do Servidor de Federação do AD FS 2.0](prepare-to-migrate-ad-fs-fed-server.md)<p> [Migrar o Servidor de Federação do AD FS 2.0](migrate-the-ad-fs-fed-server.md)|  
|AD FS o proxy do servidor de Federação 2,0 instalado no Windows Server 2008 ou no Windows Server 2008 R2|Há suporte para migração no mesmo servidor.  Para obter mais informações, consulte:<p> [Preparar a migração do Proxy do Servidor de Federação do AD FS 2.0](prepare-to-migrate-ad-fs-fed-proxy.md)<p> [Migrar o Proxy do Servidor de Federação do AD FS 2.0](migrate-the-ad-fs-2-fed-server-proxy.md)|  
  
## <a name="see-also"></a>Consulte também  
 [Prepare-se para migrar o servidor de federação AD FS 2,0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparar para migrar o proxy do servidor de federação AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar o servidor de federação AD FS 2,0](migrate-the-ad-fs-fed-server.md)   
 [Migrar o proxy do servidor de federação AD FS 2,0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar os Agentes Web do AD FS 1.1](migrate-the-ad-fs-web-agent.md)