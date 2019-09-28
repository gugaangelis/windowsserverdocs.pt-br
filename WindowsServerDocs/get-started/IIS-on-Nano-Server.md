---
title: IIS do Nano Server
description: Detalhes para configurar do IIS no Nano Server
ms.prod: windows-server
ms.service: na
manager: DonGill
ms.technology: server-nano
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 09/06/2017
ms.assetid: 16984724-2d77-4d7b-9738-3dff375ed68c
author: jaimeo
ms.author: jaimeo
ms.localizationpriority: medium
ms.openlocfilehash: 04c2d7eab2f149505758ab21f08cd6b8bdb74b85
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71360296"
---
# <a name="iis-on-nano-server"></a>IIS do Nano Server

>Aplica-se a: Windows Server 2016

> [!IMPORTANT]
> A partir do Windows Server, versão 1709, o Nano Server estará disponível somente como uma [imagem do sistema operacional de base de contêiner](/virtualization/windowscontainers/quick-start/using-insider-container-images#install-base-container-image). Confira [Mudanças no Nano Server](nano-in-semi-annual-channel.md) para saber o que isso significa. 

Você pode instalar a função de servidor IIS (Serviços de Informações da Internet) no Nano Server usando o parâmetro -Package com Microsoft-NanoServer-IIS-Package. Para saber mais sobre como configurar o Nano Server, incluindo a instalação dos pacotes, confira [Instalar o Nano Server](Getting-Started-with-Nano-Server.md).  

Nesta versão do Nano Server, os seguintes recursos do IIS estão disponíveis:  

|Recurso|Habilitado por padrão|  
|-----------|----------------------|  
|**Recursos HTTP comuns**||  
|Documento padrão|x|  
|Pesquisa no diretório|x|  
|Erros de HTTP|x|  
|Conteúdo estático|x|  
|Redirecionamento HTTP||  
|**Integridade e diagnóstico**||  
|Log de FTP|x|  
|Log personalizado||  
|Monitor de solicitação||  
|Rastreamento||  
|**Desempenho**||  
|Compactação de conteúdo estático|x|  
|Compactação de conteúdo dinâmico||  
|**Segurança**||  
|Filtragem de solicitações|x|  
|Autenticação básica||  
|Autenticação do mapeamento de certificado de cliente||  
|Autenticação digest||  
|Autenticação do mapeamento de certificados de cliente do IIS||  
|Restrições de IP e de domínio||  
|Autorização de URL||  
|Autenticação do Windows||  
|**Desenvolvimento de aplicativo**||  
|Inicialização de aplicativo||  
|CGI||  
|Extensões ISAPI||  
|Filtros ISAPI||  
|Server-Side includes||  
|Protocolo WebSocket||  
|**Ferramentas de gerenciamento**||  
|Módulo de administração de IIS para Windows PowerShell|x|  

Uma série de artigos sobre outras configurações do IIS (por exemplo, como usar ASP.NET, PHP e Java), bem como outros conteúdos relacionados, estão publicados em [http://iis.net/learn](http://iis.net/learn).  

## <a name="installing-iis-on-nano-server"></a>Instalar o IIS no Nano Server  
Você pode instalar essa função de servidor offline (com o Nano Server desativado) ou online (com o Nano Server em execução); a instalação offline é a opção recomendada.  

Para a instalação offline, adicione o pacote com o parâmetro -Packages de New-NanoServerImage, como neste exemplo:  

`New-NanoServerImage -Edition Standard -DeploymentType Guest -MediaPath f:\ -BasePath .\Base -TargetPath .\Nano1.vhd -ComputerName Nano1 -Package Microsoft-NanoServer-IIS-Package`  

Se você tiver um arquivo VHD existente, poderá instalar o IIS offline com DISM.exe, montar o VHD e, em seguida, usar a opção **Add-Package**.   
As etapas de exemplo a seguir pressupõem que você esteja executando no diretório especificado pela opção BasePath, que foi criado após a execução de New-NanoServerImage.  

1.  mkdir mountdir
2.  .\Tools\dism.exe /Mount-Image /ImageFile:.\NanoServer.vhd /Index:1 /MountDir:.\mountdir
3.  .\Tools\dism.exe /Add-Package /PackagePath:.\packages\Microsoft-NanoServer-IIS-Package.cab /Image:.\mountdir
4.  .\Tools\dism.exe /Add-Package /PackagePath:.\packages\en-us\Microsoft-NanoServer-IIS-Package_en-us.cab /Image:.\mountdir
5.  .\Tools\dism.exe /Unmount-Image /MountDir:.\MountDir /Commit


> [!NOTE]  
> Observe que a Etapa 4 adicione o pacote de idiomas--este exemplo instala EN-US.  

Neste ponto, você pode iniciar o Nano Server com IIS.  

### <a name="installing-iis-on-nano-server-online"></a>Instalar o IIS no Nano Server online  
Embora seja recomendada a instalação offline da função de servidor, talvez seja necessário instalá-la online (com o Nano Server em execução) em cenários de contêiner. Para fazer isso, execute estas etapas:  

1.  Copie a pasta Pacotes da mídia de instalação localmente no Nano Server em execução (por exemplo, em C:\packages.).  

2.  Crie um novo arquivo Unattend.xml em outro computador e, em seguida, copie-o no Nano Server. Você pode copiar e colar esse conteúdo XML no arquivo XML que você criou:  



```  

    <unattend xmlns="urn:schemas-microsoft-com:unattend">  
    <servicing>  
        <package action="install">  
            <assemblyIdentity name="Microsoft-NanoServer-IIS-Package" version="10.0.14393.0" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="neutral" />  
            <source location="c:\packages\Microsoft-NanoServer-IIS-Package.cab" />  
        </package>  
        <package action="install">  
            <assemblyIdentity name="Microsoft-NanoServer-IIS-Package" version="10.0.14393.0" processorArchitecture="amd64" publicKeyToken="31bf3856ad364e35" language="en-US" />  
            <source location="c:\packages\en-us\Microsoft-NanoServer-IIS-Package_en-us.cab" />  
        </package>  
    </servicing>  
    <cpi:offlineImage cpi:source="" xmlns:cpi="urn:schemas-microsoft-com:cpi" />  
</unattend>  
```  




3. No novo arquivo XML criado (ou copiado), edite C:\packages para o diretório no qual você copiou o conteúdo de Pacotes.  

4. Alternar para o diretório com o arquivo XML criado recentemente e executar  

   **dism /online /apply-unattend:.\unattend.xml**  


5. Confirme se o pacote do IIS e seu pacote de idiomas associado estão instalados corretamente, executando:  

   **dism /online /get-packages**  

   Você deve ver "Package Identity: Microsoft-NanoServer-IIS-Package~31bf3856ad364e35~amd64~~10.0.14393.1000" listado duas vezes, uma vez para Release Type: Language Pack e uma vez para Release Type: Feature Pack.  

6. Inicie o serviço W3SVC com **net start w3svc** ou reiniciando o Nano Server.  

## <a name="starting-iis"></a>Iniciar o IIS  
Após a instalação e execução do IIS, ele estará pronto para atender às solicitações da Web. Verifique se o IIS está em execução procurando a página Web padrão do IIS em http://\<endereço IP do Nano Server>. Em um computador físico, você pode determinar o endereço IP usando o Console de Recuperação. Em uma máquina virtual, você pode obter o endereço IP usando um prompt do Windows PowerShell e executando:  

`Get-VM -name <VM name> | Select -ExpandProperty networkadapters | select IPAddresses`  

Se você não conseguir acessar a página Web padrão do IIS, verifique a instalação do IIS procurando o diretório **c:\inetpub** no Nano Server.  

## <a name="enabling-and-disabling-iis-features"></a>Habilitar e desabilitar os recursos do IIS  
Uma quantidade de recursos do IIS é habilitada por padrão quando você instala a função do IIS (confira a tabela na seção "Visão geral do IIS em Nano Server" deste tópico). Você pode habilitar (ou desabilitar) recursos adicionais usando DISM.exe

Cada recurso do IIS existe como um conjunto de elementos de configuração. Por exemplo, o recurso de autenticação do Windows é composto por estes elementos:  

|Seção|Elementos de configuração|  
|-----------|--------------------------|  
|`<globalModules>`|`<add name="WindowsAuthenticationModule" image="%windir%\System32\inetsrv\authsspi.dll`|  
|`<modules>`|`<add name="WindowsAuthenticationModule" lockItem="true" \/>`|  
|`<windowsAuthentication>`|`<windowsAuthentication enabled="false" authPersistNonNTLM\="true"><providers><add value="Negotiate" /><add value="NTLM" /><br /></providers><br /></windowsAuthentication>`|  

O conjunto completo de sub-recursos do IIS está incluído no Apêndice 1 deste tópico, e seus elementos de configuração correspondente estão incluídos no Apêndice 2 deste tópico.  


### <a name="example-installing-windows-authentication"></a>Exemplo: instalar a autenticação do Windows  

1.  Abra um console de sessão remota do Windows PowerShell no Nano Server.  

2.  Use `DISM.exe`para instalar o módulo de autenticação do Windows:

    ```
    dism /Enable-Feature /online /featurename:IIS-WindowsAuthentication /all
    ```

    A opção `/all` instalará qualquer recurso do qual o recurso escolhido depende.

### <a name="example-uninstalling-windows-authentication"></a>Exemplo: desinstalar a autenticação do Windows  

1.  Abra um console de sessão remota do Windows PowerShell no Nano Server.  

2.  Use `DISM.exe` para desinstalar o módulo de autenticação do Windows:

    ```
    dism /Disable-Feature /online /featurename:IIS-WindowsAuthentication
    ```

## <a name="other-common-iis-configuration-tasks"></a>Outras tarefas comuns de configuração do IIS  
**Criar sites**  

Use este cmdlet:  

`PS D:\> New-IISSite -Name TestSite -BindingInformation "*:80:TestSite" -PhysicalPath c:\test`  

Você pode executar `Get-IISSite` para verificar o estado do site (retorna o nome do site, ID de estado, o caminho físico e associações).  

**Excluir sites**  

Execute `Remove-IISSite -Name TestSite -Confirm:$false`.  

**Criar diretórios virtuais**  

Você pode criar diretórios virtuais usando o objeto IISServerManager retornado por Get-IISServerManager, que expõe a API Microsoft.Web.Administration.ServerManager do .NET. Neste exemplo, esses comandos acessam o elemento "Site Padrão" da coleção Sites e o elemento do aplicativo raiz ("/") da seção Aplicativos. Em seguida, eles chamam o método Add() da coleção VirtualDirectories desse elemento de aplicativo para criar o novo diretório:  

```  
PS C:\> $sm = Get-IISServerManager  
PS C:\> $sm.Sites["Default Web Site"].Applications["/"].VirtualDirectories.Add("/DemoVirtualDir1", "c:\test\virtualDirectory1")  
PS C:\> $sm.Sites["Default Web Site"].Applications["/"].VirtualDirectories.Add("/DemoVirtualDir2", "c:\test\virtualDirectory2")  
PS C:\> $sm.CommitChanges()  
```  

**Criar pools de aplicativos**  

Da mesma forma, você pode usar Get-IISServerManager para criar pools de aplicativos:  

```  
PS C:\> $sm = Get-IISServerManager  
PS C:\> $sm.ApplicationPools.Add("DemoAppPool")  
```  

**Configurar HTTPS e certificados**  

Use o utilitário Certoc.exe para importar certificados, como neste exemplo, que mostra como configurar o HTTPS para um site em um Nano Server:  

1.  Em outro computador que não esteja executando o Nano Server, crie um certificado (usando seu o nome e senha de seu próprio certificado) e, em seguida, exporte-o para c:\temp\test.pfx.  

    `$newCert = New-SelfSignedCertificate -DnsName "www.foo.bar.com" -CertStoreLocation cert:\LocalMachine\my`  

    `$mypwd = ConvertTo-SecureString -String "YOUR_PFX_PASSWD" -Force -AsPlainText`  

    `Export-PfxCertificate -FilePath c:\temp\test.pfx -Cert $newCert -Password $mypwd`  

2.  Copie o arquivo test.pfx no computador do Nano Server.  

3.  No Nano Server, importe o certificado para o repositório "My" com este comando:  

    **certoc.exe -ImportPFX -p YOUR_PFX_PASSWD My c:\temp\test.pfx**  

4.  Recuperar a impressão digital desse certificado novo (no exemplo, 61E71251294B2A7BB8259C2AC5CF7BA622777E73) com `Get-ChildItem Cert:\LocalMachine\my`.  

5.  Adicione a associação HTTPS ao Site padrão (ou qualquer site que ao qual você deseja adicionar a associação) usando estes comandos do Windows PowerShell:  

    ```  
    $certificate = get-item Cert:\LocalMachine\my\61E71251294B2A7BB8259C2AC5CF7BA622777E73  
    # Use your actual thumbprint instead of this example  
    $hash = $certificate.GetCertHash()  

    Import-Module IISAdministration  
    $sm = Get-IISServerManager  
    $sm.Sites["Default Web Site"].Bindings.Add("*:443:", $hash, "My", "0")    # My is the certificate store name  
    $sm.CommitChanges()  
    ```  

    Também é possível usar a SNI (Indicação de Nome de Servidor) com um nome de host específico com esta sintaxe: `$sm.Sites["Default Web Site"].Bindings.Add("*:443:www.foo.bar.com", $hash, "My", "Sni".`  

## <a name="appendix-1-list-of-iis-sub-features"></a>Apêndice 1: Lista de sub-recursos do IIS

- IIS-WebServer
- IIS-CommonHttpFeatures
- IIS-StaticContent
- IIS-DefaultDocument
- IIS-DirectoryBrowsing
- IIS-HttpErrors
- IIS-HttpRedirect
- IIS-ApplicationDevelopment
- IIS-CGI
- IIS-ISAPIExtensions
- IIS-ISAPIFilter
- IIS-ServerSideIncludes
- IIS-WebSockets
- IIS-ApplicationInit
- IIS-Security
- IIS-BasicAuthentication
- IIS-WindowsAuthentication
- IIS-DigestAuthentication
- IIS-ClientCertificateMappingAuthentication
- IIS-IISCertificateMappingAuthentication
- IIS-URLAuthorization
- IIS-RequestFiltering
- IIS-IPSecurity
- IIS-CertProvider
- IIS-Performance
- IIS-HttpCompressionStatic
- IIS-HttpCompressionDynamic
- IIS-HealthAndDiagnostics
- IIS-HttpLogging
- IIS-LoggingLibraries
- IIS-RequestMonitor
- IIS-HttpTracing
- IIS-CustomLogging

## <a name="appendix-2-elements-of-http-features"></a>Apêndice 2: Elementos dos recursos HTTP  
Cada recurso do IIS existe como um conjunto de elementos de configuração. Este apêndice lista os elementos de configuração de todos os recursos nesta versão do Nano Server  

### <a name="common-http-features"></a>Recursos HTTP comuns  
**Documento padrão**  

|Seção|Elementos de configuração|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name="DefaultDocumentModule" image="%windir%\System32\inetsrv\defdoc.dll" />`|  
|`<modules>`|`<add name="DefaultDocumentModule" lockItem="true" />`|  
|`<handlers>`|`<add name="StaticFile" path="*" verb="*" modules="DefaultDocumentModule" resourceType="EiSecther" requireAccess="Read" />`|  
|`<defaultDocument>`|`<defaultDocument enabled="true"><br /><files><br /> <add value="Default.htm" /><br />        <add value="Default.asp" /><br />        <add value="index.htm" /><br />        <add value="index.html" /><br />        <add value="iisstart.htm" /><br />    </files><br /></defaultDocument>`|  

A entrada `StaticFile <handlers>` já pode estar presente; nesse caso, basta adicionar "DefaultDocumentModule" ao atributo \<modules>, separado por uma vírgula.  

**Pesquisa no diretório**  

|Seção|Elementos de configuração|  
|----------------|--------------------------|   
|`<globalModules>`|`<add name="DirectoryListingModule" image="%windir%\System32\inetsrv\dirlist.dll" />`|  
|`<modules>`|`<add name="DirectoryListingModule" lockItem="true" />`|  
|`<handlers>`|`<add name="StaticFile" path="*" verb="*" modules="DirectoryListingModule" resourceType="Either" requireAccess="Read" />`|  

A entrada `StaticFile <handlers>` já pode estar presente; nesse caso, basta adicionar "DirectoryListingModule" ao atributo \<modules>, separado por uma vírgula.  

**Erros de HTTP**  

|Seção|Elementos de configuração|  
|----------------|--------------------------|   
|`<globalModules>`|`<add name="CustomErrorModule" image="%windir%\System32\inetsrv\custerr.dll" />`|  
|`<modules>`|`<add name="CustomErrorModule" lockItem="true" />`|  
|`<httpErrors>`|`<httpErrors lockAttributes="allowAbsolutePathsWhenDelegated,defaultPath"><br />    <error statusCode="401"    prefixLanguageFilePath="%SystemDrive%\inetpub\custerr" path="401.htm" ><br />    <error statusCode="403" prefixLanguageFilePath="%SystemDrive%\inetpub\custerr" path="403.htm" /><br />    <error statusCode="404" prefixLanguageFilePath="%SystemDrive%\inetpub\custerr" path="404.htm" /><br />    <error statusCode="405" prefixLanguageFilePath="%SystemDrive%\inetpub\custerr" path="405.htm" /><br />    <error statusCode="406" prefixLanguageFilePath="%SystemDrive%\inetpub\custerr" path="406.htm" /><br />    <error statusCode="412" prefixLanguageFilePath="%SystemDrive%\inetpub\custerr" path="412.htm" /><br />    <error statusCode="500" prefixLanguageFilePath="%SystemDrive%\inetpub\custerr" path="500.htm" /><br />    <error statusCode="501" prefixLanguageFilePath="%SystemDrive%\inetpub\custerr" path="501.htm" /><br />    <error statusCode="502" prefixLanguageFilePath="%SystemDrive%\inetpub\custerr" path="502.htm" /><br /></httpErrors>`|  

**Conteúdo estático**  

|Seção|Elementos de configuração|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name="StaticFileModule" image="%windir%\System32\inetsrv\static.dll" />`|  
|`<modules>`|`<add name="StaticFileModule" lockItem="true" />`|  
|`<handlers>`|`<add name="StaticFile" path="*" verb="*" modules="StaticFileModule" resourceType="Either" requireAccess="Read" />`|  

A entrada `StaticFile \<handlers>` já pode estar presente; nesse caso, basta adicionar "StaticFileModule" ao atributo \<modules>, separado por uma vírgula.  

**Redirecionamento HTTP**  

|Seção|Elementos de configuração|  
|----------------|--------------------------|    
|`<globalModules>`|`<add name="HttpRedirectionModule" image="%windir%\System32\inetsrv\redirect.dll" />`|  
|`<modules>`|`<add name="HttpRedirectionModule" lockItem="true" />`|  
|`<httpRedirect>`|`<httpRedirect enabled="false" />`|  

### <a name="health-and-diagnostics"></a>Integridade e diagnóstico  
**Log de HTTP**  

|Seção|Elementos de configuração|  
|----------------|--------------------------|   
|`<globalModules>`|`<add name="HttpLoggingModule" image="%windir%\System32\inetsrv\loghttp.dll" />`|  
|`<modules>`|`<add name="HttpLoggingModule" lockItem="true" />`|  
|`<httpLogging>`|`<httpLogging dontLog="false" />`|  

**Log personalizado**  

|Seção|Elementos de configuração|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name="CustomLoggingModule" image="%windir%\System32\inetsrv\logcust.dll" />`|  
|`<modules>`|`<add name="CustomLoggingModule" lockItem="true" />`|  

**Monitor de solicitação**  

|Seção|Elementos de configuração|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name="RequestMonitorModule" image="%windir%\System32\inetsrv\iisreqs.dll" />`|  

**Rastreamento**  

|Seção|Elementos de configuração|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name="TracingModule" image="%windir%\System32\inetsrv\iisetw.dll" \/><br /><add name="FailedRequestsTracingModule" image="%windir%\System32\inetsrv\iisfreb.dll" />`|  
|`<modules>`|`<add name="FailedRequestsTracingModule" lockItem="true" />`|  
|`<traceProviderDefinitions>`|`<traceProviderDefinitions><br />    <add name="WWW Server" guid\="{3a2a4e84-4c21-4981-ae10-3fda0d9b0f83}"><br />        <areas><br />            <clear /><br />            <add name="Authentication" value="2" /><br />            <add name="Security" value="4" /><br />            <add name="Filter" value="8" /><br />            <add name="StaticFile" value="16" /><br />            <add name="CGI" value="32" /><br />            <add name="Compression" value="64" /><br />            <add name="Cache" value="128" /><br />            <add name="RequestNotifications" value="256" /><br />            <add name="Module" value="512" /><br />            <add name="FastCGI" value="4096" /><br />            <add name="WebSocket" value="16384" /><br />        </areas><br />    </add><br />    <add name="ISAPI Extension" guid="{a1c2040e-8840-4c31-ba11-9871031a19ea}"><br />        <areas><br />            <clear /><br />        </areas><br />    </add><br /></traceProviderDefinitions>`|  

### <a name="performance"></a>Desempenho  
**Compactação de conteúdo estático**  

|Seção|Elementos de configuração|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name="StaticCompressionModule" image="%windir%\System32\inetsrv\compstat.dll" />`|  
|`<modules>`|`<add name="StaticCompressionModule" lockItem="true" />`|  
|`<httpCompression>`|`<httpCompression directory="%SystemDrive%\inetpub\temp\IIS Temporary Compressed Files"><br />    <scheme name="gzip" dll="%Windir%\system32\inetsrv\gzip.dll" /><br />   <staticTypes><br />        <add mimeType="text/*" enabled="true" /><br />        <add mimeType="message/*" enabled="true" /><br />        <add mimeType="application/javascript" enabled="true" \/><br />        <add mimeType="application/atom+xml" enabled="true" /><br />        <add mimeType="application/xaml+xml" enabled="true" /><br />        <add mimeType="\*\*" enabled="false" /><br />    </staticTypes><br /></httpCompression>`|  

**Compactação de conteúdo dinâmico**  

|Seção|Elementos de configuração|  
|-----------|--------------------------|  
|`<globalModules>`|`<add name="DynamicCompressionModule" image="%windir%\System32\inetsrv\compdyn.dll" />`|  
|`<modules>`|`<add name="DynamicCompressionModule" lockItem="true" />`|  
|`<httpCompression>`|`<httpCompression directory\="%SystemDrive%\inetpub\temp\IIS Temporary Compressed Files"><br />    <scheme name="gzip" dll="%Windir%\system32\inetsrv\gzip.dll" \/><br />    \<dynamicTypes><br />        <add mimeType="text/*" enabled="true" \/><br />        <add mimeType="message/*" enabled="true" /><br />        <add mimeType="application/x-javascript" enabled="true" /><br />        <add mimeType="application/javascript" enabled="true" /><br />        <add mimeType="*/*" enabled="false" /><br />    <\/dynamicTypes><br /></httpCompression>`|  

### <a name="security"></a>Segurança  
**Filtragem de solicitações**  


|       Seção        |                                                                                                                                        Elementos de configuração                                                                                                                                        |
|----------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|  `<globalModules>`   |                                                                                                        `<add name="RequestFilteringModule" image="%windir%\System32\inetsrv\modrqflt.dll" />`                                                                                                        |
|     `<modules>`      |                                                                                                                       `<add name="RequestFilteringModule" lockItem="true" />`                                                                                                                        |
| \`<requestFiltering> | `<requestFiltering><br />    <fileExtensions allowUnlisted="true" applyToWebDAV="true" /><br />    <verbs allowUnlisted="true" applyToWebDAV="true" /><br />    <hiddenSegments applyToWebDAV="true"><br />        <add segment="web.config" /><br />    </hiddenSegments><br /></requestFiltering>` |

**Autenticação básica**  

|Seção|Elementos de configuração|  
|----------------|--------------------------|   
|`<globalModules>`|`<add name="BasicAuthenticationModule" image="%windir%\System32\inetsrv\authbas.dll" />`|  
|`<modules>`|`<add name="WindowsAuthenticationModule" lockItem="true" />`|  
|`<basicAuthentication>`|`<basicAuthentication enabled="false" />`|  

**Autenticação do mapeamento de certificado de cliente**  

|Seção|Elementos de configuração|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name="CertificateMappingAuthentication" image="%windir%\System32\inetsrv\authcert.dll" />`|  
|`<modules>`|`<add name="CertificateMappingAuthenticationModule" lockItem="true" />`|  
|`<clientCertificateMappingAuthentication>`|`<clientCertificateMappingAuthentication enabled="false" />`|  

**Autenticação Digest**  

|Seção|Elementos de configuração|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name="DigestAuthenticationModule" image="%windir%\System32\inetsrv\authmd5.dll" />`|  
|`<modules>`|`<add name="DigestAuthenticationModule" lockItem="true" />`|  
|`<other>`|`<digestAuthentication enabled="false" />`|  

**Autenticação do mapeamento de certificados de cliente do IIS**  


|                  Seção                   |                                         Elementos de configuração                                         |
|--------------------------------------------|--------------------------------------------------------------------------------------------------------|
|             `<globalModules>`              | `<add name="CertificateMappingAuthenticationModule" image="%windir%\System32\inetsrv\authcert.dll" />` |
|                `<modules>`                 |               `<add name="CertificateMappingAuthenticationModule" lockItem="true" `/>\`                |
| `<clientCertificateMappingAuthentication>` |                      `<clientCertificateMappingAuthentication enabled="false" />`                      |

**Restrições de IP e domínio**  

|Seção|Elementos de configuração|  
|----------------|--------------------------|  
|`<globalModules>`|```<add name="IpRestrictionModule" image="%windir%\System32\inetsrv\iprestr.dll" /><br /><add name="DynamicIpRestrictionModule" image="%windir%\System32\inetsrv\diprestr.dll" />```|  
|`<modules>`|`<add name="IpRestrictionModule" lockItem="true" \/><br /><add name="DynamicIpRestrictionModule" lockItem="true" \/>`|  
|`<ipSecurity>`|`<ipSecurity allowUnlisted="true" />`|  

**Autorização de URL**  

|Seção|Elementos de configuração|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name="UrlAuthorizationModule" image="%windir%\System32\inetsrv\urlauthz.dll" />`|  
|`<modules>`|`<add name="UrlAuthorizationModule" lockItem="true" />`|  
|`<authorization>`|`<authorization><br />    <add accessType="Allow" users="*" /><br /></authorization>`|  

**Autenticação do Windows**  

|Seção|Elementos de configuração|  
|----------------|--------------------------|    
|`<globalModules>`|`<add name="WindowsAuthenticationModule" image="%windir%\System32\inetsrv\authsspi.dll" />`|  
|`<modules>`|`<add name="WindowsAuthenticationModule" lockItem="true" />`|  
|`<windowsAuthentication>`|`<windowsAuthentication enabled="false" authPersistNonNTLM\="true"><br />    <providers><br />        <add value="Negotiate" /><br />        <add value="NTLM" /><br />    <\providers><br /><\windowsAuthentication><windowsAuthentication enabled="false" authPersistNonNTLM\="true"><br />    <providers><br />        <add value="Negotiate" /><br />        <add value="NTLM" /><br />    <\/providers><br /><\/windowsAuthentication>`|  

### <a name="application-development"></a>Desenvolvimento de aplicativo  
**Inicialização de aplicativos**  

|Seção|Elementos de configuração|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name="ApplicationInitializationModule" image="%windir%\System32\inetsrv\warmup.dll" />`|  
|`<modules>`|`<add name="ApplicationInitializationModule" lockItem="true" />`|  

**CGI**  

|Seção|Elementos de configuração|  
|----------------|--------------------------|  
|`<globalModules>`|`<add name="CgiModule" image="%windir%\System32\inetsrv\cgi.dll" /><br /><add name="FastCgiModule" image="%windir%\System32\inetsrv\iisfcgi.dll" />`|  
|`<modules>`|`<add name="CgiModule" lockItem="true" /><br /><add name="FastCgiModule" lockItem="true" />`|  
|`<handlers>`|`<add name="CGI-exe" path="*.exe" verb="\*" modules="CgiModule" resourceType="File" requireAccess="Execute" allowPathInfo="true" />`|  

**Extensões ISAPI**  

|Seção|Elementos de configuração|  
|----------------|--------------------------|    
|`<globalModules>`|`<add name="IsapiModule" image="%windir%\System32\inetsrv\isapi.dll" />`|  
|`<modules>`|`<add name="IsapiModule" lockItem="true" />`|  
|`<handlers>`|`<add name="ISAPI-dll" path="*.dll" verb="*" modules="IsapiModule" resourceType="File" requireAccess="Execute" allowPathInfo="true" />`|  

**Filtros ISAPI**  

|Seção|Elementos de configuração|  
|----------------|--------------------------|    
|`<globalModules>`|`<add name="IsapiFilterModule" image="%windir%\System32\inetsrv\filter.dll" />`|  
|`<modules>`|`<add name="IsapiFilterModule" lockItem="true" />`|  

**Inclusões no lado do servidor**  

|Seção|Elementos de configuração|  
|----------------|--------------------------|  
|`<globalModules>`|<`add name="ServerSideIncludeModule" image="%windir%\System32\inetsrv\iis_ssi.dll" />`|  
|`<modules>`|`<add name="ServerSideIncludeModule" lockItem="true" />`|  
|`<handlers>`|`<add name="SSINC-stm" path="*.stm" verb="GET,HEAD,POST" modules="ServerSideIncludeModule" resourceType="File" \/><br /><add name="SSINC-shtm" path="*.shtm" verb="GET,HEAD,POST" modules="ServerSideIncludeModule" resourceType="File" /><br /><add name="SSINC-shtml" path="*.shtml" verb="GET,HEAD,POST" modules="ServerSideIncludeModule" resourceType="File" />`|  
|`<serverSideInclude>`|`<serverSideInclude ssiExecDisable="false" />`|  

**Protocolo WebSocket**  

|Seção|Elementos de configuração|  
|----------------|--------------------------|    
|`<globalModules>`|`<add name="WebSocketModule" image="%windir%\System32\inetsrv\iiswsock.dll" />`|  
|`<modules>`|`<add name="WebSocketModule" lockItem="true" />`|  