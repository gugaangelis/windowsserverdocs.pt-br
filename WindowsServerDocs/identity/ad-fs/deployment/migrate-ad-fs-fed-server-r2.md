---
title: "Migrar o servidor de Federação 2.0 do AD FS"
description: "Fornece informações sobre como migrar um servidor do AD FS para Windows Server 2012 R2."
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: bef24f79cfa92dfeca1846501f14ebf6d8231f0d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="migrate-the-ad-fs-20-federation-server-to-ad-fs-on-windows-server-2012-r2"></a>Migrar o servidor de Federação 2.0 AD FS para o AD FS no Windows Server 2012 R2

Para migrar um servidor de Federação do AD FS que pertence a um farm de AD FS do nó único, um farm WIF ou um farm de SQL Server ao Windows Server 2012 R2, você deve executar as seguintes tarefas:  
  
1.  [Exportar e fazer backup dos dados de configuração do AD FS](migrate-ad-fs-fed-server-r2.md#export-and-backup-the-ad-fs-configuration-data)  
  
2.  [Criar um farm de servidores de Federação do Windows Server 2012 R2](migrate-ad-fs-fed-server-r2.md#create-a-windows-server-2012-r2-federation-server-farm)  
  
3.  [Importe os dados de configuração original para o farm do Windows Server 2012 R2 AD FS](migrate-ad-fs-fed-server-r2.md#import-the-original-configuration-data-into-the-windows-server-2012-r2-ad-fs-farm)  
  
##  <a name="export-and-backup-the-ad-fs-configuration-data"></a>Exportar e fazer backup dos dados de configuração do AD FS  
 Para exportar as definições de configuração do AD FS, execute os procedimentos a seguir:  
  
###  <a name="to-export-service-settings"></a>Para exportar configurações de serviço  
  
1.  Certifique-se de que você tenha acesso aos seguintes certificados e suas chaves privadas em um arquivo. pfx:  
  
    -   O certificado SSL que é usado pelo farm do servidor de federação que você deseja migrar  
  
    -   O certificado de comunicação de serviço (se ele for diferente do certificado SSL) que é usado pelo farm do servidor de federação que você deseja migrar  
  
    -   Todos os certificados de assinatura de token ou token de criptografia/descriptografia de terceiros que são usados pelo farm do servidor de federação que você deseja migrar  
  
Para localizar o certificado SSL, abra o gerenciamento de serviços de informações da Internet (IIS) do console, selecione **site padrão** no painel esquerdo, clique em **associações...** No **ação** painel, encontre e selecione a ligação https, clique em **editar**e clique em **exibição**.  
  
Exporte o certificado SSL usado pelo serviço de Federação e sua chave privada em um arquivo. pfx. Para obter mais informações, consulte [exportar a parte de chave privada de um certificado de autenticação de servidor](export-the-private-key-portion-of-a-server-authentication-certificate.md).  
  
> [!NOTE]
>  Se você planeja implantar o serviço de registro de dispositivo como parte da execução o AD FS no Windows Server 2012 R2, você deve obter um novo certificado SSL. Para obter mais informações, consulte [registrar um certificado SSL do AD FS](enroll-an-ssl-certificate-for-ad-fs.md) e [configurar um servidor de federação com o serviço de registro de dispositivo](configure-a-federation-server-with-device-registration-service.md).  
  
Para exibir o token de assinatura, token descriptografia e certificados de comunicação de serviço que são usados, execute o seguinte comando do Windows PowerShell para criar uma lista de todos os certificados em uso em um arquivo:  
  
``` powershell
Get-ADFSCertificate | Out-File “.\certificates.txt”  
 ```  
  
2.  Exporte propriedades de serviço de Federação do AD FS, como o nome do serviço de federação, o nome de exibição do serviço de Federação e o identificador de servidor de federação para um arquivo.  
  
Para exportar as propriedades do serviço de federação, abra o Windows PowerShell e execute o seguinte comando: 

``` powershell
Get-ADFSProperties | Out-File “.\properties.txt”`.  
``` 

O arquivo de saída conterá os seguintes valores de configuração importantes:  
 
|**Nome da propriedade de serviço de Federação conforme relatado pelo Get-ADFSProperties**|**Nome da propriedade de serviço de federação no console de gerenciamento do AD FS**|
|-----|-----|  
|Nome do host|Nome do serviço de Federação|  
|Identificador|Identificador de serviço de Federação|  
|DisplayName|Nome de exibição do serviço de Federação|  
  
3.  Faça backup do arquivo de configuração do aplicativo. Entre outras configurações, esse arquivo contém a cadeia de caracteres de conexão de banco de dados de política.  
  
Para fazer backup do arquivo de configuração do aplicativo, você deve copiar manualmente os `%programfiles%\Active Directory Federation Services 2.0\Microsoft.IdentityServer.Servicehost.exe.config`arquivo para um local seguro no servidor de backup.  
  
> [!NOTE]
>  Anote a cadeia de caracteres de conexão de banco de dados nesse arquivo, localizada imediatamente após "policystore connectionstring =". Se a cadeia de caracteres de conexão Especifica um banco de dados do SQL Server, o valor é necessária ao restaurar a configuração original do AD FS no servidor de Federação.  
>   
>  A seguir está um exemplo de uma cadeia de caracteres de conexão de trabalho:`“Data Source=\\.\pipe\mssql$microsoft##ssee\sql\query;Initial Catalog=AdfsConfiguration;Integrated Security=True"`. A seguir está um exemplo de uma cadeia de caracteres de conexão do SQL Server:`"Data Source=databasehostname;Integrated Security=True"`.  
  
4.  Registre a identidade da conta de serviço de Federação do AD FS e a senha dessa conta.  
  
Para localizar o valor de identidade, examine o **fazer logon como** coluna de **AD FS 2.0 Windows serviço** no **serviços** do console e registrar manualmente esse valor.  
  
> [!NOTE]
>  Para um serviço de Federação autônomo, a conta de serviço de rede interno é usada.  Nesse caso, você não precisa ter uma senha.  
  
5.  Exporte a lista de pontos de extremidade do AD FS habilitados para um arquivo.  
  
Para fazer isso, abra o Windows PowerShell e execute o seguinte comando: 

``` powershell
Get-ADFSEndpoint | Out-File “.\endpoints.txt”`.  
``` 

6.  Exporte descrições qualquer reivindicação personalizado para um arquivo.  
  
Para fazer isso, abra o Windows PowerShell e execute o seguinte comando: 

``` powershell
Get-ADFSClaimDescription | Out-File “.\claimtypes.txt”`.  
 ```

7.  Se você tiver configurações personalizadas, como useRelayStateForIdpInitiatedSignOn configuradas no arquivo Web. config, certifique-se de que você faça backup do arquivo Web. config para referência. Você pode copiar o arquivo da pasta que é mapeada para o caminho virtual **"/ adfs/ls"** no IIS. Por padrão, ele é o **%systemdrive%\inetpub\adfs\ls** diretório.  
  
###  <a name="to-export-claims-provider-trusts-and-relying-party-trusts"></a>Para exportar requerimentos judiciais ou Extrajudiciais provedor relações de confiança e terceira parte  
  
1.  Para exportar o AD FS declarações relações de confiança do provedor e confiar relações de confiança de terceiros, você deve faça logon como administrador (no entanto, não como administrador do domínio) em seu federação servidor e execute o seguinte do Windows PowerShell script ou seja localizado no **server_support/mídia/adfs** pasta do CD de instalação do Windows Server 2012 R2:`export-federationconfiguration.ps1`.  
  
> [!IMPORTANT]
>  O script de exportação aceita os seguintes parâmetros:  
>   
>  -   . Ps1 Export-FederationConfiguration-caminho < string\ > [-ComputerName < string\ >] [-credenciais < pscredential\ >] [-força] [-CertificatePassword < securestring\ >]  
> -   . Ps1 Export-FederationConfiguration-caminho < string\ > [-ComputerName < string\ >] [-credenciais < pscredential\ >] [-força] [-CertificatePassword < securestring\ >] [-RelyingPartyTrustIdentifier < cadeia de caracteres [] >] [-ClaimsProviderTrustIdentifier < cadeia de caracteres [] >]  
> -   . Ps1 Export-FederationConfiguration-caminho < string\ > [-ComputerName < string\ >] [-credenciais < pscredential\ >] [-força] [-CertificatePassword < securestring\ >] [-RelyingPartyTrustName < cadeia de caracteres [] >] [-ClaimsProviderTrustName < cadeia de caracteres [] >]  
>   
>  **-RelyingPartyTrustIdentifier < cadeia de caracteres [] >** - as exportações somente cmdlet dependência relações de confiança de terceiros cujos identificadores são especificados na matriz de cadeia de caracteres. O padrão é exportar NONE as terceira relações de confiança de terceiros. Se nenhuma das RelyingPartyTrustIdentifier, ClaimsProviderTrustIdentifier, RelyingPartyTrustName e ClaimsProviderTrustName for especificada, o script exportará todos terceiro relações de confiança e relações de confiança do provedor de declarações.  
>   
>  **-ClaimsProviderTrustIdentifier < cadeia de caracteres [] >** -o cmdlet exporta apenas relações de confiança do provedor requerimentos judiciais ou Extrajudiciais cujos identificadores são especificados na matriz de cadeia de caracteres. O padrão é exportar NONE as relações de confiança do provedor de declarações.  
>   
>  **-RelyingPartyTrustName < cadeia de caracteres [] >** - as exportações somente cmdlet dependência relações de confiança de terceiros cujos nomes são especificados na matriz de cadeia de caracteres. O padrão é exportar NONE as terceira relações de confiança de terceiros.  
>   
>  **-ClaimsProviderTrustName < cadeia de caracteres [] >** -o cmdlet exporta apenas relações de confiança do provedor requerimentos judiciais ou Extrajudiciais cujos nomes são especificados na matriz de cadeia de caracteres. O padrão é exportar NONE as relações de confiança do provedor de declarações.  
>   
>  **-Path < string\ >** -o caminho para uma pasta que contenha os arquivos exportados.  
>   
>  **-ComputerName < string\ >** -Especifica o nome de host do servidor STS. O padrão é o computador local. Se você estiver migrando o AD FS 2.0 ou AD FS no Windows Server 2012 para o AD FS no Windows Server 2012 R2, esse é o nome de host do servidor do AD FS herdado.  
>   
>  **-Credenciais < PSCredential\ >** -Especifica uma conta de usuário que tem permissão para executar essa ação. O padrão é o usuário atual.  
>   
>  **-Forçar** – especifica para não prompt de confirmação do usuário.  
>   
>  **-CertificatePassword < SecureString\ >** -Especifica uma senha para exportar chaves privadas dos certificados do AD FS. Se não for especificado, o script solicitará uma senha, se precisar de um certificado AD FS com chave privada seja exportada.  
>   
>  **Entradas**: None  
>   
>  **Saídas**: cadeia de caracteres - este cmdlet retorna o caminho da pasta de exportação. Você pode direcionar o objeto retornado para Import-FederationConfiguration.  
  
###  <a name="to-back-up-custom-attribute-stores"></a>Fazer backup de repositórios de atributo personalizado  
  
1.  Exporte manualmente todas as lojas de atributo personalizado que você deseja manter em seu novo farm AD FS no Windows Server 2012 R2.  
  
> [!NOTE]
>  No Windows Server 2012 R2, o AD FS requer repositórios de atributo personalizado que são baseados no .NET Framework 4.0 ou acima. Siga as instruções em [do Microsoft .NET Framework 4.5](https://www.microsoft.com/download/details.aspx?id=30653) para instalar e configurar o .net Framework 4.5.  
  
Você pode encontrar informações sobre o atributo personalizado lojas em uso pelo AD FS executando o seguinte comando do Windows PowerShell: 

``` powershell
Get-ADFSAttributeStore
```  

As etapas para atualizar ou migrar o atributo personalizado lojas variam.  
  
2.  Exporte também manualmente todos os arquivos. dll das lojas atributo personalizado que você deseja manter em seu novo farm AD FS no Windows Server 2012 R2. As etapas para atualizar ou migrar arquivos. dll do atributo personalizado lojas variam.  
  
##  <a name="create-a-windows-server-2012-r2-federation-server-farm"></a>Criar um farm de servidores de Federação do Windows Server 2012 R2  
  
1.  Instale o sistema operacional Windows Server 2012 R2 em um computador que você deseja funcione como um servidor de Federação e, em seguida, adicione a função de servidor do AD FS. Para obter mais informações, consulte [instalar o serviço de função do AD FS](install-the-ad-fs-role-service.md). Em seguida, configure seu novo serviço de federação por meio do Assistente de configuração do serviço de Federação Active Directory ou por meio do Windows PowerShell. Para obter mais informações, consulte "Configurar o servidor de Federação primeiro em um novo farm de servidores de federação" em [configurar um servidor de Federação](configure-a-federation-server.md).  

Ao concluir essa etapa, você deve seguir estas instruções:  
  
-   Você deve ter privilégios de administrador do domínio para configurar o serviço de Federação.  
  
-   Você deve usar o mesmo nome de serviço de Federação (nome da Fazenda) que foi usada no AD FS 2.0 ou AD FS no Windows Server 2012. Se você não usar o mesmo nome de serviço de federação, os certificados de backup não funcionará no serviço de Federação do Windows Server 2012 R2 que você está tentando configurar.  
  
-   Especifique se este é um farm de servidores de Federação do trabalho ou o SQL Server. Se for um farm de SQL, especifique o local de banco de dados do SQL Server e o nome de instância.  
  
-   Você deve fornecer um arquivo pfx que contenha o certificado de autenticação de servidor SSL backup como parte da preparação para o processo de migração do AD FS.  
  
-   Você deve especificar a mesma identidade da conta de serviço que foi usada no AD FS 2.0 ou AD FS no Windows Server 2012 farm.  
  
2.  Depois que o nó inicial estiver configurado, você pode adicionar nós adicionais para seu novo farm. Para obter mais informações, consulte "Adicionar um servidor de Federação a um farm de servidor de Federação existente" no [configurar um servidor de Federação](configure-a-federation-server.md).  
  
##  <a name="import-the-original-configuration-data-into-the-windows-server-2012-r2-ad-fs-farm"></a>Importe os dados de configuração original para o farm do Windows Server 2012 R2 AD FS  
 Agora que você tem um farm de servidores de Federação do AD FS em execução no Windows Server 2012 R2, você pode importar os dados de configuração do AD FS originais para ele.  
  
1.  Importar e configurar outros certificados do AD FS personalizados, incluindo externamente registrados no token de assinatura e criptografia-de descriptografia de token de certificados e o certificado de comunicação de serviço se ele for diferente do que o certificado SSL.  
  
No console de gerenciamento do AD FS, selecione **certificados**. Verifique se os certificados de token de criptografia/descriptografia e o token de assinatura de comunicações, serviço, verificando uns com os valores exportadas para o arquivo certificates.txt ao preparar para a migração.  
  
Para alterar os certificados de descriptografia de token ou token de assinatura de certificados autoassinados padrão para certificados externos, primeiro desabilite o recurso de sobreposição de certificado automática é habilitado por padrão. Para fazer isso, você pode usar o seguinte comando do Windows PowerShell:  
  
``` powershell 
Set-ADFSProperties –AutoCertificateRollover $false  
```  
  
2.  Configure quaisquer configurações de serviço do AD FS personalizadas, como vida AutoCertificateRollover ou logon único usando o cmdlet Set-AdfsProperties.  
  
3.  Para importar o AD FS terceira parte relações de confiança e requerimentos judiciais ou Extrajudiciais provedor, você deve estar conectado como administrador (no entanto, não como administrador do domínio) em seu federação servidor e execute o seguinte do Windows PowerShell script ou seja localizado na pasta \support\adfs do CD de instalação do Windows Server 2012 R2:  
  
``` powershell 
import-federationconfiguration.ps1  
```  
  
> [!IMPORTANT]
>  O script de importação aceita os seguintes parâmetros:  
>   
>  -  . Ps1 Import-FederationConfiguration-caminho < string\ > [-ComputerName < string\ >] [-credenciais < pscredential\ >] [-força] [-/LogPath < string\ >] [-CertificatePassword < securestring\ >]  
> -   . Ps1 Import-FederationConfiguration-caminho < string\ > [-ComputerName < string\ >] [-credenciais < pscredential\ >] [-força] [-/LogPath < string\ >] [-CertificatePassword < securestring\ >] [-RelyingPartyTrustIdentifier < cadeia de caracteres [] >] [-ClaimsProviderTrustIdentifier < cadeia de caracteres [] >  
> -   . Ps1 Import-FederationConfiguration-caminho < string\ > [-ComputerName < string\ >] [-credenciais < pscredential\ >] [-força] [-/LogPath < string\ >] [-CertificatePassword < securestring\ >] [-RelyingPartyTrustName < cadeia de caracteres [] >] [-ClaimsProviderTrustName < cadeia de caracteres [] >]  
>   
>  **-RelyingPartyTrustIdentifier < cadeia de caracteres [] >** - imports somente cmdlet dependência relações de confiança de terceiros cujos identificadores são especificados na matriz de cadeia de caracteres. O padrão é importar NONE as terceira relações de confiança de terceiros. Se nenhuma das RelyingPartyTrustIdentifier, ClaimsProviderTrustIdentifier, RelyingPartyTrustName e ClaimsProviderTrustName for especificada, o script importará todos terceiro relações de confiança e relações de confiança do provedor de declarações.  
>   
>  **-ClaimsProviderTrustIdentifier < cadeia de caracteres [] >** -o cmdlet importa apenas relações de confiança do provedor requerimentos judiciais ou Extrajudiciais cujos identificadores são especificados na matriz de cadeia de caracteres. O padrão é importar NONE as relações de confiança do provedor de declarações.  
>   
>  **-RelyingPartyTrustName < cadeia de caracteres [] >** - imports somente cmdlet dependência relações de confiança de terceiros cujos nomes são especificados na matriz de cadeia de caracteres. O padrão é importar NONE as terceira relações de confiança de terceiros.  
>   
>  **-ClaimsProviderTrustName < cadeia de caracteres [] >** -o cmdlet importa apenas relações de confiança do provedor requerimentos judiciais ou Extrajudiciais cujos nomes são especificados na matriz de cadeia de caracteres. O padrão é importar NONE as relações de confiança do provedor de declarações.  
>   
>  **-Path < string\ >** -o caminho para uma pasta que contém os arquivos de configuração para ser importado.  
>   
>  **-/LogPath < string\ >** -o caminho para uma pasta que contenha o arquivo de log de importação. Um arquivo de log chamado "Import" será criado nesta pasta.  
>   
>  **-ComputerName < string\ >** -Especifica o nome de host do servidor STS. O padrão é o computador local. Se você estiver migrando o AD FS 2.0 ou AD FS no Windows Server 2012 para o AD FS no Windows Server 2012 R2, este parâmetro deve ser definido para o nome do host do servidor do AD FS herdado.  
>   
>  **-Credenciais < PSCredential\ >**-Especifica uma conta de usuário que tem permissão para executar essa ação. O padrão é o usuário atual.  
>   
>  **-Forçar** – especifica para não prompt de confirmação do usuário.  
>   
>  **-CertificatePassword < SecureString\ >** -Especifica uma senha para importar chaves privadas dos certificados do AD FS. Se não for especificado, o script solicitará uma senha, se um certificado AD FS com chave privada precisa ser importado.  
>   
>  **Entradas:** cadeia de caracteres - esse comando leva o caminho da pasta importar como entrada. Você pode canalizar Export-FederationConfiguration para esse comando.  
>   
>  **Saídas:** None.  
  
Todos os espaços à direita na propriedade WSFedEndpoint de uma terceira confiança de terceiros podem fazer com que o script de importação de erro. Nesse caso, remova manualmente os espaços do antes do arquivo da importação. Por exemplo, essas entradas causam erros:  
  
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
>  Se você tiver qualquer reivindicação personalizado regras (regras que não sejam as regras de padrão do AD FS) em relação de confiança da Active Directory requerimentos judiciais ou Extrajudiciais provedor no sistema de origem, elas não serão migradas pelos scripts. Isso ocorre porque o Windows Server 2012 R2 tem novos padrões. Qualquer regras personalizadas devem ser mescladas, adicionando-os manualmente para a relação de confiança do Active Directory requerimentos judiciais ou Extrajudiciais provedor no novo farm Windows Server 2012 R2.  
  
4.  Configure todas as configurações personalizadas de ponto de extremidade do AD FS. No console de gerenciamento do AD FS, selecione **pontos de extremidade**. Verifique os pontos de extremidade do AD FS habilitados com a lista de pontos de extremidade do AD FS habilitados exportadas para um arquivo ao preparar para a migração do AD FS.  
  
     \ - E -  
  
     Configure descrições qualquer reivindicação personalizado. No console de gerenciamento do AD FS, selecione **reivindicação descrições**. Verifique a lista de descrições de declaração do AD FS contra a lista das descrições reivindicação que você exportou para um arquivo ao preparar para a migração do AD FS. Adicione descrições qualquer reivindicação personalizados incluídos no seu arquivo, mas não incluído na lista padrão no AD FS. Observe que o identificador de declaração no console de gerenciamento é mapeado para ClaimType no arquivo.  
  
5.  Instalar e configurar personalizado backup de todas as lojas de atributo. Como um administrador, certifique-se de que nenhum binário do repositório de atributo personalizado é atualização para o .NET Framework 4.0 ou posterior antes de atualizar a configuração do AD FS para aponta para eles.  
  
6.  Configure as propriedades de serviço de mapa para os parâmetros do arquivo Web. config herdado.  
  
    -   Se **useRelayStateForIdpInitiatedSignOn** foi adicionado para o **Web. config** do arquivo no seu AD FS 2.0 ou AD FS no Windows Server 2012 farm, em seguida, você deve configurar as seguintes propriedades de serviço no seu AD FS no Windows Server 2012 R2 farm:  
  
        -   AD FS no Windows Server 2012 R2 inclui um **%systemroot%\ADFS\Microsoft.IdentityServer.Servicehost.exe.config** arquivo. Crie um elemento com a mesma sintaxe como o **Web. config** elemento do arquivo:`<useRelayStateForIdpInitiatedSignOn enabled="true" />`. Inclua esse elemento como parte do **< Microsoft.identityserver.web >** seção o **Microsoft.IdentityServer.Servicehost.exe.config** arquivo.  
  
    -   Se **< persistIdentityProviderInformation habilitado = "true & #124; false" lifetimeInDays = enablewhrPersistence "90" = "true & #124; false" / \ >** foi adicionado para o **Web. config** do arquivo no seu AD FS 2.0 ou AD FS no Windows Server 2012 farm, em seguida, você deve configurar as seguintes propriedades de serviço no seu AD FS no Windows Server 2012 R2 farm:  
  
        1.  No AD FS no Windows Server 2012 R2, execute o seguinte comando do Windows PowerShell:`Set-AdfsWebConfig –HRDCookieEnabled –HRDCookieLifetime`.  
  
    -   Se **< singleSignOn habilitado = "true & #124; false" / \ >** foi adicionado para o **Web. config** arquivo no seu AD FS 2.0 ou AD FS no Windows Server 2012 farm, você não precisa definir as propriedades de um serviço adicional no seu AD FS no Windows Server 2012 R2 farm. Logon único é habilitado por padrão no AD FS no Windows Server 2012 R2 farm.  
  
    -   Se localAuthenticationTypes configurações foram adicionadas para o **Web. config** do arquivo no seu AD FS 2.0 ou AD FS no Windows Server 2012 farm, em seguida, você deve configurar as seguintes propriedades de serviço no seu AD FS no Windows Server 2012 R2 farm:  
  
        -   Integrado, formulários, TlsClient, lista Transform básicas em AD FS equivalente no Windows Server 2012 R2 tem configurações de política de autenticação global para dar suporte a ambos os tipos de autenticação de proxy e de serviço de Federação. Essas configurações podem ser definidas no AD FS no snap-in Gerenciamento sob o **políticas de autenticação**.  
  
 Depois de importar os dados de configuração original, você pode personalizar o log do AD FS nas páginas conforme necessário. Para obter mais informações, consulte [Personalizando as páginas do AD FS Sign-in](../operations/AD-FS-Customization-in-Windows-Server-2016.md).  
  
## <a name="next-steps"></a>Próximas etapas
 [Migrar Serviços de função de serviços de Federação do Active Directory para Windows Server 2012 R2](migrate-ad-fs-service-role-to-windows-server-r2.md)   
 [Preparando para migrar o servidor de Federação do AD FS](prepare-migrate-ad-fs-server-r2.md)   
 [Migrando o Proxy de servidor de Federação do AD FS](migrate-fed-server-proxy-r2.md)   
 [Verificando o AD FS migração ao Windows Server 2012 R2](verify-ad-fs-migration.md)