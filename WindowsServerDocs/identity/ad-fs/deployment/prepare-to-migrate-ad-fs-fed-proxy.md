---
title: Preparar para migrar o proxy do servidor de federação do AD FS 2.0
description: Fornece informações sobre como preparar-se para migrar o proxy do AD FS Server para o Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 20fbf3ea9231706635df2bd4c1d541fde0c1484b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408209"
---
# <a name="prepare-to-migrate-the-ad-fs-20-federation-server-proxy"></a>Preparar para migrar o proxy do servidor de federação do AD FS 2.0

Para se preparar para migrar um proxy de servidor de Federação AD FS 2,0 para o Windows Server 2012, você deve exportar e fazer backup dos dados de configuração de AD FS deste proxy de servidor.  As etapas neste tópico se aplicam a um cenário com um servidor de federação de proxy ou vários servidores de federação de proxy.  
  
 Para exportar os dados de configuração do AD FS, realize as seguintes tarefas:  
  
-   [Etapa 1: Exportar configurações do serviço de proxy @ no__t-0  
  
-   [Etapa 2: Fazer backup de personalizações de página da Web @ no__t-0  
  
##  <a name="step-1-export-proxy-service-settings"></a>Etapa 1: Configurações de serviço de proxy de exportação  
 Para exportar configurações de serviço de proxy do servidor de federação, realize o seguinte procedimento:  
  
### <a name="to-export-proxy-service-settings"></a>Para exportar as configurações de serviço de proxy  
  
1.  Exporte o certificado do protocolo SSL e sua chave privada para um arquivo .pfx. Para obter mais informações, consulte [Exportar a parte da chave privada de um Certificado de Autenticação de Servidor](export-the-private-key-portion-of-a-server-authentication-certificate.md).  
  
> [!NOTE]
>  Esta etapa é opcional porque esse certificado é preservado durante a atualização do sistema operacional.  
  
2. Exporte as propriedades de proxy de federação do AD FS 2.0 para um arquivo. É possível fazer isso usando o Windows PowerShell.  
  
Abra o Windows PowerShell e execute o seguinte comando para adicionar os cmdlets do AD FS à sessão do Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Em seguida, execute o seguinte comando para exportar as propriedades do proxy de federação para um arquivo: `PSH:> Get-ADFSProxyProperties | out-file “.\proxyproperties.txt”`.  
  
3. Certifique-se de que você saiba as credenciais de uma conta que é um administrador do servidor de federação do AD FS ou a conta de serviço sob a qual o serviço de federação do AD FS é executado.  Essas informações são necessárias para a configuração de confiança do proxy.  
  
   Concluir essa etapa resulta em reunir as informações a seguir que são necessárias para configurar seu proxy do servidor de federação do AD FS:  
  
-   Nome do serviço de federação do AD FS  
  
-   Nome da conta de domínio que é necessária para a configuração de confiança de proxy  
  
-   O endereço e a porta do proxy HTTP (se houver um proxy HTTP entre o proxy do servidor de federação do AD FS e os servidores de federação do AD FS)  
  
##  <a name="step-2-back-up-webpage-customizations"></a>Etapa 2: Fazer backup de personalizações de página da Web  
 Para fazer backup de personalizações da página da Web, copie as páginas da Web do proxy do AD FS e o arquivo **web.config** do diretório mapeado para o caminho virtual **“/adfs/ls”** no IIS.  Por padrão, ele fica no diretório **%systemdrive%\inetpub\adfs\ls**.  
  
## <a name="next-steps"></a>Próximas etapas
 [Preparar para migrar o servidor de federação AD FS 2,0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparar para migrar o proxy do servidor de federação AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar o servidor de federação AD FS 2,0](migrate-the-ad-fs-fed-server.md)   
 [Migrar o proxy do servidor de federação AD FS 2,0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar os Agentes Web do AD FS 1.1](migrate-the-ad-fs-web-agent.md)