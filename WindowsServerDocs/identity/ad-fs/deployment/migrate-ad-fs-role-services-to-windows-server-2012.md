---
title: Migrar a Função Serviços de Federação do Active Directory para o Windows Server 2012
description: Fornece instruções para migrar o serviço de AD FS para o Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.openlocfilehash: abbd51daea374eb4684f5416bed45fb0aaf315d4
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87938206"
---
# <a name="migrate-active-directory-federation-services-role-services-to-windows-server-2012"></a>Migrar a Função Serviços de Federação do Active Directory para o Windows Server 2012

O seguinte fornece instruções sobre como migrar os seguintes serviços de função para Serviços de Federação do Active Directory (AD FS) (AD FS) no Windows Server 2012:

-   Agente AD FS 1.1 do Windows baseado em token e agente AD FS 1.1 compatível com declarações instalados com o Windows Server 2008 ou Windows Server 2008 R2

-   do servidor de federação do AD FS 2.0 e proxy do servidor de federação do AD FS 2.0 instalado no Windows Server 2008 ou no Windows Server 2008 R2

## <a name="supported-migration-scenarios"></a>Cenários de migração com suporte
 As instruções de migração contêm as seguintes tarefas:

- Exportar dados de configuração do AD FS 2.0 do servidor que está executando o Windows Server 2008 ou Windows Server 2008 R2

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
|Com base em x64|Opção de instalação Server Core do Windows Server 2008 R2|
|Com base em x64|Server Core e opções de instalação completa do Windows Server 2012|

> [!NOTE]
> - As versões dos sistemas operacionais mostradas na tabela anterior são as combinações mais antigas de sistemas operacionais e service packs com suporte.
>   -   As edições Foundation, Standard, Enterprise e Datacenter do sistema operacional Windows Server têm suporte como servidor de origem ou de destino.
>   -   Também há suporte para migrações entre sistemas operacionais físicos e virtuais.

### <a name="supported-ad-fs-role-services-and-features"></a>Recursos e serviços de função do AD FS suportados
 A tabela a seguir descreve os cenários de migração dos serviços de função do AD FS e suas respectivas configurações descritas neste guia.

|De|Para AD FS instalado com o Windows Server 2012|
|----------|-----|
|Servidor de federação do AD FS 1.0 instalado com o Windows Server 2003 R2|Não há suporte para migração|
|Proxy do servidor de federação do AD FS 1.0 instalado com o Windows Server 2003 R2|Não há suporte para migração|
|Agente do Windows baseado em token AD FS 1.0 instalado com o Windows Server 2003 R2|Não há suporte para migração|
|Agente de reconhecimento de declarações AD FS 1.0 instalado com o Windows Server 2003 R2)|Não há suporte para migração|
|Servidor de federação do AD FS 1.1 instalado no Windows Server 2008 ou no Windows Server 2008 R2|Não há suporte para migração|
|Proxy do servidor de federação do AD FS 1.1 instalado com o Windows Server 2008 ou Windows Server 2008 R2|Não há suporte para migração|
|Agente do Windows baseado em token do AD FS 1.1 instalado no Windows Server 2008 ou no Windows Server 2008 R2|Há suporte para a migração no mesmo servidor, mas o agente do Windows baseado em token do AD FS funcionará apenas com um serviço de federação do AD FS 1.1 instalado com o Windows Server 2008 ou Windows Server 2008 R2. Para obter mais informações, consulte:<p> [Migrar os Agentes Web do AD FS 1.1](migrate-the-ad-fs-web-agent.md)<p> [Como interoperar com o AD FS 1.x](Interoperating-with-AD-FS-1.x.md)|
|Agente de reconhecimento de declarações AD FS 1.1 instalado com o Windows Server 2008 ou Windows Server 2008 R2.|Há suporte para migração no mesmo servidor. O agente Web de reconhecimento de declarações AD FS 1.1 migrado funcionará com o seguinte:<p> Serviço de federação do AD FS 1.1 instalado com Windows Server 2008 ou Windows Server 2008 R2<p> Serviço de federação do AD FS 2.0 instalado no Windows Server 2008 ou Windows Server 2008 R2<p> AD FS serviço de Federação instalado com o Windows Server 2012<p> Para obter mais informações, consulte:<p> [Migrar os Agentes Web do AD FS 1.1](migrate-the-ad-fs-web-agent.md)<p> [Como interoperar com o AD FS 1.x](Interoperating-with-AD-FS-1.x.md)|
|Servidor de federação do AD FS 2.0 instalado no Windows Server 2008 ou no Windows Server 2008 R2|Há suporte para migração no mesmo servidor. Para obter mais informações, consulte:<p> [Preparar a migração do Servidor de Federação do AD FS 2.0](prepare-to-migrate-ad-fs-fed-server.md)<p> [Migrar o Servidor de Federação do AD FS 2.0](migrate-the-ad-fs-fed-server.md)|
|Proxy do servidor de federação do AD FS 2.0 instalado no Windows Server 2008 ou Windows Server 2008 R2|Há suporte para migração no mesmo servidor.  Para obter mais informações, consulte:<p> [Preparar a migração do Proxy do Servidor de Federação do AD FS 2.0](prepare-to-migrate-ad-fs-fed-proxy.md)<p> [Migrar o Proxy do Servidor de Federação do AD FS 2.0](migrate-the-ad-fs-2-fed-server-proxy.md)|

## <a name="see-also"></a>Consulte Também
 [Preparar para migrar o servidor de federação AD FS 2,0](prepare-to-migrate-ad-fs-fed-server.md) [preparar para migrar o proxy do servidor de Federação AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md) [migrar o AD FS servidor de Federação do 2,0](migrate-the-ad-fs-fed-server.md) [migrar o proxy do servidor de Federação do AD FS 2,0](migrate-the-ad-fs-2-fed-server-proxy.md) migrar os [agentes Web do AD FS 1,1](migrate-the-ad-fs-web-agent.md)