---
title: "Migrar o proxy do servidor de Federação 2.0 AD FS"
description: "Fornece informações sobre como migrar o proxy de servidor de Federação do AD FS para o Windows Server 2012."
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 98e28c9be808f63ed39a3ac24dd95014b388d001
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="migrate-the-ad-fs-20-federation-server-proxy"></a>Migrar o proxy do servidor de Federação 2.0 AD FS
Este documento fornece informações detalhadas sobre como migrar um servidor de proxy de Federação 2.0 do AD FS para o Windows Server 2012.

## <a name="migrate-the-proxy"></a>Migrar do proxy

Para migrar um proxy do servidor de Federação 2.0 AD FS para o Windows Server 2012, execute o seguinte procedimento:  
  
1.  Para cada proxy de servidor de federação que você pretende migrar para o Windows Server 2012, revise e execute os procedimentos [preparar para migrar o AD FS 2.0 Proxy do servidor de Federação](prepare-to-migrate-ad-fs-fed-proxy.md).  
  
2.  Remova um proxy de servidor de Federação balanceador de carga.  
  
3.  Realizar uma atualização in-loco do sistema operacional neste servidor do Windows Server 2008 R2 ou Windows Server 2008 para o Windows Server 2012. Para obter mais informações, consulte [instalando o Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).  
  
> [!IMPORTANT]
>  Como resultado da atualização do sistema operacional, a configuração de proxy do AD FS nesse servidor é perdida e a função de servidor 2.0 AD FS é removida. A função de servidor Windows Server 2012 AD FS é instalada em vez disso, mas ela não está configurada. Você deve criar a configuração de proxy do AD FS original e restaurar as configurações de proxy do AD FS restantes para concluir a migração de proxy do servidor de Federação manualmente.  
  
4.  Criar a configuração de proxy do AD FS original usando o **Assistente de configuração do AD FS federação servidor Proxy**. Para obter mais informações, consulte [configurar um computador para a função de Proxy do servidor de Federação](configure-a-computer-for-the-federation-server-proxy-role.md). Conforme você executar o assistente, use as informações coletadas na preparação para migrar o AD FS 2.0 Proxy do servidor de Federação da seguinte maneira:  
  
 
|**Opção de entrada de Assistente de Proxy do servidor de Federação**|**Use o seguinte valor**|
|-----|-----|  
|**Nome do serviço de Federação**|Insira o valor de BaseHostName do arquivo proxyproperties.txt|  
|**Usar um servidor proxy HTTP ao enviar solicitações para essa federação** caixa de seleção do serviço|Marcar essa caixa se o arquivo proxyproperties.txt contiver um valor para a propriedade ForwardProxyUrl|  
|**Endereço do servidor proxy HTTP**|Insira o valor de ForwardProxyUrl do arquivo proxyproperties.txt|  
|Solicitação de credencial|Insira as credenciais de uma conta que é o administrador do servidor de Federação do AD FS ou a conta de serviço na qual o serviço de Federação do AD FS é executado.|  
  
5.  Atualize suas páginas da Web do AD FS nesse servidor. Se você fez backup de suas páginas da Web personalizadas do AD FS proxy ao preparar seu proxy de servidor de federação para a migração, usar seus dados de backup para substituir o padrão do AD FS páginas da Web que foram criados por padrão no **%systemdrive%\inetpub\adfs\ls** diretório como resultado da configuração de proxy do AD FS no Windows Server 2012.  
  
6.  Adicione esse servidor volta ao balanceador de carga.  
  
7.  Se você tiver outros proxies de servidor de Federação 2.0 AD FS migrar, repita as etapas 2 a 6 para os computadores de proxy de servidor de Federação restantes.  
  
  
## <a name="next-steps"></a>Próximas etapas
 [Preparar para migrar o servidor de Federação 2.0 do AD FS](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparar para migrar o Proxy de servidor de Federação 2.0 do AD FS](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar o servidor de Federação 2.0 do AD FS](migrate-the-ad-fs-fed-server.md)   
 [Migrar o Proxy de servidor de Federação 2.0 do AD FS](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar os agentes do AD FS Web 1.1](migrate-the-ad-fs-web-agent.md)