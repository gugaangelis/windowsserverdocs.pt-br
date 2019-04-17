---
title: "Preparar para migrar um servidor de Federação do AD FS autônomo"
description: "Fornece informações sobre Preparando-se para migrar um servidor independente do AD FS para o Windows Server 2012."
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0c1fd2bc1026d9aee25c591cf5c91a1c59f66ee0
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
#  <a name="prepare-to-migrate-a-stand-alone-ad-fs-federation-server-or-a-single-node-ad-fs-farm"></a>Prepare-se para migrar um servidor de Federação do AD FS autônomo ou um farm de AD FS do nó único  
 
Para se preparar para migrar (mesmo servidor migração) um servidor de Federação 2.0 AD FS autônomo ou um farm de AD FS do nó único para o Windows Server 2012, você deve exportar e fazer backup dos dados de configuração do AD FS desse servidor.  
  
Para exportar os dados de configuração do AD FS, execute as seguintes tarefas:  
  
-   [Etapa 1: Exportar configurações de serviço](#step-1-export-service-settings)  
  
-   [Etapa 2: Exportar declarações relações de confiança do provedor](#step-2-export-claims-provider-trusts)  
  
-   [Etapa 3: Exportar terceira relações de confiança de terceiros](#step-3-export-relying-party-trusts)  
  
-   [Etapa 4: Backup repositórios de atributo personalizado](#step-4-back-up-custom-attribute-stores)  
  
-   [Etapa 5: Fazer backup de personalizações de página da Web](#step-5-back-up-webpage-customizations)  
  
## <a name="step-1-export-service-settings"></a>Etapa 1: Exportar configurações de serviço  
 Para exportar configurações de serviço, execute o seguinte procedimento:  
  
### <a name="to-export-service-settings"></a>Para exportar configurações de serviço  
  
1.  Registre o certificado assunto valor de nome e a impressão digital do certificado SSL usado pelo serviço de Federação. Para localizar o certificado SSL, abra o gerenciamento de serviços de informações da Internet (IIS) do console, selecione **site padrão** no painel esquerdo, clique em **associações...** No **ação** painel, encontre e selecione a ligação https, clique em **editar**e clique em **exibição**.  
  
> [!NOTE]
>  Opcionalmente, você também pode exportar o certificado SSL usado pelo serviço de Federação e sua chave privada em um arquivo. pfx. Para obter mais informações, consulte [exportar a parte de chave privada de um certificado de autenticação de servidor ](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md).  
>   
>  Exportar o certificado SSL é opcional, pois esse certificado é armazenado no repositório de certificados pessoal do computador local e será preservado na atualização do sistema operacional.  
  
2.  Registre a configuração das comunicações de serviço do AD FS, certificados de descriptografia de token e o token de assinatura.  Para exibir todos os certificados que são usados, abra o Windows PowerShell e execute o seguinte comando para adicionar os cmdlets do AD FS a sessão do Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Em seguida, execute o seguinte comando para criar uma lista de todos os certificados em uso em um arquivo `PSH:>Get-ADFSCertificate | Out-File “.\certificates.txt”`  
  
> [!NOTE]
>  Opcionalmente, você também pode exportar qualquer token de assinatura, criptografia de token ou comunicações de serviço certificados e chaves não internamente gerados, além de todos os certificados autoassinados. Você pode exibir todos os certificados que estão em uso no servidor usando o Windows PowerShell. Abra o Windows PowerShell e execute o seguinte comando para adicionar os cmdlets do AD FS a sessão do Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell`. Em seguida, execute o seguinte comando para exibir todos os certificados que estão em uso no seu servidor `PSH:>Get-ADFSCertificate`. A saída desse comando inclui valores StoreLocation e StoreName que especificam o local do repositório de cada certificado. Você pode usar as orientações no [exportar a parte de chave privada de um certificado de autenticação de servidor](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md) para exportar cada certificado e sua chave privada para um arquivo. pfx.  
>   
>  Exportar esses certificados é opcional, pois todos os certificados externos são preservados durante a atualização do sistema operacional.  
  
3.  Exportar AD FS 2.0 federação propriedades do serviço, como o nome do serviço de federação, nome de exibição do serviço de Federação e identificador de servidor de federação para um arquivo.  
  
Para exportar as propriedades do serviço de federação, abra o Windows PowerShell e execute o seguinte comando para adicionar os cmdlets do AD FS a sessão do Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Em seguida, execute o seguinte comando para exportar as propriedades do serviço de Federação:`PSH:> Get-ADFSProperties | Out-File “.\properties.txt”`.  
  
O arquivo de saída conterá os seguintes valores de configuração importantes:  
  
    
|**Nome da propriedade de serviço de Federação conforme relatado pelo Get-ADFSProperties**|**Nome da propriedade de serviço de federação no console de gerenciamento do AD FS**|
|------|------|
|Nome do host|Nome do serviço de Federação|  
|Identificador|Identificador de serviço de Federação|  
|DisplayName|Nome de exibição do serviço de Federação|  
  
4.  Faça backup do arquivo de configuração do aplicativo. Entre outras configurações, esse arquivo contém a cadeia de caracteres de conexão de banco de dados de política.  
  
Para fazer backup do arquivo de configuração do aplicativo, você deve copiar manualmente os `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`arquivo para um local seguro no servidor de backup.  
  
> [!NOTE]
>  Anote a cadeia de caracteres de conexão de banco de dados nesse arquivo, localizada imediatamente após "policystore connectionstring ="). Se a cadeia de caracteres de conexão Especifica um banco de dados do SQL Server, o valor é necessária ao restaurar a configuração original do AD FS no servidor de Federação.  
>   
>  A seguir está um exemplo de uma cadeia de caracteres de conexão de trabalho:`“Data Source=\\.\pipe\mssql$microsoft##ssee\sql\query;Initial Catalog=AdfsConfiguration;Integrated Security=True"`. A seguir está um exemplo de uma cadeia de caracteres de conexão do SQL Server:`"Data Source=databasehostname;Integrated Security=True"`.  
  
5.  Registre a identidade da conta de serviço de Federação 2.0 do AD FS e a senha dessa conta.  
  
Para localizar o valor de identidade, examine o **fazer logon como** coluna de **AD FS 2.0 Windows serviço** no **serviços** do console e registrar manualmente esse valor.  
  
> [!NOTE]
>  Para um serviço de Federação autônomo, a conta de serviço de rede interno é usada.  Nesse caso, você não precisa ter uma senha.  
  
6.  Exporte a lista de pontos de extremidade do AD FS habilitados para um arquivo.  
  
Para fazer isso, abra o Windows PowerShell e execute o seguinte comando para adicionar os cmdlets do AD FS a sessão do Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Em seguida, execute o seguinte comando para exportar a lista de pontos de extremidade do AD FS habilitados para um arquivo:`PSH:> Get-ADFSEndpoint | Out-File “.\endpoints.txt”`.  
  
7.  Exporte descrições qualquer reivindicação personalizado para um arquivo.  
  
Para fazer isso, abra o Windows PowerShell e execute o seguinte comando para adicionar os cmdlets do AD FS a sessão do Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Em seguida, execute o seguinte comando para exportar descrições qualquer reivindicação personalizado para um arquivo:`Get-ADFSClaimDescription | Out-File “.\claimtypes.txt”`.  
  
##  <a name="step-2-export-claims-provider-trusts"></a>Etapa 2: Exportar declarações relações de confiança do provedor  
 Para exportar as relações de confiança do provedor de declarações, execute o seguinte procedimento:  
  
### <a name="to-export-claims-provider-trusts"></a>Para exportar declarações relações de confiança do provedor  
  
1.  Você pode usar o Windows PowerShell para exportar todas as relações de confiança de provedor de declarações. Abra o Windows PowerShell e execute o seguinte comando para adicionar os cmdlets do AD FS a sessão do Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Em seguida, execute o seguinte comando para exportar todas as relações de confiança de provedor de declarações:`PSH:>Get-ADFSClaimsProviderTrust | Out-File “.\cptrusts.txt”`.  
  
## <a name="step-3-export-relying-party-trusts"></a>Etapa 3: Exportar terceira relações de confiança de terceiros  
 Para exportar terceira relações de confiança de terceiros, execute o seguinte procedimento:  
  
### <a name="to-export-relying-party-trusts"></a>Para exportar terceira relações de confiança de terceiros  
  
1.  Para exportar terceira todas as relações de confiança de terceiros, abra o Windows PowerShell e execute o seguinte comando para adicionar os cmdlets do AD FS a sessão do Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Em seguida, execute o seguinte comando para Exportar tudo terceira relações de confiança de terceiros:`PSH:>Get-ADFSRelyingPartyTrust | Out-File “.\rptrusts.txt”`.  
  
## <a name="step-4-back-up-custom-attribute-stores"></a>Etapa 4: Backup repositórios de atributo personalizado  
 Você pode encontrar informações sobre o atributo personalizado lojas em uso pelo AD FS usando o Windows PowerShell. Abra o Windows PowerShell e execute o seguinte comando para adicionar os cmdlets do AD FS a sessão do Windows PowerShell:`PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Em seguida, execute o seguinte comando para encontrar informações sobre os repositórios do atributo personalizado:`PSH:>Get-ADFSAttributeStore`. As etapas para atualizar ou migrar o atributo personalizado lojas variam.  
  
## <a name="step-5-back-up-webpage-customizations"></a>Etapa 5: Fazer backup de personalizações de página da Web  
 Para fazer backup de todas as personalizações página da Web, copie as páginas da Web do AD FS e o **Web. config** arquivo da pasta que é mapeada para o caminho virtual **"/ adfs/ls"** no IIS. Por padrão, ele é o **%systemdrive%\inetpub\adfs\ls** diretório.  

## <a name="next-steps"></a>Próximas etapas
 [Preparar para migrar o servidor de Federação 2.0 do AD FS](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparar para migrar o Proxy de servidor de Federação 2.0 do AD FS](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar o servidor de Federação 2.0 do AD FS](migrate-the-ad-fs-fed-server.md)   
 [Migrar o Proxy de servidor de Federação 2.0 do AD FS](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar os agentes do AD FS Web 1.1](migrate-the-ad-fs-web-agent.md)