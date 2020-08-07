---
title: Implantar o Windows Server Hybrid Cloud Print
description: Como configurar a impressão em nuvem híbrida da Microsoft
ms.assetid: fc239aec-e719-47ea-92fc-d82a7247c5e9
ms.topic: how-to
author: msjimwu
ms.author: coreyp
manager: dongill
ms.date: 3/15/2018
ms.openlocfilehash: 3e5b732beb502bb0bf365136947ff380caf71545
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87879917"
---
# <a name="deploy-windows-server-hybrid-cloud-print"></a>Implantar o Windows Server Hybrid Cloud Print

>Aplica-se a: Windows Server 2016

Este tópico, para administradores de ti, descreve a implantação de ponta a ponta da solução de impressão de nuvem híbrida da Microsoft (HCP). Essas camadas de solução sobre o Windows Server (s) existente em execução como Servidor de Impressão e habilita o Azure Active Directory (Azure AD) ingressado e os dispositivos gerenciados pelo MDM para descobrir e imprimir em impressoras gerenciadas pela organização.

## <a name="pre-requisites"></a>Pré-requisitos

Há várias assinaturas, serviços e computadores que você precisará adquirir antes de iniciar esta instalação. Elas são as seguintes:

- Assinatura do Azure AD Premium.

  Confira [introdução a uma assinatura do Azure](https://azure.microsoft.com/trial/get-started-active-directory/) para uma assinatura de avaliação do Azure.

- Serviço MDM, como o Intune.

  Consulte [Microsoft Intune](https://www.microsoft.com/cloud-platform/microsoft-intune) para obter uma assinatura de avaliação do Intune.

- O computador Windows Server 2016 ou posterior que executa o Active Directory.

  Consulte [passo a passo: Configurando Active Directory no Windows Server 2016](https://blogs.technet.microsoft.com/canitpro/2017/02/22/step-by-step-setting-up-active-directory-in-windows-server-2016/) para obter ajuda sobre como configurar o Active Directory.

- Uma máquina dedicada, ingressada no domínio do Windows Server 2016 ou posterior em execução como Servidor de Impressão.

- Um computador dedicado, ingressado no domínio do Windows Server 2016 ou posterior em execução como servidor do conector.

  Consulte [entender os conectores de proxy de aplicativo do AD do Azure](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-connectors) para obter mais informações.

- Uma atualização do criador de outono do Windows 10 ou um computador posterior para publicar impressoras.

- Nome de domínio voltado para o público.

  Você pode usar o nome de domínio criado para você pelo Azure (*DomainName*. onmicrosoft.com) ou comprar seu próprio nome de domínio. Consulte [Adicionar seu nome de domínio personalizado usando o portal de Azure Active Directory](https://docs.microsoft.com/azure/active-directory/fundamentals/add-custom-domain).

## <a name="deployment-steps"></a>Etapas de implantação.

As etapas a seguir são para uma implantação típica de impressão em nuvem híbrida.

### <a name="step-1---install-azure-ad-connect"></a>Etapa 1-instalar o Azure AD Connect

1. O Azure AD Connect sincroniza o Azure AD com o AD local. No computador Windows Server com Active Directory, baixe e instale o software Azure AD Connect com as configurações expressas. Consulte [introdução ao Azure ad Connect usando as configurações expressas](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-express).

### <a name="step-2---install-application-proxy"></a>Etapa 2 – instalar o proxy de aplicativo

1. O proxy de aplicativo permite que os usuários em sua organização acessem aplicativos locais da nuvem. Instale o proxy de aplicativo no servidor do conector.
    - Para obter instruções de instalação, consulte [tutorial: adicionar um aplicativo local para acesso remoto por meio do proxy de aplicativo no Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-add-on-premises-application).
    - Um grupo de conectores dedicado é recomendado se a organização tiver topologia de rede complexa. Consulte [publicar aplicativos em redes e locais separados usando grupos de conectores](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-connector-groups).

### <a name="step-3---register-and-configure-applications"></a>Etapa 3-registrar e configurar aplicativos

Para habilitar a comunicação autenticada com os serviços HCP, precisamos criar três aplicativos: 2 aplicativos Web para representar os dois serviços HCPs e um aplicativo nativo para se comunicar com esses serviços.

1. Faça logon em portal do Azure para registrar aplicativos Web.
    - Em Azure Active Directory, vá para **registros de aplicativo**  >  **novo registro**.

    ![Registro de aplicativo do AAD 1](../media/hybrid-cloud-print/AAD-AppRegistration.png)

    - Insira um nome de aplicativo para o serviço de descoberta do Mopria. Clique em **registrar** para concluir.

    ![Registro de aplicativo do AAD 2](../media/hybrid-cloud-print/AAD-AppRegistration-Mopria.png)

    - Repita para o serviço de impressão de nuvem empresarial.
    - Repita para o aplicativo nativo.
    - Os três aplicativos devem ser exibidos em **registros de aplicativo**.

    ![Registro de aplicativo do AAD 3](../media/hybrid-cloud-print/AAD-AppRegistration-AllApps.png)

2. Expor a API para os 2 aplicativos Web.
    - Ainda na folha **registros de aplicativo** , clique no aplicativo serviço de descoberta do Mopria, selecione **expor uma API**e clique em **definir** ao lado de ID do aplicativo URI.

    ![API de exposição do AAD 1](../media/hybrid-cloud-print/AAD-AppRegistration-Mopria-ExposeAPI.png)

    - Clique em **salvar** sem alterar o valor padrão para o URI da ID do aplicativo. Esse valor só precisa ser definido agora e será alterado posteriormente.

    ![API de exposição do AAD 2](../media/hybrid-cloud-print/AAD-AppRegistration-Mopria-ExposeAPI-Save.png)

    - Clique em **Adicionar um escopo**.

    ![API de exposição do AAD 3](../media/hybrid-cloud-print/AAD-AppRegistration-Mopria-ExposeAPI-AddScope.png)

    - Forneça um nome de escopo, permita que administradores e usuários consentim, insira a descrição de consentimento e clique em **Adicionar escopo** para concluir.

    ![API de exposição do AAD 4](../media/hybrid-cloud-print/AAD-AppRegistration-Mopria-ExposeAPI-ScopeName.png)

    - Repita para o serviço de impressão de nuvem empresarial. Use um nome de escopo e uma descrição de consentimento diferentes.

    ![API de exposição do AAD 5](../media/hybrid-cloud-print/AAD-AppRegistration-ECP-ExposeAPI-ScopeName.png)

3. Adicionar permissões de API
    - Retorne à folha Registros de aplicativo. Clique no aplicativo nativo e selecione permissões de API. Clique em **Adicionar uma permissão**.

    ![Permissão 1 da API do AAD](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission.png)

    - Mude para as **APIs que minha organização usa**e, em seguida, use a caixa de pesquisa para localizar o serviço de descoberta do Mopria adicionado anteriormente. Clique no serviço do resultado da pesquisa.

    ![Permissão 2 da API do AAD](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission-Mopria.png)

    - Selecione **permissões delegadas**. Marque a caixa ao lado do escopo da API. Clique em **adicionar permissões**.

    ![Permissão de API do AAD 3](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission-Mopria-Add.png)

    - Repita para adicionar permissões ao serviço de impressão de nuvem empresarial.

    ![Permissão de API do AAD 4](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission-ECP-Add.png)

    - Depois de retornar à folha permissões de API, aguarde 10 segundos antes de clicar em **consentimento de administrador global...**.

    ![Permissão 5 da API do AAD](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission-GrantConsent.png)

    - Clique em **Sim** quando solicitado.

    ![Permissão de API do AAD 6](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission-GrantConsent-Yes.png)

    - Verifique se a coluna status da permissão de API é exibida com marcas de seleção verdes.

    ![Permissão de API do AAD 7](../media/hybrid-cloud-print/AAD-AppRegistration-APIPermission-Verify.png)

4. Configurar o proxy de aplicativo para os aplicativos Web
    - Vá para **Azure Active Directory**  >  **aplicativos empresariais**  >  **todos os aplicativos**. Procure o serviço de descoberta do Mopria e clique nele.

    ![Proxy de aplicativo do AAD 1](../media/hybrid-cloud-print/AAD-EnterpriseApp-AllApps.png)

    - Clique em **proxy de aplicativo**. Insira a URL interna usando o formato `https://<fully qualified domain name of the Print Server>/mcs/` . Clique em **salvar** para concluir.

    ![Proxy de aplicativo do AAD 2](../media/hybrid-cloud-print/AAD-EnterpriseApp-Mopria-AppProxy.png)

    - Repita para o serviço de impressão de nuvem empresarial. Observe que a URL interna é `https://<fully qualified domain name of the Print Server>/ecp/` .

    ![Proxy de aplicativo do AAD 3](../media/hybrid-cloud-print/AAD-EnterpriseApp-ECP-AppProxy.png)

    - Vá para **Azure Active Directory**  >  **registros de aplicativo**. Clique no serviço de descoberta do Mopria. Em **visão geral**, observe que o URI da ID do aplicativo foi alterado do padrão para a URL externa em **proxy de aplicativo**. O URI será usado durante a instalação do Servidor de Impressão, na política de MDM do cliente e para a impressora de publicação.

    ![Proxy de aplicativo do AAD 4](../media/hybrid-cloud-print/AAD-AppRegistration-Mopria-Overview.png)

5. Atribuir usuários a aplicativos
    - Vá para **Azure Active Directory**  >  **aplicativos empresariais**  >  **todos os aplicativos**. Pesquise o serviço de descoberta do Mopria e clique nele
    - Clique em **usuários e grupos** e atribua usuários ou clique em **Propriedades** e altere a **atribuição de usuário necessária?** para **não**
    - Repita para o serviço de impressão de nuvem empresarial.

6. Configurar URI de redirecionamento no aplicativo nativo
    - Vá para **Azure Active Directory**  >  **registros de aplicativo**. Clique no aplicativo nativo. Acesse **visão geral** e copie a **ID do aplicativo (cliente)**.

    ![URI de redirecionamento do AAD 1](../media/hybrid-cloud-print/AAD-AppRegistration-Native-Overview.png)

    - Acesse **autenticação**. Altere a caixa suspensa **tipo** para `Public...` e insira dois URIs de redirecionamento usando o formato abaixo, em que `<NativeClientAppID>` é a partir da etapa anterior:

        `ms-appx-web://Microsoft.AAD.BrokerPlugin/<NativeClientAppID>`

        `ms-appx-web://Microsoft.AAD.BrokerPlugin/S-1-15-2-3784861210-599250757-1266852909-3189164077-45880155-1246692841-283550366`

    ![URI de redirecionamento do AAD 2](../media/hybrid-cloud-print/AAD-AppRegistration-Native-Authentication.png)

    - Clique em **salvar** para concluir.

### <a name="step-4---setup-the-print-server"></a>Etapa 4: configurar o Servidor de Impressão

1. Verifique se o Servidor de Impressão tem todas as Windows Update disponíveis instaladas. Observação: o servidor 2019 deve ser corrigido para a compilação 17763,165 ou posterior.
    - Instale as seguintes funções de servidor:
        - Servidor de Impressão função
        - IIS (Serviços de Informações da Internet)
    - Consulte [instalar funções, serviços de função e recursos usando o assistente para adicionar funções e recursos](https://docs.microsoft.com/windows-server/administration/server-manager/install-or-uninstall-roles-role-services-or-features#BKMK_installarfw) para obter detalhes sobre como instalar funções de servidor.

    ![Servidor de Impressão funções](../media/hybrid-cloud-print/PrintServer-Roles.png)

2. Instale os módulos do PowerShell de impressão em nuvem híbrida.
    - Execute os seguintes comandos em um prompt de comando do PowerShell com privilégios elevados:

        `find-module -Name PublishCloudPrinter`para confirmar que o computador pode alcançar o Galeria do PowerShell (PSGallery)

        `install-module -Name PublishCloudPrinter`

    > Observação: você pode ver uma mensagem informando que ' PSGallery ' é um repositório não confiável.  Digite ' y ' para continuar com a instalação.

    ![Servidor de Impressão publicar impressora de nuvem](../media/hybrid-cloud-print/PrintServer-PublishCloudPrinter.png)

3. Instale a solução de impressão de nuvem híbrida.
    - No mesmo prompt de comando do PowerShell com privilégios elevados, altere o diretório para aquele abaixo (aspas necessárias):

        `C:\Program Files\WindowsPowerShell\Modules\PublishCloudPrinter\1.0.0.0`

    - Executar

        `.\CloudPrintDeploy.ps1 -AzureTenant <Azure Active Directory domain name> -AzureTenantGuid <Azure Active Directory ID>`

    - Consulte a captura de tela abaixo para localizar o nome de domínio Azure Active Directory.

    ![Servidor de Impressão como obter o nome de domínio do AAD](../media/hybrid-cloud-print/PrintServer-GetAADDomainName.png)

    - Consulte a captura de tela abaixo para localizar a ID de Azure Active Directory.

    ![Implantação de impressão em nuvem Servidor de Impressão](../media/hybrid-cloud-print/PrintServer-GetAADId.png)

    - A saída do script CloudPrintDeploy é parecida com esta:

    ![Implantação de impressão em nuvem Servidor de Impressão](../media/hybrid-cloud-print/PrintServer-CloudPrintDeploy.png)

    - Verifique o arquivo de log para ver se há algum erro:`C:\Program Files\WindowsPowerShell\Modules\PublishCloudPrinter\1.0.0.0\CloudPrintDeploy.log`

4. Execute **RegitEdit** em um prompt de comandos com privilégios elevados. Vá para o computador \ HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows\CurrentVersion\CloudPrint\EnterpriseCloudPrintService.
    - Certifique-se de que AzureAudience está definido como o URI de ID de aplicativo do aplicativo de impressão de nuvem empresarial.
    - Verifique se AzureTenant está definido como o nome de domínio do Azure AD.

    ![Servidor de Impressão chaves do registro ECP](../media/hybrid-cloud-print/PrintServer-RegEdit-ECP.png)

5. Vá para o computador \ HKEY_LOCAL_MACHINE \SOFTWARE\Microsoft\Windows\CurrentVersion\CloudPrint\MopriaDiscoveryService.
    - Certifique-se de que AzureAudience é o URI de ID do aplicativo do aplicativo do serviço de descoberta do Mopria.
    - Certifique-se de que AzureTenant é o nome de domínio do Azure AD.
    - Verifique se a URL é o URI da ID do aplicativo do aplicativo do serviço de descoberta do Mopria.

    ![Servidor de Impressão chaves do registro Mopria](../media/hybrid-cloud-print/PrintServer-RegEdit-Mopria.png)

6. Execute **iisreset** em um prompt de comando do PowerShell de elevação. Isso garantirá que qualquer alteração no registro feita na etapa anterior entrará em vigor.

7. Configure os pontos de extremidade do IIS para dar suporte ao SSL.
    - O certificado SSL pode ser um certificado autoassinado ou um emitido por uma AC (autoridade de certificação) confiável.
    - Se estiver usando um certificado autoassinado, **Verifique se o certificado foi importado para o (s) computador (es) cliente**.
    - Se você registrar seu domínio com um provedor de terceiros, será necessário configurar os pontos de extremidade do IIS com o certificado SSL. Consulte este [guia](https://www.sslsupportdesk.com/microsoft-server-2016-iis-10-10-5-ssl-installation/) para obter detalhes.

8. Instale o pacote SQLite.
   - Abra um prompt elevado do PowerShell.
   - Execute o comando a seguir para baixar os pacotes NuGet System. Data. SQLite.

        `Register-PackageSource -Name nuget.org -ProviderName NuGet -Location https://www.nuget.org/api/v2/ -Trusted -Force`

   - Execute o comando a seguir para instalar os pacotes.

        `Install-Package system.data.sqlite [-requiredversion x.x.x.x] -providername nuget`

   > Observação: é recomendável baixar e instalar a versão mais recente, deixando a opção-requiredversion.

    ![Servidor de Impressão chaves do registro Mopria](../media/hybrid-cloud-print/PrintServer-InstallSQLite.png)

9. Copie as DLLs do SQLite para a pasta bin do MopriaCloudService webapp (C:\inetpub\wwwroot\MopriaCloudService\bin).
    - Crie um arquivo. ps1 contendo o script do PowerShell abaixo.
    - Altere a variável $version para a versão do SQLite instalada na etapa anterior.
    - Execute o arquivo. ps1 em um prompt de comando do PowerShell com privilégios elevados.

    ```powershell
    $source = \Program Files\PackageManagement\NuGet\Packages
    $core = System.Data.SQLite.Core
    $linq = System.Data.SQLite.Linq
    $ef6 = System.Data.SQLite.EF6
    $version = x.x.x.x
    $target = C:\inetpub\wwwroot\MopriaCloudService\bin

    xcopy /y $source\$core.$version\lib\net46\System.Data.SQLite.dll $target\
    xcopy /y $source\$core.$version\build\net46\x86\SQLite.Interop.dll $target\x86\
    xcopy /y $source\$core.$version\build\net46\x64\SQLite.Interop.dll $target\x64\
    xcopy /y $source\$linq.$version\lib\net46\System.Data.SQLite.Linq.dll $target\
    xcopy /y $source\$ef6.$version\lib\net46\System.Data.SQLite.EF6.dll $target\
    ```

10. Atualize o arquivo de c:\inetpub\wwwroot\MopriaCloudService\web.config para incluir o SQLite versão x.x.x. x. x nas seções a seguir `<runtime>/<assemblyBinding>` . Essa é a mesma versão usada na etapa anterior.

    ```xml
    ...
    <dependentAssembly>
    assemblyIdentity name=System.Data.SQLite culture=neutral publicKeyToken=db937bc2d44ff139 /
    <bindingRedirect oldVersion=0.0.0.0-x.x.x.x newVersion=x.x.x.x />
    </dependentAssembly>
    <dependentAssembly>
    <assemblyIdentity name=System.Data.SQLite.Core culture=neutral publicKeyToken=db937bc2d44ff139 />
    <bindingRedirect oldVersion=0.0.0.0-x.x.x.x newVersion=x.x.x.x />
    </dependentAssembly>
    <dependentAssembly>
    <assemblyIdentity name=System.Data.SQLite.EF6 culture=neutral publicKeyToken=db937bc2d44ff139 />
    <bindingRedirect oldVersion=0.0.0.0-x.x.x.x newVersion=x.x.x.x />
    </dependentAssembly>
    <dependentAssembly>
    <assemblyIdentity name=System.Data.SQLite.Linq culture=neutral publicKeyToken=db937bc2d44ff139 />
    <bindingRedirect oldVersion=0.0.0.0-x.x.x.x newVersion=x.x.x.x />
    </dependentAssembly>
    ...
    ```

11. Crie o banco de dados SQLite.
    - Baixe e instale os binários das ferramentas do SQLite de `https://www.sqlite.org/` .
    - Vá para o `c:\inetpub\wwwroot\MopriaCloudService\Database` diretório.
    - Execute o seguinte comando para criar o banco de dados neste diretório:

        `sqlite3.exe MopriaDeviceDb.db .read MopriaSQLiteDb.sql`

    - No explorador de arquivos, abra as propriedades do arquivo MopriaDeviceDb. DB para adicionar usuários ou grupos que têm permissão para publicar no banco de dados Mopria na guia segurança. Os usuários ou grupos devem existir no local Active Directory e sincronizados com o Azure AD.
    - Se a solução for implantada em um domínio não roteável (por exemplo, *mydomain*. local), o domínio do Azure AD (por exemplo, *DomainName*. onmicrosoft.com, ou uma comprada do fornecedor de terceiros) precisará ser adicionado como um sufixo UPN ao Active Directory local. Isso é assim, exatamente o mesmo usuário que publicará impressoras (por exemplo, admin@*nome_do_domínio*. onmicrosoft.com) pode ser adicionado na configuração de segurança do arquivo de banco de dados. Consulte [preparar um domínio não roteável para sincronização de diretório](https://docs.microsoft.com/office365/enterprise/prepare-a-non-routable-domain-for-directory-synchronization).

    ![Servidor de Impressão chaves do registro Mopria](../media/hybrid-cloud-print/PrintServer-SQLiteDB.png)

### <a name="step-5-optional---configure-pre-authentication-with-azure-ad"></a>Etapa 5 \[ opcional \] – configurar pré-autenticação com o Azure AD

1. Examine o documento [delegação restrita de Kerberos para logon único em seus aplicativos com o proxy de aplicativo](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-configure-single-sign-on-with-kcd).

2. Configure o Active Directory local.
    - Na máquina Active Directory, abra Gerenciador do servidor e vá para **ferramentas**  >  **Active Directory usuários e computadores**.
    - Navegue até o nó **computadores** e selecione o servidor do conector.
    - Clique com o botão direito **Properties**do mouse e selecione a  ->  guia**delegação**de propriedades   .
    - Selecione **confiar neste computador para delegação apenas aos serviços especificados**.
    - Selecione **usar qualquer protocolo de autenticação**.
    - Em **serviços aos quais essa conta pode apresentar credenciais delegadas**.
        - Adicione o SPN (nome da entidade de serviço) da máquina Servidor de Impressão.
        - Selecione HOST para o tipo de serviço.
    ![Delegação de Active Directory](../media/hybrid-cloud-print/AD-Delegation.png)

3. Verifique se a autenticação do Windows está habilitada no IIS.
    - No Servidor de Impressão, abra Gerenciador do Servidor ferramentas de > > Gerenciador do serviço de informações da Internet (IIS).
    - Navegue até o site.
    - Clique duas vezes em **autenticação**.
    - Clique em **autenticação do Windows** e clique em **habilitar** em **ações**.
    ![Servidor de Impressão autenticação IIS](../media/hybrid-cloud-print/PrintServer-IIS-Authentication.png)

4. Configure o logon único.
    - Em portal do Azure, acesse **Azure Active Directory**  >  **aplicativos empresariais**  >  **todos os aplicativos**.
    - Selecione aplicativo MopriaDiscoveryService.
    - Vá para **proxy de aplicativo**. Altere o método de pré-autenticação para **Azure Active Directory**.
    - Acesse **Logon único**. Selecione autenticação integrada do Windows como o método de logon único.
    - Defina o **SPN do aplicativo interno** para o SPN do computador servidor de impressão.
    - Defina a **identidade de logon delegada** para o nome principal do usuário.
    - Repita para o aplicativo EntperiseCloudPrint.
    ![IWA de logon único do AAD](../media/hybrid-cloud-print/AAD-SingleSignOn-IWA.png)

### <a name="step-6---configure-the-required-mdm-policies"></a>Etapa 6 – configurar as políticas de MDM necessárias

1. Faça logon no seu provedor de MDM.
2. Localize o grupo de políticas de impressão em nuvem corporativa e configure as políticas seguindo as diretrizes abaixo:
    - CloudPrintOAuthAuthority = `https://login.microsoftonline.com/<Azure AD Directory ID>` . A ID do diretório pode ser encontrada em Propriedades do Azure Active Directory >.
    - CloudPrintOAuthClientId = \( \) valor da ID do cliente de aplicativo do aplicativo nativo. Você pode encontrá-lo em Azure Active Directory > Registros de aplicativo > selecione o aplicativo nativo > visão geral.
    - CloudPrinterDiscoveryEndPoint = URL externa do aplicativo de serviço de descoberta do Mopria. Você pode encontrá-lo em Azure Active Directory > aplicativos empresariais > selecionar o aplicativo serviço de descoberta do Mopria > proxy de aplicativo. **Ele deve ser exatamente o mesmo, mas sem a direita/**.
    - MopriaDiscoveryResourceId = o URI da ID do aplicativo do aplicativo do serviço de descoberta do Mopria. Você pode encontrá-lo em Azure Active Directory > Registros de aplicativo > selecione o aplicativo serviço de descoberta do Mopria > visão geral. **Ele deve ser exatamente o mesmo com o à direita/**.
    - CloudPrintResourceId = o URI de ID de aplicativo do aplicativo de impressão em nuvem empresarial. Você pode encontrá-lo em Azure Active Directory > Registros de aplicativo > selecione o aplicativo de impressão em nuvem empresarial > visão geral. **Ele deve ser exatamente o mesmo com o à direita/**.
    - DiscoveryMaxPrinterLimit = \<a positive integer\> .

> [!NOTE]
> Se você estiver usando o serviço Microsoft Intune, poderá encontrar essas configurações na categoria impressora de nuvem.

|Nome de exibição do Intune                     |Política                         |
|----------------------------------------|-------------------------------|
|URL de descoberta de impressora                   |CloudPrinterDiscoveryEndpoint  |
|URL de autoridade de acesso à impressora            |CloudPrintOAuthAuthority       |
|GUID do aplicativo cliente nativo do Azure            |CloudPrintOAuthClientId        |
|URI de recurso do serviço de impressão              |CloudPrintResourceId           |
|Máximo de impressoras a serem consultadas (somente dispositivos móveis)  |DiscoveryMaxPrinterLimit       |
|URI de recurso do serviço de descoberta de impressora  |MopriaDiscoveryResourceId      |

> [!NOTE]
> Se o grupo de política de impressão em nuvem não estiver disponível, mas o provedor de MDM oferecer suporte a configurações de OMA-URI, você poderá definir as mesmas políticas.  Consulte [isso](https://docs.microsoft.com/windows/client-management/mdm/policy-csp-enterprisecloudprint#enterprisecloudprint-cloudprintoauthauthority) para obter informações adicionais.

- Valores para OMA-URI
  - CloudPrintOAuthAuthority =./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintOAuthAuthority
    - Valor =`https://login.microsoftonline.com/<Azure AD Directory ID>`
  - CloudPrintOAuthClientId =./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintOAuthClientId
    - Valor =`<Azure AD Native App's Application ID>`
  - CloudPrinterDiscoveryEndPoint =./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrinterDiscoveryEndPoint
    - Valor = URL externa do aplicativo do serviço de descoberta do Mopria (deve ser exatamente o mesmo, mas sem a direita `/` )
  - MopriaDiscoveryResourceId =./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/MopriaDiscoveryResourceId
    - Valor = o URI da ID do aplicativo do aplicativo do serviço de descoberta do Mopria
  - CloudPrintResourceId =./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintResourceId
    - Valor = o URI de ID de aplicativo do aplicativo de impressão em nuvem empresarial
  - DiscoveryMaxPrinterLimit =./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/DiscoveryMaxPrinterLimit
    - Valor = um inteiro positivo

### <a name="step-7---publish-the-shared-printer"></a>Etapa 7 – publicar a impressora compartilhada

1. Instale a impressora desejada no Servidor de Impressão.
2. Compartilhe a impressora por meio da interface do usuário Propriedades da impressora.
3. Selecione o conjunto de usuários desejado para conceder acesso.
4. Salve as alterações e feche a janela Propriedades da impressora.
5. Prepare uma atualização do criador de outono do Windows 10 ou uma máquina posterior. Ingresse o computador no Azure AD e faça logon como um usuário que está sincronizado com o Active Directory local e tenha recebido a permissão adequada para o arquivo MopriaDeviceDb. DB.
6. No computador com Windows 10, abra um prompt de comando elevado do Windows PowerShell.
    - Execute os seguintes comandos.
        - `find-module -Name PublishCloudPrinter`para confirmar que o computador pode alcançar o Galeria do PowerShell (PSGallery)
        - `install-module -Name PublishCloudPrinter`

            > Observação: você pode ver uma mensagem informando que ' PSGallery ' é um repositório não confiável.  Digite ' y ' para continuar com a instalação.

        - `Publish-CloudPrinter`
        - Impressora = o nome da impressora compartilhada. Esse nome deve corresponder exatamente ao nome do compartilhamento mostrado na guia **compartilhamento** de propriedades da impressora, aberto na servidor de impressão.

        ![Servidor de Impressão Propriedades da impressora](../media/hybrid-cloud-print/PrintServer-PrinterProperties.png)

        - Fabricante = fabricante da impressora.
        - Model = modelo de impressora.
        - OrgLocation = uma cadeia de caracteres JSON especificando o local da impressora, por exemplo,

            `{attrs: [{category:country, vs:USA, depth:0}, {category:organization, vs:Microsoft, depth:1}, {category:site, vs:Redmond, WA, depth:2}, {category:building, vs:Building 1, depth:3}, {category:floor_number, vs:1, depth:4}, {category:room_name, vs:1111, depth:5}]}`

        - SDDL = cadeia de caracteres SDDL que representa permissões para a impressora.
            - Faça logon no Servidor de Impressão como administrador e, em seguida, execute o seguinte comando do PowerShell na impressora que você deseja publicar: `(Get-Printer PrinterName -full).PermissionSDDL` .
            - Adicione **O:BA** como prefixo ao resultado do comando acima. Por ex.: se a cadeia de caracteres retornada pelo comando anterior for G:DUD: (A; OICI; FA;;; WD), em seguida, SDDL = O:BAG: DUD: (A; OICI; FA;;; WD).
        - DiscoveryEndpoint = faça logon no portal do Azure e, em seguida, obtenha a cadeia de caracteres de aplicativos empresariais > aplicativo do serviço de descoberta do Mopria > proxy de aplicativo > URL externa. Omita a direita/.
        - PrintServerEndpoint = faça logon no portal do Azure e, em seguida, obtenha a cadeia de caracteres de aplicativos empresariais > aplicativo de impressão de nuvem empresarial > proxy de aplicativo > URL externa. Omita a direita/.
        - AzureClientId = ID de aplicativo do aplicativo nativo registrado.
        - AzureTenantGuid = ID de diretório do seu locatário do Azure AD.
        - DiscoveryResourceId = URI de ID de aplicativo do aplicativo de serviço de descoberta do Mopria.

    - Você também pode inserir todos os valores de parâmetro necessários na linha de comando. A sintaxe do é:

        `Publish-CloudPrinter -Printer <string> -Manufacturer <string> -Model <string> -OrgLocation <string> -Sddl <string> -DiscoveryEndpoint <string> -PrintServerEndpoint <string> -AzureClientId <string> -AzureTenantGuid <string> -DiscoveryResourceId <string>`

        Comando de exemplo:

        `Publish-CloudPrinter -Printer HcpTestPrinter -Manufacturer Manufacturer1 -Model Model1 -OrgLocation '{attrs: [{category:country, vs:USA, depth:0}, {category:organization, vs:MyCompany, depth:1}, {category:site, vs:MyCity, State, depth:2}, {category:building, vs:Building 1, depth:3}, {category:floor_name, vs:1, depth:4}, {category:room_name, vs:1111, depth:5}]}' -Sddl O:BAG:DUD:(A;OICI;FA;;;WD) -DiscoveryEndpoint https://mopriadiscoveryservice-contoso.msappproxy.net/mcs -PrintServerEndpoint https://enterprisecloudprint-contoso.msappproxy.net/ecp -AzureClientId dbe4feeb-cb69-40fc-91aa-73272f6d8fe1 -AzureTenantGuid 8de6a14a-5a23-4c1c-9ae4-1481ce356034 -DiscoveryResourceId https://mopriadiscoveryservice-contoso.msappproxy.net/mcs/`

    - Use o comando a seguir para verificar se a impressora foi publicada.

        `Publish-CloudPrinter -Query -DiscoveryEndpoint <string> -AzureClientId <string> -AzureTenantGuid <string> -DiscoveryResourceId <string>`

        Comando de exemplo:

        `Publish-CloudPrinter -Query -DiscoveryEndpoint https://mopriadiscoveryservice-contoso.msappproxy.net/mcs -AzureClientId dbe4feeb-cb69-40fc-91aa-73272f6d8fe1 -AzureTenantGuid 8de6a14a-5a23-4c1c-9ae4-1481ce356034 -DiscoveryResourceId https://mopriadiscoveryservice-contoso.msappproxy.net/mcs/`

## <a name="verify-the-deployment"></a>Verificar a implantação

Em um dispositivo ingressado no Azure AD que tem as políticas de MDM configuradas:
- Abra um navegador da Web e vá para https://mopriadiscoveryservice- *Tenant-name*. msappproxy.net/MCS/Services.
- Você deve ver o texto JSON que descreve o conjunto de funcionalidades deste ponto de extremidade.
- Vá para **configurações**  >  **dispositivos**  >  **impressoras & scanners**.
    - Clique em **Adicionar impressora ou scanner**.
    - Você deve ver uma pesquisa por impressoras de nuvem (ou Pesquisar impressoras em minha organização em um link mais recente do Windows 10).
    - Clique no link.
    - Clique no link Selecionar um local de pesquisa.
        - Você deve ver a hierarquia de localização do dispositivo.
    - Escolha um local e clique em **OK** e, em seguida, clique no botão **Pesquisar** para localizar as impressoras.
    - Selecione impressora e clique no botão **Adicionar dispositivo** .
    - Após a instalação bem-sucedida da impressora, imprima na impressora do seu aplicativo favorito.

> Observação: se estiver usando a impressora EcpPrintTest, você poderá encontrar o arquivo de saída na máquina Servidor de Impressão em C: \\ ECPTestOutput \\ EcpTestPrint. XPS local.

## <a name="troubleshooting"></a>Solução de problemas

Veja abaixo problemas comuns durante a implantação HCP

|Erro |Etapas recomendadas |
|------|------|
|Falha no script do PowerShell do CloudPrintDeploy | <ul><li>Verifique se o Windows Server tem a atualização mais recente.</li><li>Se Windows Server Update Services (WSUS) for usado, consulte [como disponibilizar recursos sob demanda e pacotes de idiomas quando você estiver usando o WSUS/SCCM](https://docs.microsoft.com/windows/deployment/update/fod-and-lang-packs).</li></ul> |
|Falha na instalação do SQLite com a mensagem: loop de dependência detectado para o pacote ' System. Data. SQLite ' | Install-Package System. Data. sqlite. Core-ProviderName NuGet-SkipDependencies<br>Install-Package System. Data. sqlite. EF6-ProviderName NuGet-SkipDependencies<br>Install-Package System. Data. sqlite. Linq-ProviderName NuGet-SkipDependencies<br><br>Depois que os pacotes tiverem sido baixados com êxito, verifique se eles são da mesma versão. Caso contrário, adicione o parâmetro-requiredversion aos comandos acima e defina-os para que sejam da mesma versão. |
|Falha ao publicar impressora | <ul><li>Para pré-autenticação de passagem, verifique se o usuário que está publicando a impressora recebeu a permissão adequada para o banco de dados de publicação.</li><li>Para a pré-autenticação do Azure AD, verifique se a autenticação do Windows está habilitada no IIS. Consulte a etapa 5,3. Além disso, tente a pré-autenticação de passagem primeiro. Se a pré-autenticação de passagem funcionar, o problema provavelmente está relacionado ao proxy de aplicativo. Consulte [solucionar problemas de proxy de aplicativo e mensagens de erro](https://docs.microsoft.com/azure/active-directory/manage-apps/application-proxy-troubleshoot). Observe que mudar para passagem redefine a configuração de logon único; Reveja a etapa 5 para configurar a pré-autenticação do Azure AD novamente.</li></ul> |
|Os trabalhos de impressão permanecem no estado enviado para a impressora | <ul><li>Verifique se o TLS 1,2 está habilitado no servidor do conector. Consulte o artigo vinculado na etapa 2,1.</li><li>Verifique se o HTTP2 está desabilitado no servidor do conector. Consulte o artigo vinculado na etapa 2,1.</li></ul> |

Abaixo estão locais de logs que podem ajudar a solucionar problemas

|Componente |Local do log |
|------|------|
|Cliente do Windows 10 | <ul><li>Use Visualizador de Eventos para ver o log das operações do Azure AD. Clique em **Iniciar** e digite Visualizador de eventos. Navegue até logs de aplicativos e serviços > Microsoft > Windows > a operação > AAD.</li><li>Use o Hub de comentários para coletar logs. Consulte [enviar comentários para a Microsoft com o aplicativo de Hub de comentários](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub-app)</li></ul> |
|Servidor do conector | Use Visualizador de Eventos para ver o log do proxy de aplicativo. Clique em **Iniciar** e digite Visualizador de eventos. Navegue até aplicativos e serviços logs > Microsoft > AadApplicationProxy > Connector > admin. |
|Servidor de Impressão | Os logs do aplicativo do serviço de descoberta do Mopria e do aplicativo de impressão em nuvem empresarial podem ser encontrados em C:\inetpub\logs\LogFiles\W3SVC1. |
