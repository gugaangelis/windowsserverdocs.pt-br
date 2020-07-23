---
title: Migrar o proxy do servidor de Federação AD FS 2,0
description: Fornece informações sobre como migrar o proxy de servidor de Federação AD FS para o Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d7b08b28f33bbab5d3e573b10195528c47f6cc6c
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86966228"
---
# <a name="migrate-the-ad-fs-20-federation-server-proxy"></a>Migrar o proxy do servidor de Federação AD FS 2,0
Este documento fornece informações detalhadas sobre como migrar um servidor proxy de Federação AD FS 2,0 para o Windows Server 2012.

## <a name="migrate-the-proxy"></a>Migrar o proxy

Para migrar um proxy de servidor de Federação AD FS 2,0 para o Windows Server 2012, execute o seguinte procedimento:  
  
1.  Para cada proxy de servidor de Federação que você planeja migrar para o Windows Server 2012, examine e execute os procedimentos em [preparar para migrar o proxy de servidor de federação AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md).  
  
2.  Remova um proxy do servidor de federação do balanceador de carga.  
  
3.  Execute uma atualização in-loco do sistema operacional neste servidor do Windows Server 2008 R2 ou do Windows Server 2008 para o Windows Server 2012. Para obter mais informações, consulte [Instalando o Windows Server 2012](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj134246(v=ws.11)).  
  
> [!IMPORTANT]
>  Como resultado da atualização do sistema operacional, a configuração de proxy do AD FS neste servidor é perdida e a função do servidor do AD FS 2.0 é removida. A função de servidor do Windows Server 2012 AD FS está instalada, mas não está configurada. Você deverá criar manualmente a configuração original de proxy do AD FS e restaurar as configurações de proxy restantes do AD FS para concluir a migração do proxy do servidor de federação.  
  
4. Você pode criar a configuração original de proxy do AD FS usando o **Assistente de configuração de proxy do servidor de federação do AD FS**. Para obter mais informações, consulte [Configure a Computer for the Federation Server Proxy Role](configure-a-computer-for-the-federation-server-proxy-role.md). Ao executar o assistente, use as informações coletadas em Preparar para migrar o proxy do servidor de federação do AD FS 2.0, da seguinte maneira:  
  
 
|**Opção de entrada do Assistente de Proxy do Servidor de Federação**|**Use o seguinte valor**|
|-----|-----|  
|**Nome do Serviço de Federação**|Insira o valor BaseHostName do arquivo proxyproperties.txt|  
|Caixa de diálogo do serviço **Usar um servidor proxy HTTP ao enviar solicitações para essa caixa de seleção do Serviço de Federação**|Marque essa caixa de seleção se o arquivo proxyproperties.txt contiver um valor para a propriedade ForwardProxyUrl|  
|**Endereço do servidor proxy HTTP**|Insira o valor ForwardProxyUrl do arquivo proxyproperties.txt|  
|Solicitação de credenciais|Insira as credenciais de uma conta que seja um administrador do servidor de Federação do AD FS ou a conta de serviço na qual o serviço de federação do AD FS é executado.|  
  
5. Atualize suas páginas da web do AD FS neste servidor. Se você tiver feito backup de suas páginas da Web de proxy de AD FS personalizadas ao preparar o proxy do servidor de Federação para a migração, use os dados de backup para substituir as páginas da Web do AD FS padrão que foram criadas por padrão no diretório **%systemdrive%\inetpub\adfs\ls** como resultado da configuração de proxy de AD FS no Windows Server 2012.  
  
6. Adicione este servidor volta ao balanceador de carga.  
  
7. Se você tiver outros proxies do servidor de federação do AD FS 2.0 para migrar, repita as etapas 2 a 6 para os computadores de proxy do servidor de federação restantes.  
  
  
## <a name="next-steps"></a>Próximas etapas
 [Preparar para migrar o servidor de Federação AD FS 2,0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparar para migrar o proxy do servidor de Federação AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar o servidor de Federação AD FS 2,0](migrate-the-ad-fs-fed-server.md)   
 [Migrar o proxy do servidor de Federação AD FS 2,0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar os Agentes Web do AD FS 1.1](migrate-the-ad-fs-web-agent.md)
