---
title: Migrar o servidor de Federação AD FS 2,0
description: Fornece informações sobre como migrar um servidor de AD FS para o Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d72217d9e8dc3b0f47382e08346dca977ac14b67
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867925"
---
# <a name="migrate-the-ad-fs-20-federation-server-to-ad-fs-on-windows-server-2012-r2"></a>Migrar o servidor de Federação AD FS 2,0 para AD FS no Windows Server 2012 R2

Para migrar um AD FS servidor de Federação que pertence a um farm de AD FS de nó único, um farm do WIF ou um farm de SQL Server para o Windows Server 2012 R2, você deve executar as seguintes tarefas:  
  
1.  [Exportar e fazer backup dos dados de configuração do AD FS](migrate-ad-fs-fed-server-r2.md#export-and-backup-the-ad-fs-configuration-data)  
  
2.  [Criar um farm de servidores de Federação do Windows Server 2012 R2](migrate-ad-fs-fed-server-r2.md#create-a-windows-server-2012-r2-federation-server-farm)  
  
3.  [Importar os dados de configuração originais para o farm de AD FS do Windows Server 2012 R2](migrate-ad-fs-fed-server-r2.md#import-the-original-configuration-data-into-the-windows-server-2012-r2-ad-fs-farm)  
  
##  <a name="export-and-backup-the-ad-fs-configuration-data"></a>Exportar e fazer backup dos dados de configuração do AD FS  
 Para exportar as definições de configuração do AD FS, execute os seguintes procedimentos:  
  
###  <a name="to-export-service-settings"></a>Para exportar as configurações de serviço  
  
1.  Verifique se você tem acesso aos seguintes certificados e suas chaves privadas em um arquivo .pfx:  
  
    -   O certificado SSL que é usado pelo farm do servidor de federação que você deseja migrar  
  
    -   O certificado de comunicação do serviço (se for diferente do certificado SSL) que é usado pelo farm do servidor de federação que você deseja migrar  
  
    -   Todos os certificados de criptografia/descriptografia de token ou autenticação de token de terceiros que são usados pelo farm do servidor de federação que você deseja migrar  
  
Para localizar o certificado SSL, abra o console de gerenciamento do IIS (Serviços de Informações da Internet), selecione **Site Padrão** no painel esquerdo, clique em **Ligações…** no painel **Ação** , encontre e selecione a ligação https, clique em **Editar**e em **Exibir**.  
  
É preciso exportar o certificado SSL usado pelo serviço de federação e sua chave privada para um arquivo .pfx. Para obter mais informações, consulte [Exportar a parte da chave privada de um Certificado de Autenticação de Servidor](export-the-private-key-portion-of-a-server-authentication-certificate.md).  
  
> [!NOTE]
>  Se você planeja implantar o serviço de registro de dispositivo como parte da execução do AD FS no Windows Server 2012 R2, você deve obter um novo certificado SSL. Para obter mais informações, consulte [Enroll an SSL Certificate for AD FS](enroll-an-ssl-certificate-for-ad-fs.md) e [Configurar um servidor de federação com o Serviço de Registro de Dispositivos](configure-a-federation-server-with-device-registration-service.md).  
  
Para exibir os certificados de assinatura de token, descriptografia de token e comunicação de serviços que são utilizados, execute o seguinte comando do Windows PowerShell para criar uma lista com todos os certificados em uso em um arquivo:  
  
``` powershell
Get-ADFSCertificate | Out-File “.\certificates.txt”  
 ```  
  
2. Exporte as propriedades do serviço de federação do AD FS, como nome do serviço de federação, nome de exibição do serviço de federação e identificador do servidor de federação, para um arquivo.  
  
Para exportar as propriedades do serviço de federação, abra o Windows PowerShell e execute o seguinte comando: 

``` powershell
Get-ADFSProperties | Out-File “.\properties.txt”`.  
``` 

O arquivo de saída conterá os valores de configuração importantes a seguir:  
 
|**Serviço de Federação nome da propriedade conforme relatado por Get-ADFSproperties**|**Serviço de Federação nome da propriedade no console de gerenciamento AD FS**|
|-----|-----|  
|HostName|Nome do Serviço de Federação|  
|Identificador|Identificador do Serviço de Federação|  
|DisplayName|Nome de exibição do Serviço de Federação|  
  
3. Faça backup do arquivo de configuração do aplicativo. Entre outras configurações, esse arquivo contém a cadeia de conexão do banco de dados da política.  
  
Para fazer backup do arquivo de configuração do aplicativo, é preciso copiar manualmente o arquivo `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config` para um local seguro em um servidor de backup.  
  
> [!NOTE]
>  Anote a cadeia de conexão de banco de dados deste arquivo, localizada imediatamente após “policystore connectionstring=”. Se a cadeia de conexão especificar um banco de dados do SQL Server, o valor será necessário ao restaurar a configuração original do AD FS no servidor de federação.  
>   
>  Veja a seguir um exemplo de uma cadeia de conexão de WID: `“Data Source=\\.\pipe\mssql$microsoft##ssee\sql\query;Initial Catalog=AdfsConfiguration;Integrated Security=True"`. Veja a seguir um exemplo de uma cadeia de conexão de SQL Server: `"Data Source=databasehostname;Integrated Security=True"`.  
  
4. Registre a identidade da conta de serviço de federação do AD FS e a senha dessa conta.  
  
Para encontrar o valor da identidade, examine a coluna **Fazer logon como** do **Windows Service do AD FS 2.0** no console **Serviços** e registre manualmente esse valor.  
  
> [!NOTE]
>  Para um serviço de federação autônomo, a conta SERVIÇO DE REDE interna é usada.  Nesse caso, não é necessário ter uma senha.  
  
5. Exporte a lista de pontos de extremidade do AD FS habilitados para um arquivo.  
  
Para fazer isso, abra o Windows PowerShell e execute o seguinte comando: 

``` powershell
Get-ADFSEndpoint | Out-File “.\endpoints.txt”`.  
``` 

6. Exporte todas as descrições de declarações personalizadas para um arquivo.  
  
Para fazer isso, abra o Windows PowerShell e execute o seguinte comando: 

``` powershell
Get-ADFSClaimDescription | Out-File “.\claimtypes.txt”`.  
 ```

7. Se você tiver configurações personalizadas, como useRelayStateForIdpInitiatedSignOn, configuradas no arquivo web.config, faça backup do arquivo web.config para consulta. É possível copiar o arquivo do diretório que é mapeado para o caminho virtual **“/adfs/ls”** no IIS. Por padrão, ele fica no diretório **%systemdrive%\inetpub\adfs\ls**.  
  
###  <a name="to-export-claims-provider-trusts-and-relying-party-trusts"></a>Para exportar os objetos de confiança do provedor de declarações e da terceira parte confiável  
  
1.  Para exportar AD FS relações de confiança de provedor de declarações e confianças de terceira parte confiável, você deve fazer logon como administrador (no entanto, não como o administrador de domínio) em seu servidor de Federação e executar o seguinte script do Windows PowerShell que está localizado na **mídia/server_support pasta/ADFS** do CD de instalação do Windows Server 2012 R2 `export-federationconfiguration.ps1`:.  
  
> [!IMPORTANT]
>  O script de exportação usa os seguintes parâmetros:  
> 
> - Export-FederationConfiguration. ps1-Path < String\> [-ComputerName < String\>] [-Credential < PSCredential\>] [-force] [-CertificatePassword < SecureString\>]  
>   -   Export-FederationConfiguration. ps1-Path < String\> [-ComputerName < String\>] [-Credential < PSCredential\>] [-force] [-CertificatePassword < SecureString\>] [- RelyingPartyTrustIdentifier < String [] >] [-ClaimsProviderTrustIdentifier < String [] >]  
>   -   Export-FederationConfiguration. ps1-Path < String\> [-ComputerName < String\>] [-Credential < PSCredential\>] [-force] [-CertificatePassword < SecureString\>] [- RelyingPartyTrustName < String [] >] [-ClaimsProviderTrustName < String [] >]  
> 
>   **-RelyingPartyTrustIdentifier <string[]>** - o cmdlet só exporta objetos de confiança da terceira parte confiável cujos identificadores estão especificados na matriz de cadeia de caracteres. O padrão é não exportar NENHUM dos objetos de confiança da terceira parte confiável. Se RelyingPartyTrustIdentifier, ClaimsProviderTrustIdentifier, RelyingPartyTrustName e ClaimsProviderTrustName não estiverem especificados, o script exportará todos os objetos de confiança da terceira parte confiável e do provedor de declarações.  
> 
>   **-ClaimsProviderTrustIdentifier <string[]>** - o cmdlet só exporta objetos de confiança do provedor de declarações cujos identificadores estão especificados na matriz de cadeia de caracteres. O padrão é não exportar NENHUM dos objetos de confiança do provedor de declarações.  
> 
>   **-RelyingPartyTrustName <string[]>** - o cmdlet só exporta objetos de confiança da terceira parte confiável cujos nomes estão especificados na matriz de cadeia de caracteres. O padrão é não exportar NENHUM dos objetos de confiança da terceira parte confiável.  
> 
>   **-ClaimsProviderTrustName <string[]>** - o cmdlet só exporta objetos de confiança do provedor de declarações cujos nomes estão especificados na matriz de cadeia de caracteres. O padrão é não exportar NENHUM dos objetos de confiança do provedor de declarações.  
> 
>   **-Path < cadeia\> de caracteres** -o caminho para uma pasta que conterá os arquivos exportados.  
> 
>   **-ComputerName < cadeia de\> caracteres** -especifica o nome do host do servidor STS. O padrão é o computador local. Se você estiver migrando o AD FS 2.0 ou o AD FS em Windows Server 2012 para o AD FS em Windows Server 2012 R2, esse será o nome do host do servidor do AD FS herdado.  
> 
>   **-Credential < PSCredential\>**  -especifica uma conta de usuário que tem permissão para executar esta ação. O padrão é o usuário atual.  
> 
>   **-Force** – especifica não solicitar confirmação do usuário.  
> 
>   **-CertificatePassword < SecureString\>**  -especifica uma senha para exportar chaves privadas de certificados AD FS. Se não estiver especificado, o script solicitará uma senha se um certificado do AD FS com chave privada precisar ser exportado.  
> 
>   **Inputs**: Nenhum  
> 
>   **Outputs**: string - este cmdlet retorna o caminho da pasta de exportação. É possível canalizar o objeto retornado para Import-FederationConfiguration.  
  
###  <a name="to-back-up-custom-attribute-stores"></a>Para fazer backup de repositórios de atributos personalizados  
  
1.  Você deve exportar manualmente todos os repositórios de atributos personalizados que deseja manter em seu novo farm de AD FS no Windows Server 2012 R2.  
  
> [!NOTE]
>  No Windows Server 2012 R2, AD FS requer repositórios de atributos personalizados baseados em .NET Framework 4,0 ou acima. Siga as instruções no [Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653) para instalar e configurar o .NET Framework 4.5.  
  
É possível encontrar informações sobre repositórios de atributos personalizados em uso pelo AD FS, executando o seguinte comando do Windows PowerShell: 

``` powershell
Get-ADFSAttributeStore
```  

As etapas para atualização ou migração de repositórios de atributos personalizados variam.  
  
2. Você também deve exportar manualmente todos os arquivos. dll dos repositórios de atributos personalizados que deseja manter em seu novo farm de AD FS no Windows Server 2012 R2. As etapas para atualização ou migração de arquivos .dll dos repositórios de atributos personalizados variam.  
  
##  <a name="create-a-windows-server-2012-r2-federation-server-farm"></a>Criar um farm de servidor de federação do Windows Server 2012 R2  
  
1.  Instale o sistema operacional Windows Server 2012 R2 em um computador que você deseja que funcione como um servidor de Federação e adicione a função de servidor AD FS. Para obter mais informações, consulte [Install the AD FS Role Service](install-the-ad-fs-role-service.md). Em seguida, configure seu novo serviço de federação pelo Assistente de configuração do serviço de federação do Active Directory ou pelo Windows PowerShell. Para obter mais informações, consulte "Configurar o primeiro servidor de federação em um novo farm de servidor de federação" em [Configurar um servidor de federação](configure-a-federation-server.md).  

Ao concluir essa etapa, é preciso seguir estas instruções:  
  
-   É preciso ter privilégios de Administrador de Domínio para configurar o serviço de federação.  
  
-   É preciso usar o mesmo nome do serviço de federação (nome do farm) utilizado no AD FS 2.0 ou no AD FS no Windows Server 2012. Se você não usar o mesmo nome de serviço de Federação, os certificados dos quais você fez backup não funcionarão no serviço de Federação do Windows Server 2012 R2 que você está tentando configurar.  
  
-   Especifique se trata-se de um farm de servidor de federação de WID ou de SQL Server. Se for um farm de SQL, especifique o local do banco de dados e o nome da instância do SQL Server.  
  
-   É preciso fornecer um arquivo .pfx que contenha o certificado de autenticação de servidor SSL do qual você fez o backup como parte da preparação para o processo de migração do AD FS.  
  
-   É preciso especificar a mesma identidade de conta de serviço que foi usada no AD FS 2.0 ou no AD FS, no farm do Windows Server 2012.  
  
2. Depois que o nó inicial é configurado, é possível adicionar nós adicionais ao novo farm. Para obter mais informações, consulte "Adicionar um servidor de federação a um novo farm de servidor de federação" em [Configurar um servidor de federação](configure-a-federation-server.md).  
  
##  <a name="import-the-original-configuration-data-into-the-windows-server-2012-r2-ad-fs-farm"></a>Importar os dados de configuração original para o farm do AD FS do Windows Server 2012 R2  
 Agora que você tem um farm de servidores de Federação AD FS em execução no Windows Server 2012 R2, você pode importar os dados de configuração de AD FS originais para ele.  
  
1.  Importe e configure outros certificados personalizados do AD FS, incluindo certificados de criptografia/descriptografia de token ou autenticação de token externamente inscritos, e o certificado de comunicação de serviço, se ele for diferente do certificado SSL.  
  
No console de gerenciamento do AD FS, selecione **Certificados**. Verifique os certificados de criptografia/descriptografia de token, autenticação de token e comunicações de serviço, comparando cada um com os valores que você exportou para o arquivo certificates.txt durante a preparação da migração.  
  
Para alterar os certificados de descriptografia de token e de autenticação de token dos certificados autoassinados padrão para certificados externos, é preciso primeiro desabilitar o recurso de sobreposição automática de certificados, que fica habilitado por padrão. Para fazer isso, é possível usar o seguinte comando do Windows PowerShell:  
  
``` powershell 
Set-ADFSProperties –AutoCertificateRollover $false  
```  
  
2. Defina todas as configurações personalizadas de serviço do AD FS como tempo de vida SSO ou AutoCertificateRollover, usando o cmdlet Set-AdfsProperties.  
  
3. Para importar AD FS relações de confiança de terceira parte confiável e provedor de declarações, você deve estar conectado como administrador (no entanto, não como o administrador de domínio) em seu servidor de Federação e executar o seguinte script do Windows PowerShell que está localizado na pasta \support\adfs do CD de instalação do Windows Server 2012 R2:  
  
``` powershell 
import-federationconfiguration.ps1  
```  
  
> [!IMPORTANT]
>  O script de importação usa os seguintes parâmetros:  
> 
> - Import-FederationConfiguration. ps1-Path < String\> [-ComputerName < String\>] [-Credential < PSCredential\>] [-force] [-logPath < String\>] [-CertificatePassword < SecureString \>]  
>   -   Import-FederationConfiguration. ps1-Path < String\> [-ComputerName < String\>] [-Credential < PSCredential\>] [-force] [-logPath < String\>] [-CertificatePassword < SecureString \>] [-RelyingPartyTrustIdentifier < String [] >] [-ClaimsProviderTrustIdentifier < String [] >  
>   -   Import-FederationConfiguration. ps1-Path < String\> [-ComputerName < String\>] [-Credential < PSCredential\>] [-force] [-logPath < String\>] [-CertificatePassword < SecureString \>] [-RelyingPartyTrustName < String [] >] [-ClaimsProviderTrustName < String [] >]  
> 
>   **-RelyingPartyTrustIdentifier <string[]>** - o cmdlet só importa objetos de confiança da terceira parte confiável cujos identificadores estão especificados na matriz de cadeia de caracteres. O padrão é não importar NENHUM dos objetos de confiança da terceira parte confiável. Se RelyingPartyTrustIdentifier, ClaimsProviderTrustIdentifier, RelyingPartyTrustName e ClaimsProviderTrustName não estiverem especificados, o script importará todos os objetos de confiança da terceira parte confiável e do provedor de declarações.  
> 
>   **-ClaimsProviderTrustIdentifier <string[]>** - o cmdlet só importa objetos de confiança do provedor de declarações cujos identificadores estão especificados na matriz de cadeia de caracteres. O padrão é não importar NENHUM dos objetos de confiança do provedor de declarações.  
> 
>   **-RelyingPartyTrustName <string[]>** - o cmdlet só importa objetos de confiança da terceira parte confiável cujos nomes estão especificados na matriz de cadeia de caracteres. O padrão é não importar NENHUM dos objetos de confiança da terceira parte confiável.  
> 
>   **-ClaimsProviderTrustName <string[]>** - o cmdlet só importa objetos de confiança do provedor de declarações cujos nomes estão especificados na matriz de cadeia de caracteres. O padrão é não importar NENHUM dos objetos de confiança do provedor de declarações.  
> 
>   **-Path < cadeia\> de caracteres** -o caminho para uma pasta que contém os arquivos de configuração a serem importados.  
> 
>   **-LogPath < cadeia\> de caracteres** -o caminho para uma pasta que conterá o arquivo de log de importação. Um arquivo de log chamado “import.log” será criado nesta pasta.  
> 
>   **-ComputerName < cadeia de\> caracteres** -especifica o nome do host do servidor STS. O padrão é o computador local. Se você estiver migrando o AD FS 2.0 ou o AD FS em Windows Server 2012 para o AD FS em Windows Server 2012 R2, esse parâmetro deverá ser definido como o nome do host do servidor AD FS herdado.  
> 
>   **-Credential < PSCredential\>** -especifica uma conta de usuário que tem permissão para executar esta ação. O padrão é o usuário atual.  
> 
>   **-Force** – especifica não solicitar confirmação do usuário.  
> 
>   **-CertificatePassword < SecureString\>**  -especifica uma senha para importar chaves privadas de certificados AD FS. Se não estiver especificado, o script solicitará uma senha se um certificado do AD FS com chave privada precisar ser importado.  
> 
>   **Inputs:** string - este comando utiliza o caminho da pasta de importação como entrada. É possível canalizar Export-FederationConfiguration para este comando.  
> 
>   **Saídas:** nenhuma.  
  
Os espaços à direita na propriedade WSFedEndpoint de uma terceira parte confiável podem causar erro na importação do script. Nesse caso, remova manualmente os espaços do arquivo antes da importação. Por exemplo, estas entradas causam erros:  
  
    ```  
    <URI N="WSFedEndpoint">https://127.0.0.1:444 /</URI>  
    ```  
  
    ```  
    <URI N="WSFedEndpoint">https://myapp.cloudapp.net:83 /</URI>  
    ```  
  
     They must be edited to:  
  
    ```  
    <URI N="WSFedEndpoint">https://127.0.0.1:444/</URI>  
    ```  
  
    ```  
    <URI N="WSFedEndpoint">https://myapp.cloudapp.net:83/</URI>  
    ```  
> [!IMPORTANT]
>  Se você tiver alguma regra de declaração personalizada (regras que não sejam as regras padrão do AD FS) nos objetos de confiança do provedor de declarações do Active Directory no sistema de origem, ela não será migrada pelos scripts. Isso ocorre porque o Windows Server 2012 R2 tem novos padrões. Todas as regras personalizadas devem ser mescladas adicionando-as manualmente à Active Directory confiança do provedor de declarações no novo farm do Windows Server 2012 R2.  
  
4. Configure todas as configurações personalizadas de ponto de extremidade do AD FS. No console de gerenciamento do AD FS, selecione **Pontos de extremidade**. Compare os pontos de extremidade do AD FS habilitados com a lista de pontos de extremidade do AD FS habilitados que você exportou para um arquivo durante a preparação para a migração do AD FS.  
  
    \-E  
  
    Configure todas as descrições de declarações personalizadas. No console de gerenciamento do AD FS, selecione **Descrições de declarações**. Compare a lista de descrições de declarações do AD FS com a lista de descrições de declarações que você exportou para um arquivo durante a preparação para a migração do AD FS. Adicione todas as descrições de declarações personalizadas de seu arquivo que não tenham sido incluídas na lista padrão do AD FS. Observe que o identificador de declarações no console de gerenciamento mapeia para ClaimType no arquivo.  
  
5. Instale e configure todos os repositórios de atributos personalizados com backup. Como administrador, verifique se todos os binários dos repositórios de atributos personalizados estão atualizados para o .NET Framework 4.0 ou superior antes de atualizar a configuração do AD FS para apontar para eles.  
  
6. Configure as propriedades de serviço que mapeiam para os parâmetros de arquivo web.config herdados.  
  
   -   Se **useRelayStateForIdpInitiatedSignOn** tiver sido adicionado ao arquivo **Web. config** em seu AD FS 2,0 ou AD FS no farm do Windows Server 2012, você deverá configurar as seguintes propriedades de serviço em seu AD FS no farm do Windows Server 2012 R2:  
  
       -   AD FS no Windows Server 2012 R2 inclui um arquivo **%systemroot%\ADFS\Microsoft.IdentityServer.ServiceHost.exe.config** . Crie um elemento com a mesma sintaxe do elemento de arquivo **Web. config** : `<useRelayStateForIdpInitiatedSignOn enabled="true" />`. Inclua este elemento como parte da **< seção Microsoft. identityserver. web >** do arquivo **Microsoft. identityserver. ServiceHost. exe. config** .  
  
   -   Se **< persistIdentityProviderInformation Enabled = "true&#124;false" lifetimeInDays = "90" enablewhrPersistence = "true&#124;false"/\>**  foi adicionado ao arquivo **Web. config** em seu AD FS 2,0 ou AD FS no Windows Server 2012 farm, você deve configurar as seguintes propriedades de serviço em seu AD FS no farm do Windows Server 2012 R2:  
  
       1.  Em AD FS no Windows Server 2012 R2, execute o seguinte comando do Windows PowerShell `Set-AdfsWebConfig –HRDCookieEnabled –HRDCookieLifetime`:.  
  
   -   Se **< logon único Enabled = "true&#124;false"/\>**  tiver sido adicionado ao arquivo **Web. config** em seu AD FS 2,0 ou AD FS no farm do Windows Server 2012, você não precisará definir nenhuma propriedade de serviço adicional em seu AD FS no Windows Server 2012 Farm R2. O logon único é habilitado por padrão no AD FS no farm do Windows Server 2012 R2.  
  
   -   Se as configurações de localAuthenticationTypes tiverem sido adicionadas ao arquivo **Web. config** em seu AD FS 2,0 ou AD FS no farm do windows Server 2012, você deverá configurar as seguintes propriedades de serviço em seu AD FS no farm do windows Server 2012 R2:  
  
       -   A lista de transformações integradas, formulários, TlsClient, básica em equivalentes AD FS no Windows Server 2012 R2 tem configurações de política de autenticação global para dar suporte tanto ao serviço de Federação quanto aos tipos de autenticação de proxy. Essas definições podem ser configuradas no AD FS, no snap-in Gerenciamento, em **Políticas de autenticação**.  
  
   Depois de importar os dados de configuração originais, é possível personalizar as páginas de entrada do AD FS conforme necessário. Para obter mais informações, consulte [Customizing the AD FS Sign-in Pages](../operations/AD-FS-Customization-in-Windows-Server-2016.md).  
  
## <a name="next-steps"></a>Próximas etapas
 [Migrar Serviços de Federação do Active Directory (AD FS) serviços de função para o Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Preparando para migrar o servidor de Federação AD FS](prepare-migrate-ad-fs-server-r2.md)   
 [Migrando o proxy do servidor de Federação AD FS](migrate-fed-server-proxy-r2.md)   
 [Verificando a migração de AD FS para o Windows Server 2012 R2](verify-ad-fs-migration.md)