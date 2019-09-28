---
title: Preparar para migrar um servidor de Federação AD FS autônomo
description: Fornece informações sobre como preparar-se para migrar um servidor de AD FS autônomo para o Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 09b8cbd9097a95cd00b1413ce9e32ff9bf2f44c3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359317"
---
#  <a name="prepare-to-migrate-a-stand-alone-ad-fs-federation-server-or-a-single-node-ad-fs-farm"></a>Preparar para migrar um servidor de federação AD FS autônomo ou um farm de AD FS de nó único  
 
Para se preparar para migrar (mesma migração de servidor) um servidor de Federação AD FS 2,0 autônomo ou um farm de AD FS de nó único para o Windows Server 2012, você deve exportar e fazer backup dos dados de configuração do AD FS deste servidor.  
  
Para exportar os dados de configuração do AD FS, realize as seguintes tarefas:  
  
-   [Etapa 1:  Exportar configurações do serviço @ no__t-0  
  
-   [Etapa 2:  Exportar relações de confiança do provedor de declarações @ no__t-0  
  
-   [Etapa 3:  Exportar confianças de terceira parte confiável @ no__t-0  
  
-   [Etapa 4:  Fazer backup de repositórios de atributos personalizados @ no__t-0  
  
-   [Etapa 5:  Fazer backup de personalizações de página da Web @ no__t-0  
  
## <a name="step-1-export-service-settings"></a>Etapa 1: Exportar configurações do serviço  
 Para exportar configurações de serviço, realize o seguinte procedimento:  
  
### <a name="to-export-service-settings"></a>Para exportar as configurações de serviço  
  
1.  Registre o nome da entidade do certificado e o valor de impressão digital do certificado SSL usado pelo serviço de federação. Para localizar o certificado SSL, abra o console de gerenciamento do IIS (Serviços de Informações da Internet), selecione **Site Padrão** no painel esquerdo, clique em **Ligações…** no painel **Ação** , encontre e selecione a ligação https, clique em **Editar**e em **Exibir**.  
  
> [!NOTE]
>  Você pode também exportar o certificado SSL usado pelo serviço de federação e sua chave privada para um arquivo .pfx. Para obter mais informações, consulte [Exportar a parte da chave privada de um Certificado de Autenticação de Servidor](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md).  
>   
>  A exportação do certificado SSL é opcional, pois esse certificado é armazenado no repositório de certificados pessoais do computador local e é preservado na atualização do sistema operacional.  
  
2. Registre a configuração dos certificados de comunicações, descriptografia de token e autenticação de token do Serviço AD FS.  Para exibir todos os certificados usados​​, abra o Windows PowerShell e execute o seguinte comando para adicionar os cmdlets do AD FS à sessão do Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Em seguida, execute o seguinte comando para criar uma lista de todos os certificados em uso em um arquivo `PSH:>Get-ADFSCertificate | Out-File “.\certificates.txt”`  
  
> [!NOTE]
>  Você também pode exportar todos os certificados de autenticação de token, criptografia de token ou comunicações de serviço e chaves que não são gerados internamente, além de todos os certificados autoassinados. Você pode exibir todos os certificados que estão em uso no seu servidor usando o Windows PowerShell. Abra o Windows PowerShell e execute o seguinte comando para adicionar os cmdlets do AD FS à sessão do Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell`. Em seguida, execute o seguinte comando para exibir todos os certificados que estão em uso no servidor `PSH:>Get-ADFSCertificate`. A saída deste comando inclui os valores StoreLocation e StoreName que especificam o local de armazenamento de cada certificado. Você pode, então, usar as orientações em [Exportar a parte da chave privada de um certificado de autenticação de servidor](Export-the-Private-Key-Portion-of-a-Server-Authentication-Certificate.md) para exportar cada certificado e sua chave privada para um arquivo .pfx.  
>   
>  A exportação desses certificados é opcional, porque todos os certificados externos são preservados durante a atualização do sistema operacional.  
  
3. Exporte as propriedades do serviço de federação do AD FS 2.0, como o nome do serviço de federação, o nome de exibição do serviço de federação e o identificador do servidor de federação para um arquivo.  
  
Para exportar as propriedades do serviço de federação, abra o Windows PowerShell e execute o seguinte comando para adicionar os cmdlets do AD FS à sessão do Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Em seguida, execute o seguinte comando para exportar as propriedades do serviço de federação: `PSH:> Get-ADFSProperties | Out-File “.\properties.txt”`.  
  
O arquivo de saída conterá os valores de configuração importantes a seguir:  
  
    
|**Serviço de Federação nome da propriedade conforme relatado por Get-ADFSproperties**|**Serviço de Federação nome da propriedade no console de gerenciamento AD FS**|
|------|------|
|HostName|Nome do Serviço de Federação|  
|Identificador|Identificador do Serviço de Federação|  
|DisplayName|Nome de exibição do Serviço de Federação|  
  
4. Faça backup do arquivo de configuração do aplicativo. Entre outras configurações, esse arquivo contém a cadeia de conexão do banco de dados da política.  
  
Para fazer backup do arquivo de configuração do aplicativo, é preciso copiar manualmente o arquivo `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config` para um local seguro em um servidor de backup.  
  
> [!NOTE]
>  Anote a cadeia de conexão do banco de dados deste arquivo, localizada imediatamente após “policystore connectionstring=”). Se a cadeia de conexão especificar um banco de dados do SQL Server, o valor será necessário ao restaurar a configuração original do AD FS no servidor de federação.  
>   
>  Veja a seguir um exemplo de uma cadeia de conexão de WID: `“Data Source=\\.\pipe\mssql$microsoft##ssee\sql\query;Initial Catalog=AdfsConfiguration;Integrated Security=True"`. Veja a seguir um exemplo de uma cadeia de conexão de SQL Server: `"Data Source=databasehostname;Integrated Security=True"`.  
  
5. Registre a identidade da conta de serviço de federação do AD FS 2.0 e a senha dessa conta.  
  
Para encontrar o valor da identidade, examine a coluna **Fazer logon como** do **Windows Service do AD FS 2.0** no console **Serviços** e registre manualmente esse valor.  
  
> [!NOTE]
>  Para um serviço de federação autônomo, a conta SERVIÇO DE REDE interna é usada.  Nesse caso, não é necessário ter uma senha.  
  
6. Exporte a lista de pontos de extremidade do AD FS habilitados para um arquivo.  
  
Para isso, abra o Windows PowerShell e execute o seguinte comando para adicionar os cmdlets do AD FS à sessão do Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Em seguida, execute o seguinte comando para exportar a lista de pontos habilitados de extremidade do AD FS para um arquivo: `PSH:> Get-ADFSEndpoint | Out-File “.\endpoints.txt”`.  
  
7. Exporte todas as descrições de declarações personalizadas para um arquivo.  
  
Para isso, abra o Windows PowerShell e execute o seguinte comando para adicionar os cmdlets do AD FS à sessão do Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Depois, execute o seguinte comando para exportar todas as descrições de declaração personalizadas para um arquivo: `Get-ADFSClaimDescription | Out-File “.\claimtypes.txt”`.  
  
##  <a name="step-2-export-claims-provider-trusts"></a>Etapa 2: Exportar objetos de confiança do provedor de declarações  
 Para exportar objetos de confiança do provedor de declarações, execute o seguinte procedimento:  
  
### <a name="to-export-claims-provider-trusts"></a>Para exportar os objetos de confiança do provedor de declarações  
  
1.  Você pode usar o Windows PowerShell para exportar todos os objetos de confiança do provedor de declarações. Abra o Windows PowerShell e execute o seguinte comando para adicionar os cmdlets do AD FS à sessão do Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Em seguida, execute o seguinte comando para exportar todos os objetos de confiança do provedor de declarações: `PSH:>Get-ADFSClaimsProviderTrust | Out-File “.\cptrusts.txt”`.  
  
## <a name="step-3-export-relying-party-trusts"></a>Etapa 3: Exportar objetos de confiança da terceira parte confiável  
 Para exportar objetos de confiança da terceira parte confiável, execute o seguinte procedimento:  
  
### <a name="to-export-relying-party-trusts"></a>Para exportar os objetos de confiança da terceira parte confiável  
  
1.  Para exportar todos os objetos de confiança da terceira parte confiável, abra o Windows PowerShell e execute o seguinte comando para adicionar os cmdlets do AD FS à sessão do Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Em seguida, execute o seguinte comando para exportar todos os objetos de confiança do provedor de declarações:`PSH:>Get-ADFSRelyingPartyTrust | Out-File “.\rptrusts.txt”`.  
  
## <a name="step-4-back-up-custom-attribute-stores"></a>Etapa 4: Fazer backup de repositórios de atributos personalizados  
 É possível encontrar informações sobre repositórios de atributos personalizados em uso pelo AD FS usando o Windows PowerShell. Abra o Windows PowerShell e execute o seguinte comando para adicionar os cmdlets do AD FS à sessão do Windows PowerShell: `PSH:>add-pssnapin “Microsoft.adfs.powershell”`. Em seguida, execute o seguinte comando para encontrar informações sobre repositórios de atributos personalizados: `PSH:>Get-ADFSAttributeStore`. As etapas para atualização ou migração de repositórios de atributos personalizados variam.  
  
## <a name="step-5-back-up-webpage-customizations"></a>Etapa 5: Fazer backup de personalizações de página da Web  
 Para fazer backup de todas as personalizações de página da Web, copie as páginas da Web do AD FS e o arquivo **web.config** do diretório mapeado para o caminho virtual **“/adfs/ls”** no IIS. Por padrão, ele fica no diretório **%systemdrive%\inetpub\adfs\ls**.  

## <a name="next-steps"></a>Próximas etapas
 [Preparar para migrar o servidor de federação AD FS 2,0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparar para migrar o proxy do servidor de federação AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Migrar o servidor de federação AD FS 2,0](migrate-the-ad-fs-fed-server.md)   
 [Migrar o proxy do servidor de federação AD FS 2,0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Migrar os Agentes Web do AD FS 1.1](migrate-the-ad-fs-web-agent.md)