---
title: Migrar o proxy do servidor do AD FS 2.0 federation
description: Fornece informações sobre como migrar o proxy do servidor de Federação do AD FS para o Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 98e28c9be808f63ed39a3ac24dd95014b388d001
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881017"
---
# <a name="migrate-the-ad-fs-20-federation-server-proxy"></a>Migrar o proxy do servidor do AD FS 2.0 federation
Este documento fornece informações detalhadas sobre como migrar um servidor de proxy do AD FS 2.0 federation para o Windows Server 2012.

## <a name="migrate-the-proxy"></a>Migrar o proxy

Para migrar um proxy do AD FS 2.0 federation server to Windows Server 2012, execute o procedimento a seguir:  
  
1.  Para cada proxy do servidor de federação que você planeja migrar para o Windows Server 2012, analise e realize os procedimentos [Prepare to Migrate the AD FS 2.0 Federation Server Proxy](prepare-to-migrate-ad-fs-fed-proxy.md).  
  
2.  Remova um proxy do servidor de federação do balanceador de carga.  
  
3.  Realize uma atualização in-loco do sistema operacional desse servidor do Windows Server 2008 R2 ou Windows Server 2008 para o Windows Server 2012. Para obter mais informações, consulte [Installing Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  Como resultado da atualização do sistema operacional, a configuração de proxy do AD FS neste servidor é perdida e a função do servidor do AD FS 2.0 é removida. A função de servidor AD FS do Windows Server 2012 está instalada em vez disso, mas ele não está configurado. Você deverá criar manualmente a configuração original de proxy do AD FS e restaurar as configurações de proxy restantes do AD FS para concluir a migração do proxy do servidor de federação.  
  
4.  Você pode criar a configuração original de proxy do AD FS usando o **Assistente de configuração de proxy do servidor de federação do AD FS**. Para obter mais informações, consulte [Configure a Computer for the Federation Server Proxy Role](configure-a-computer-for-the-federation-server-proxy-role.md). Ao executar o assistente, use as informações coletadas em Preparar para migrar o proxy do servidor de federação do AD FS 2.0, da seguinte maneira:  
  
 
|**Opção de entrada do Assistente de Proxy do servidor de Federação**|**Use o seguinte valor**|
|-----|-----|  
|**Nome do serviço de Federação**|Insira o valor BaseHostName do arquivo proxyproperties.txt|  
|Caixa de diálogo do serviço **Usar um servidor proxy HTTP ao enviar solicitações para essa caixa de seleção do Serviço de Federação**|Marque essa caixa de seleção se o arquivo proxyproperties.txt contiver um valor para a propriedade ForwardProxyUrl|  
|**Endereço do servidor proxy HTTP**|Insira o valor ForwardProxyUrl do arquivo proxyproperties.txt|  
|Solicitação de credenciais|Insira as credenciais de uma conta que seja um administrador do servidor de Federação do AD FS ou a conta de serviço na qual o serviço de federação do AD FS é executado.|  
  
5.  Atualize suas páginas da web do AD FS neste servidor. Se você fez backup suas páginas personalizadas do proxy do AD FS ao preparar seu proxy do servidor de federação para a migração, use os dados de backup para substituir a páginas da Web do AD FS que foram criadas por padrão no padrão de **%systemdrive%\inetpub\adfs\ls** diretório de configuração de proxy do AD FS no Windows Server 2012.  
  
6.  Adicione este servidor volta ao balanceador de carga.  
  
7.  Se você tiver outros proxies do servidor de federação do AD FS 2.0 para migrar, repita as etapas 2 a 6 para os computadores de proxy do servidor de federação restantes.  
  
  
## <a name="next-steps"></a>Próximas etapas
 [Preparar para migrar o servidor do AD FS 2.0 Federation](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparar para migrar o Proxy do AD FS 2.0 Federation Server](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar o servidor do AD FS 2.0 Federation](migrate-the-ad-fs-fed-server.md)   
 [Migrar o Proxy do AD FS 2.0 Federation Server](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar os AD FS agentes Web 1.1](migrate-the-ad-fs-web-agent.md)