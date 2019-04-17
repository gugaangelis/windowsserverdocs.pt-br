---
title: Preparar para migrar um farm AD FS SQL
description: "Fornece informações sobre Preparando-se para migrar um farm SQL server AD FS para o Windows Server 2012."
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 284e02174b4a8c06f114640223d289dc63ea3a26
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="prepare-to-migrate-a-sql-server-farm"></a>Preparar para migrar um farm de servidores do SQL  
 Para preparar para migrar os servidores de Federação 2.0 do AD FS que pertencem a um farm de servidores do SQL para o Windows Server 2012, você deve exportar e fazer backup dos dados de configuração do AD FS desses servidores.  
  
 Para exportar os dados de configuração do AD FS, execute as seguintes tarefas:  
  
-   [Etapa 1: Exportar configurações de serviço](#step-1-export-service-settings)  
  
-   [Etapa 2: Backup repositórios de atributo personalizado](#step-2-back-up-custom-attribute-stores)  
  
-   [Etapa 3: Fazer backup de personalizações de página da Web](#step-3-back-up-webpage-customizations)  
  
## <a name="step-1-export-service-settings"></a>Etapa 1: Exportar configurações de serviço  
 Para exportar configurações de serviço, execute o seguinte procedimento:  
  
### <a name="to-export-service-settings"></a>Para exportar configurações de serviço  
  
1.  Registre o certificado assunto valor de nome e a impressão digital do certificado SSL usado pelo serviço de Federação. Para encontrar o certificado SSL, abra o console de gerenciamento de serviços de informações da Internet (IIS), selecione **site padrão** no painel esquerdo, clique em **associações...** No **ação** painel, encontre e selecione a ligação https, clique em **editar**e clique em **exibição**.  
  
> [!NOTE]
>  Opcionalmente, você também pode exportar o SSL) certificado e sua chave privada em um arquivo. pfx. Para obter mais informações, consulte [exportar a parte de chave privada de um certificado de autenticação de servidor ](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md).  
>   
>  Esta etapa é opcional, pois esse certificado é armazenado no repositório de certificados pessoal do computador local e será preservado na atualização do sistema operacional.  
  
2.  Outros certificados de assinatura de token, criptografia de token ou comunicações de serviço e chaves que não são geradas internamente pelo AD FS de exportação.  
  
Você pode exibir todos os certificados que estão sendo usados pelo AD FS no servidor usando o Windows PowerShell. Abra o Windows PowerShell e execute o seguinte comando para adicionar os cmdlets do AD FS a sessão do Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Em seguida, execute o seguinte comando para exibir todos os certificados que estão em uso no seu servidor `PSH:>Get-ADFSCertificate`. A saída desse comando inclui valores StoreLocation e StoreName que especificam o local do repositório de cada certificado.  
  
> [!NOTE]
>  Opcionalmente, você pode usar as orientações no [exportar a parte de chave privada de um certificado de autenticação de servidor](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md) para exportar cada certificado e sua chave privada para um arquivo. pfx. Esta etapa é opcional, pois todos os certificados externos são preservados durante a atualização do sistema operacional.  
  
3.  Faça backup do arquivo de configuração do aplicativo. Entre outras configurações, esse arquivo contém a cadeia de caracteres de conexão de banco de dados de política.  
  
Para fazer backup do arquivo de configuração do aplicativo, você deve copiar manualmente os `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`arquivo para um local seguro no servidor de backup.  
  
> [!NOTE]
>  Gravar cadeia de caracteres de conexão da SQL Server após "policystore connectionstring =" no arquivo a seguir:`%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`. Você precisará essa cadeia de caracteres quando você restaurar a configuração original do AD FS no servidor de Federação.  
  
4.  Registre a identidade da conta de serviço de Federação 2.0 do AD FS e a senha dessa conta.  
  
Para localizar o valor de identidade, examine o **fazer logon como** coluna de **AD FS 2.0 Windows serviço** no **serviços** do console e registrar manualmente o valor.  
  
## <a name="step-2-back-up-custom-attribute-stores"></a>Etapa 2: Backup repositórios de atributo personalizado  
 Você pode encontrar informações sobre o atributo personalizado lojas em uso pelo AD FS usando o Windows PowerShell. Abra o Windows PowerShell e execute o seguinte comando para adicionar os cmdlets do AD FS a sessão do Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Em seguida, execute o seguinte comando para encontrar informações sobre os repositórios do atributo personalizado:`PSH:>Get-ADFSAttributeStore`. As etapas para atualizar ou migrar o atributo personalizado lojas variam.  
  
## <a name="step-3-back-up-webpage-customizations"></a>Etapa 3: Fazer backup de personalizações de página da Web  
 Para fazer backup de todas as personalizações página da Web, copie as páginas da Web do AD FS e o **Web. config** arquivo da pasta que é mapeada para o caminho virtual **"/ adfs/ls"** no IIS. Por padrão, ele é o **%systemdrive%\inetpub\adfs\ls** diretório.  
  
## <a name="next-steps"></a>Próximas etapas
 [Preparar para migrar o servidor de Federação 2.0 do AD FS](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparar para migrar o Proxy de servidor de Federação 2.0 do AD FS](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar o servidor de Federação 2.0 do AD FS](migrate-the-ad-fs-fed-server.md)   
 [Migrar o Proxy de servidor de Federação 2.0 do AD FS](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar os agentes do AD FS Web 1.1](migrate-the-ad-fs-web-agent.md)