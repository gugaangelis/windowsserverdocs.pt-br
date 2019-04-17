---
title: "Preparar para migrar o Proxy de servidor de Federação 2.0 do AD FS"
description: "Fornece informações sobre Preparando-se para migrar o proxy do servidor do AD FS para o Windows Server 2012."
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f207993580e6fd06c9ff185e58e5b7e81af60252
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="prepare-to-migrate-the-ad-fs-20-federation-server-proxy"></a>Preparar para migrar o Proxy de servidor de Federação 2.0 do AD FS

Para preparar para migrar um proxy do servidor de Federação 2.0 AD FS para o Windows Server 2012, você deve exportar e fazer backup dos dados de configuração do AD FS desse proxy server.  As etapas neste tópico se aplicam a um cenário com um servidor de federação de proxy ou de vários servidores proxy de Federação.  
  
 Para exportar os dados de configuração do AD FS, execute as seguintes tarefas:  
  
-   [Etapa 1: Exportar configurações de serviço de proxy](#step-1-export-proxy-service-settings)  
  
-   [Etapa 2: Backup personalizações de página da Web](#step-2-back-up-webpage-customizations)  
  
##  <a name="step-1-export-proxy-service-settings"></a>Etapa 1: Exportar configurações de serviço de proxy  
 Para exportar configurações de serviço de proxy de servidor de federação, execute o seguinte procedimento:  
  
### <a name="to-export-proxy-service-settings"></a>Para exportar as configurações de serviço de proxy  
  
1.  Exporte o certificado Secure Sockets Layer (SSL) e sua chave privada para um arquivo. pfx. Para obter mais informações, consulte [exportar a parte de chave privada de um certificado de autenticação de servidor](export-the-private-key-portion-of-a-server-authentication-certificate.md).  
  
> [!NOTE]
>  Esta etapa é opcional, pois esse certificado é preservado durante a atualização do sistema operacional.  
  
2.  Exporte propriedades de proxy de Federação 2.0 do AD FS para um arquivo. Você pode fazer isso usando o Windows PowerShell.  
  
Abra o Windows PowerShell e execute o seguinte comando para adicionar os cmdlets do AD FS a sessão do Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Em seguida, execute o seguinte comando para exportar propriedades de proxy de federação para um arquivo:`PSH:> Get-ADFSProxyProperties | out-file “.\proxyproperties.txt”`.  
  
3.  Certifique-se de que você sabe que as credenciais de uma conta que é o administrador do servidor de Federação do AD FS ou a conta de serviço na qual o serviço de Federação do AD FS é executado.  Essas informações são necessárias para a configuração de proxy de confiança.  
  
 Concluir resultados essa etapa na coleta as informações a seguir que é necessário para configurar seu proxy de servidor de Federação do AD FS:  
  
-   Nome do serviço de Federação do AD FS  
  
-   Nome da conta de domínio é necessária para a configuração de proxy de confiança  
  
-   O endereço e a porta do proxy HTTP (se houver um proxy HTTP entre o proxy de servidor de Federação do AD FS e os servidores de Federação do AD FS)  
  
##  <a name="step-2-back-up-webpage-customizations"></a>Etapa 2: Backup personalizações de página da Web  
 Para fazer backup das personalizações de página da Web, copie as páginas da Web do AD FS proxy e o **Web. config** arquivo da pasta que é mapeada para o caminho virtual **"/ adfs/ls"** no IIS.  Por padrão, ele é o **%systemdrive%\inetpub\adfs\ls** diretório.  
  
## <a name="next-steps"></a>Próximas etapas
 [Preparar para migrar o servidor de Federação 2.0 do AD FS](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparar para migrar o Proxy de servidor de Federação 2.0 do AD FS](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar o servidor de Federação 2.0 do AD FS](migrate-the-ad-fs-fed-server.md)   
 [Migrar o Proxy de servidor de Federação 2.0 do AD FS](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar os agentes do AD FS Web 1.1](migrate-the-ad-fs-web-agent.md)