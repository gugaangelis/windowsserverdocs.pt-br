---
title: Implantar impressão em nuvem híbrida do Windows Server
description: Como configurar a impressão em nuvem híbrida da Microsoft
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: Windows Server 2016
ms.tgt_pltfrm: na
ms.topic: ''
ms.assetid: fc239aec-e719-47ea-92fc-d82a7247c5e9
author: msjimwu
ms.author: coreyp
manager: dongill
ms.date: 3/15/2018
ms.openlocfilehash: 552695626c98ee0fc01148536b50d4466d1b96e4
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70866804"
---
# <a name="deploy-windows-server-hybrid-cloud-print-with-pre-authentication"></a>Implantar a impressão de nuvem do Windows Server híbrido com pré-autenticação

>Aplica-se a: Windows Server 2016

Este tópico, para administradores de ti, descreve a implantação de ponta a ponta da solução de impressão de nuvem híbrida da Microsoft. Essas camadas de solução sobre o Windows Server (s) existente em execução como Servidor de Impressão e permitem que os dispositivos Azure Active Directory associados e gerenciados pelo MDM sejam descobertos e impressos em impressoras gerenciadas pela organização.

## <a name="pre-requisites"></a>Pré-requisitos

Há várias assinaturas, serviços e computadores que você precisará adquirir antes de iniciar esta instalação. Elas são as seguintes:

-   Assinatura do Azure AD Premium
    
    Consulte [introdução a uma assinatura do Azure](https://azure.microsoft.com/trial/get-started-active-directory/)para obter uma assinatura de avaliação do Azure. 

-   Serviço MDM, como o Intune
    
    Consulte [Microsoft Intune](https://www.microsoft.com/en-us/cloud-platform/microsoft-intune), para uma assinatura de avaliação do Intune.

-   Windows Server em execução como Active Directory

    Consulte [passo a passo: Configurando o Active Directory no Windows](https://blogs.technet.microsoft.com/canitpro/2017/02/22/step-by-step-setting-up-active-directory-in-windows-server-2016/)Server 2016, para obter ajuda sobre como configurar Active Directory.

-   Windows Server 2016 ingressado no domínio em execução como Servidor de Impressão
    
    Consulte [instalar funções, serviços de função e recursos usando o assistente para adicionar funções e recursos](https://docs.microsoft.com/windows-server/administration/server-manager/install-or-uninstall-roles-role-services-or-features#BKMK_installarfw), para obter informações sobre como instalar funções e serviços de função no Windows Server.

-   Azure AD Connect
    
    Consulte [instalação personalizada do Azure ad Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-custom)para obter ajuda sobre como configurar Azure ad Connect.

-   Aplicativo Azure o conector de proxy em um computador separado do Windows Server ingressado no domínio
    
    Confira [introdução ao proxy de aplicativo e instale o conector](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable)para obter ajuda para configurar o conector de proxy aplicativo Azure.

-   Nome de domínio voltado para o público
    
    Você pode usar o nome de domínio criado para você pelo Azure ou comprar seu próprio nome de domínio.

## <a name="deployment-steps"></a>Etapas de implantação

Este guia descreve cinco (5) etapas de instalação:

- Etapa 1: Instalar Azure AD Connect para sincronização entre o Azure AD e o AD local
- Etapa 2: Instalar o pacote de impressão de nuvem híbrida no Servidor de Impressão
- Etapa 3: Instalar o proxy de Aplicativo Azure (AAP) com a delegação restrita de Kerberos (KCD)
- Etapa 4: Configurar as políticas de MDM necessárias
- Etapa 5: Publicar impressoras compartilhadas

### <a name="step-1---install-azure-ad-connect-to-sync-between-azure-ad-and-on-premises-ad"></a>Etapa 1-instalar o Azure AD Connect para sincronizar entre o Azure AD e o AD local
1. Na máquina Active Directory do Windows Server, baixe o software Azure AD Connect
2. Inicie o pacote de instalação do Azure AD Connect e selecione **usar configurações expressas**
3. Insira as informações solicitadas no processo de instalação e clique em **instalar**

### <a name="step-2---install-hybrid-cloud-print-package-on-the-print-server"></a>Etapa 2-instalar o pacote de impressão de nuvem híbrida no Servidor de Impressão

1. Instalar os módulos do PowerShell de impressão em nuvem híbrida
   - Execute os seguintes comandos em um prompt de comando do PowerShell com privilégios elevados
      - `find-module -Name "PublishCloudPrinter"`para confirmar que o computador pode alcançar o Galeria do PowerShell (PSGallery)
      - `install-module -Name "PublishCloudPrinter"`

     > OBSERVAÇÃO: Você pode ver uma mensagem informando que ' PSGallery ' é um repositório não confiável.  Digite ' y ' para continuar com a instalação.

2. Instalar a solução de impressão de nuvem híbrida
    - No mesmo prompt de comando do PowerShell com privilégios elevados, altere o diretório para`C:\Program Files\WindowsPowerShell\Modules\PublishCloudPrinter\1.0.0.0`
    - Executar <br>
        `CloudPrintDeploy.ps1 -AzureTenant <Domain name used by Azure AD Connect> -AzureTenantGuid <Azure AD Directory ID>`
3. Configurar os 2 pontos de extremidade do IIS para dar suporte ao SSL
   -   O certificado SSL pode ser um certificado autoassinado ou um emitido por alguma AC (autoridade de certificação) confiável
   -  Se estiver usando um certificado autoassinado, verifique se o certificado foi importado para o (s) computador (es) cliente
4. Instalar pacote do SQLite
   - Abrir um prompt de comando do PowerShell com privilégios elevados
   - Execute o comando a seguir para baixar os pacotes NuGet System. Data. SQLite <br>
       `Register-PackageSource -Name nuget.org -ProviderName NuGet -Location https://www.nuget.org/api/v2/ -Trusted -Force`
   - Execute o comando a seguir para instalar os pacotes<br>
   `Install-Package system.data.sqlite [-requiredversion x.x.x.x] -providername nuget`

   > OBSERVAÇÃO: É recomendável baixar e instalar a versão mais recente, deixando a opção "-requiredversion".

5. Copie as DLLs do SQLite para a pasta \<bin\> do MopriaCloudService webapp (**C\\:\\\\Inetpub\\wwwroot MopriaCloudService bin**): <br>
   - Os binários do SQLite devem estar\\em "\\arquivos\\de\\programas" pacotes NuGet do PackageManagement "

           \\System.Data.SQLite.**Core**.x.x.x.x\\lib\\net46\\System.Data.SQLite.dll
           --\> \<bin\>\\System.Data.SQLite.dll  
           \\System.Data.SQLite.**Core**.x.x.x.x\\build\\net46\\x86\\SQLite.Interop.dll
           --\> \<bin\>\\**x86**\\SQLite.Interop.dll  
           \\System.Data.SQLite.**Core**.x.x.x.x\\build\\net46\\x64\\SQLite.Interop.dll
           --\> \<bin\>\\**x64**\\SQLite.Interop.dll
           \\System.Data.SQLite.**Linq**.x.x.x.x\\lib\\net46\\System.Data.SQLite.Linq.dll
           --\> \<bin\>\\System.Data.SQLite.Linq.dll  
           \\System.Data.SQLite.**EF6**.x.x.x.x\\lib\\net46\\System.Data.SQLite.EF6.dll
           --\> \<bin\>\\System.Data.SQLite.EF6.dll

   > Observação: x. x. x. x é a versão do SQLite instalada acima.

6. Atualize o `c:\inetpub\wwwroot\MopriaCloudService\web.config` arquivo para incluir o SQLite versão x.x.x. x. x nas seguintes/\> \<seções assemblyBinding de tempo\>de execução:\<

       <dependentAssembly>
       assemblyIdentity name="System.Data.SQLite" culture="neutral" publicKeyToken="db937bc2d44ff139" /
       <bindingRedirect oldVersion="0.0.0.0-x.x.x.x" newVersion="x.x.x.x" />
       </dependentAssembly>
       <dependentAssembly>
       <assemblyIdentity name="System.Data.SQLite.Core" culture="neutral" publicKeyToken="db937bc2d44ff139" />
       <bindingRedirect oldVersion="0.0.0.0-x.x.x.x" newVersion="x.x.x.x" />
       </dependentAssembly>
       <dependentAssembly>
       <assemblyIdentity name="System.Data.SQLite.EF6" culture="neutral" publicKeyToken="db937bc2d44ff139" />
       <bindingRedirect oldVersion="0.0.0.0-x.x.x.x" newVersion="x.x.x.x" />
       </dependentAssembly>
       <dependentAssembly>
       <assemblyIdentity name="System.Data.SQLite.Linq" culture="neutral" publicKeyToken="db937bc2d44ff139" />
       <bindingRedirect oldVersion="0.0.0.0-x.x.x.x" newVersion="x.x.x.x" />
       </dependentAssembly>

7. Crie o banco de dados SQLite:
    -  Baixe e instale os binários das ferramentas do SQLite de<https://www.sqlite.org/>
    -  Vá para **c:\\Inetpub\\wwwroot\\MopriaCloudService\\** diretório de banco de dados
    -  Execute o seguinte comando para criar o banco de dados neste diretório:`sqlite3.exe MopriaDeviceDb.db ".read MopriaSQLiteDb.sql"`
    -  No explorador de arquivos, abra as propriedades do arquivo MopriaDeviceDb. DB para adicionar usuários/grupos que têm permissão para publicar no banco de dados Mopria na guia Segurança
        - Recomenda-se apenas adicionar o grupo de usuários administrador necessário.
8. Registrar o aplicativo Web 2 com o Azure AD para dar suporte à autenticação OAuth2
   - Faça logon como administrador global no portal de gerenciamento de locatário do Azure AD
     1. Vá para a guia "Registros de aplicativo" para adicionar o ponto de extremidade de impressão
        - Adicionar aplicativo, selecione "novo registro de aplicativo"
        - Forneça um nome apropriado e selecione "aplicativo Web/API"
        - URL de logon = "<http://MicrosoftEnterpriseCloudPrint/CloudPrint>"
     2. Repetir para o ponto de extremidade de descoberta
        - URL de logon = "<http://MopriaDiscoveryService/CloudPrint>"
     3. Repetir para o aplicativo cliente nativo
        -   Ao fornecer o nome do aplicativo, certifique-se de selecionar "aplicativo cliente nativo" como o "tipo de aplicativo"
        -   URI de redirecionamento\<= "https://Services-\>Machine-Endpoint/RedirectUrl"
     4. Vá para o aplicativo cliente nativo "configurações"
        -   Copiar o valor de "ID do aplicativo" a ser usado para etapas de instalação posteriores
        -   Selecione "permissões necessárias"
            1.  Clique em "Adicionar"
            2.  Clique em "selecionar uma API"
            3.  Procure o ponto de extremidade de impressão e o ponto de extremidade de descoberta pelo nome que você definiu ao criar o ponto de extremidade do aplicativo
            4.  Adicionar os dois pontos de extremidade
            5.  Certifique-se de que a opção "permissões delegadas" para cada ponto de extremidade do aplicativo está habilitada
            6.  Certifique-se de clicar no botão "concluído" na parte inferior
            7.  Clique em "conceder permissões", depois que os dois pontos de extremidade tiverem sido adicionados.  Selecione "Sim" quando for solicitado a aprovar a solicitação.
        -   Vá para "redirecionar URIS" e adicione os seguintes URIs de redirecionamento à lista:`ms-appx-web://Microsoft.AAD.BrokerPlugin/\<NativeClientAppID\>`
            `ms-appx-web://Microsoft.AAD.BrokerPlugin/S-1-15-2-3784861210-599250757-1266852909-3189164077-45880155-1246692841-283550366`

### <a name="step-3---install-azure-application-proxy-aap-with-kerberos-constrained-delegation-kcd"></a>Etapa 3-instalar o proxy de Aplicativo Azure (AAP) com a delegação restrita de Kerberos (KCD)
1. Faça logon no portal de gerenciamento de locatário do AAD (Azure AD)
    - Na lista de menus do AAD, selecione "proxy de aplicativo"
    - Clique em "Habilitar proxy de aplicativo" na parte superior da tela
    - Baixe o "conector de proxy de aplicativo" em um computador ingressado no domínio do Windows Server que atuará como o WAP (proxy de aplicativo Web).
2. No computador WAP, faça logon como administrador e instale o "conector de proxy de aplicativo"
    - Durante a instalação, dê ao conector do proxy de aplicativo as credenciais para seu princípio do Azure em que você deseja habilitar AAP
    - Certifique-se de que a máquina WAP esteja ingressada no domínio em seu local Active Directory
3. Na máquina Active Directory, vá para **ferramentas – > usuários e computadores**
    - Selecione o computador que está executando o conector de proxy de aplicativo
    - Clique com o botão direito do mouse e selecione **Propriedades – guia Delegação de >**
    - Selecione **confiar neste computador para delegação apenas aos serviços especificados.**
    - Selecione **usar qualquer protocolo de autenticação.**
    - Em **serviços aos quais essa conta pode apresentar credenciais delegadas**
        - Adicione o valor para a identidade SPN do computador que executa os serviços (MopriaDiscoveryService e MicrosoftEnterpriseCloudPrint Service)
            - Para SPN, insira o SPN do próprio computador, ou seja, "Host/\<MachineName\>.\< Domínio\>"<br>
                `HOST/appServer.Contoso.com`
4. Voltar para o portal de gerenciamento de locatário do AAD e adicionar os proxies de aplicativo
   - Ir para a guia **aplicativos empresariais**
   - Clique em **novo aplicativo**
   - Selecione o **aplicativo local** e preencha os campos
       - Nome: Qualquer nome que você desejar
       - URL interna: Esta é a URL interna para o serviço de nuvem de descoberta do Mopria que seu computador WAP pode acessar
       - URL externa: Configurar conforme apropriado para sua organização
       - Método de pré-autenticação: Active Directory do Azure

     >   Observação: Se você não encontrar todas as configurações acima, adicione o proxy com as configurações disponíveis e, em seguida, selecione o proxy de aplicativo que você acabou de criar e acesse a guia **proxy de aplicativo** e adicione todas as informações acima.

   - Depois de criado, volte para **aplicativos** -> empresariais**todos os aplicativos**, selecione o novo aplicativo que você acabou de criar
   - Vá para **logon único**, verifique se o "modo de logon único" está definido como "autenticação integrada do Windows"
   - Defina o "SPN do aplicativo interno" para o SPN especificado na etapa 3,3, acima
   - Certifique-se de que a "identidade de logon delegada" esteja definida como "nome principal do usuário"

5. Repita 4, acima, para o serviço de impressão de nuvem empresarial e forneça a URL interna para seu serviço de impressão de nuvem empresarial
6. Volte para o portal de gerenciamento de locatário do Azure AD e vá para **registros de aplicativo** e selecione o aplicativo cliente nativo – > "configurações"
    - Selecione **as permissões necessárias**
        - Adicione os dois novos aplicativos proxy que você acabou de criar
        - Conceder permissões delegadas para esses 2 aplicativos
        - Depois que os dois aplicativos proxy tiverem sido adicionados, clique em "conceder permissões".  Selecione "Sim" quando for solicitado a aprovar a solicitação.

7. Habilitar a autenticação do Windows no IIS para o serviço de nuvem Mopria e computadores do serviço de impressão de nuvem empresarial
    - Verifique se o recurso de autenticação do Windows está instalado:
        1. Abrir o Gerenciador do Servidor
        2. Clique em **gerenciar**
        3. Clique em **adicionar funções e recursos**
        4. Selecione **a instalação baseada em função ou recurso**
        5. Selecione o servidor
        6. Em servidor Web (IIS)-> servidor Web-> segurança, selecione **autenticação do Windows**
        7. Clique em Avançar até concluir a instalação
    - Habilitar a autenticação do Windows no IIS:
        1. Abrir Gerenciador do Serviços de Informações da Internet (IIS)
        2. Para cada serviço/site:
            1.  Selecione o serviço/site
            2.  Clique duas vezes em **autenticação**
            3.  Clique em **autenticação do Windows** e clique em **habilitar** em **ações**

### <a name="step-4---configure-the-required-mdm-policies"></a>Etapa 4 – configurar as políticas de MDM necessárias
- Faça logon no seu provedor de MDM
- Localize o grupo de políticas de impressão em nuvem corporativa e configure as políticas seguindo as diretrizes abaixo:
  - CloudPrintOAuthAuthority = https://login.microsoftonline.com/\<Azure ID do diretório do AD\>
  - CloudPrintOAuthClientId = valor "ID do aplicativo" do aplicativo Web nativo que você registrou no portal de gerenciamento do Azure AD
  - CloudPrinterDiscoveryEndPoint = URL externa do serviço de descoberta do Mopria Aplicativo Azure proxy criado na etapa 3,3 (deve ser exatamente o mesmo, mas sem a direita/)
  - MopriaDiscoveryResourceId = URL externa do serviço de descoberta do Mopria Aplicativo Azure proxy criado na etapa 3,4 (deve ser exatamente o mesmo, incluindo o à direita/)
  - CloudPrintResourceId = URL externa do serviço de impressão em nuvem da empresa Aplicativo Azure o proxy criado na etapa 3,5 (deve ser exatamente o mesmo, incluindo o à direita/)
  - DiscoveryMaxPrinterLimit = \<um inteiro positivo\>

>   Observação: Se você estiver usando o serviço Microsoft Intune, poderá encontrar essas configurações na categoria "impressora em nuvem".

|Nome de exibição do Intune                     |Política                         |
|----------------------------------------|-------------------------------|
|URL de descoberta de impressora                   |CloudPrinterDiscoveryEndpoint  |
|URL de autoridade de acesso à impressora            |CloudPrintOAuthAuthority       |
|GUID do aplicativo cliente nativo do Azure            |CloudPrintOAuthClientId        |
|URI de recurso do serviço de impressão              |CloudPrintResourceId           |
|Máximo de impressoras a serem consultadas (somente dispositivos móveis)  |DiscoveryMaxPrinterLimit       |
|URI de recurso do serviço de descoberta de impressora  |MopriaDiscoveryResourceId      |

>   Observação: Se o grupo de política de impressão em nuvem não estiver disponível, mas o provedor de MDM oferecer suporte a configurações de OMA-URI, você poderá definir as mesmas políticas.  Veja este <a href="https://docs.microsoft.com/windows/client-management/mdm/policy-csp-enterprisecloudprint#enterprisecloudprint-cloudprintoauthauthority">artigo</a> para obter informações adicionais.

- OMA-URI
    - `CloudPrintOAuthAuthority = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintOAuthAuthority`
        - Valor = `https://login.microsoftonline.com/` \<ID de diretório do Azure AD\>
    - `CloudPrintOAuthClientId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintOAuthClientId`
        - Valor = \<ID do aplicativo do aplicativo nativo do Azure AD\>
    - `CloudPrinterDiscoveryEndPoint = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrinterDiscoveryEndPoint`
        - Valor = URL externa do serviço de descoberta do Mopria Aplicativo Azure proxy criado na etapa 3,3 (deve ser exatamente o mesmo, mas sem a direita/)
    - `MopriaDiscoveryResourceId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/MopriaDiscoveryResourceId`
        - Valor = URL externa do serviço de descoberta do Mopria Aplicativo Azure proxy criado na etapa 3,4 (deve ser exatamente o mesmo, incluindo o à direita/)
    - `CloudPrintResourceId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintResourceId`
        - Valor = URL externa do serviço de impressão em nuvem da empresa Aplicativo Azure o proxy criado na etapa 3,5 (deve ser exatamente o mesmo, incluindo o à direita/)
    - `DiscoveryMaxPrinterLimit = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/DiscoveryMaxPrinterLimit`
        - Valor = \<um inteiro positivo\>

### <a name="step-5---publish-desired-shared-printers"></a>Etapa 5 – publicar impressoras compartilhadas desejadas
1. Instalar a impressora desejada no Servidor de Impressão
2. Compartilhar a impressora por meio da interface do usuário de propriedades da impressora
3. Selecione o conjunto de usuários desejado para conceder acesso
4. Salvar as alterações e fechar a janela Propriedades da impressora
5. Em um computador de atualização do criador de outono do Windows 10, abra um prompt de comando com privilégios elevados do Windows PowerShell
   1. Execute os seguintes comandos
      - `find-module -Name "PublishCloudPrinter"`para confirmar que o computador pode alcançar o Galeria do PowerShell (PSGallery)
      - `install-module -Name "PublishCloudPrinter"`

        >   OBSERVAÇÃO: Você pode ver uma mensagem informando que ' PSGallery ' é um repositório não confiável.  Digite ' y ' para continuar com a instalação.

      - Publish-CloudPrinter
        - Printer = o nome da impressora compartilhada que foi definido
        - Fabricante = fabricante da impressora
        - Modelo = modelo de impressora
        - OrgLocation = uma cadeia de caracteres JSON especificando o local da impressora, por exemplo:`{"attrs": [{"category":"country", "vs":"USA", "depth":0}, {"category":"organization", "vs":"Microsoft", "depth":1}, {"category":"site", "vs":"Redmond, WA", "depth":2}, {"category":"building", "vs":"Building 1", "depth":3}, {"category":"floor\_number", "vn":1, "depth":4}, {"category":"room\_name", "vs":"1111", "depth":5}]}`
        - SDDL = cadeia de caracteres SDDL que representa permissões para a impressora. Isso pode ser obtido modificando a guia Segurança de propriedades da impressora apropriadamente e, em seguida, executando o seguinte comando em um prompt de comando:`(Get-Printer PrinterName -full).PermissionSDDL`
            P. "G:DUD: (A; OICI; FA;;; WD) "

          > OBSERVAÇÃO: Será necessário adicionar **`O:BA`** como prefixo ao resultado do comando de prompt de comando acima antes de definir o valor como a configuração de SDDL.  Exemplo: SDDL =`O:BAG:DUD:(A;OICI;FA;;;WD)`

        - DiscoveryEndpoint = URL externa do serviço de descoberta do Mopria Aplicativo Azure proxy criado na etapa 3,4
        - PrintServerEndpoint = URL externa do serviço de impressão em nuvem empresarial Aplicativo Azure proxy criado na etapa 3,5
        - AzureClientId = ID do aplicativo do valor do aplicativo Web nativo registrado acima
        - AzureTenantGuid = ID de diretório do seu locatário do Azure AD
        - DiscoveryResourceId = [opcional] ID do aplicativo do serviço de nuvem de descoberta de Mopria com proxy

        > OBSERVAÇÃO: Você também pode inserir todos os valores de parâmetro necessários na linha de comando.<br>
        **Publish-CloudPrinter** Sintaxe de comando do PowerShell: <br>
        Publish-CloudPrinter-Printer \<String\> -\> \<\> fabricante \<cadeiade caracteres \<-modelo cadeia de caracteres-OrgLocation cadeia de caracteres-SDDL\> \< String-DiscoveryEndpoint\<String-PrintServerEndpoint\> String-\> AzureClientId String-\<AzureTenantGuid \<\> \> \<String[\> - DiscoveryResourceId\<String]\> <br>
        Comando de exemplo:`publish-cloudprinter -Printer EcpPrintTest -Manufacturer Microsoft -Model FilePrinterEcp -OrgLocation '{"attrs": [{"category":"country", "vs":"USA", "depth":0}, {"category":"organization", "vs":"MyCompany", "depth":1}, {"category":"site", "vs":"MyCity, State", "depth":2}, {"category":"building", "vs":"Building 1", "depth":3}, {"category":"floor\_number", "vn":1, "depth":4}, {"category":"room\_name", "vs":"1111", "depth":5}]}' -Sddl "O:BAG:DUD:(A;OICI;FA;;;WD)" -DiscoveryEndpoint https://<services-machine-endpoint>/mcs -PrintServerEndpoint https://<services-machine-endpoint>/ecp -AzureClientId <Native Web App ID> -AzureTenantGuid <Azure AD Directory ID> -DiscoveryResourceId <Proxied Mopria Discovery Cloud Service App ID>`


## <a name="verifying-the-deployment"></a>Verificando a implantação
Em um dispositivo ingressado no Azure AD que tem as políticas de MDM configuradas:
- Abra um navegador da Web e vá para https://&lt;Services-Machine-Endpoint&gt;/MCS/Services (a URL externa para o ponto de extremidade de descoberta)
- Você deve ver o texto JSON que descreve o conjunto de funcionalidades deste ponto de extremidade
- Vá para "configurações do sistema\> operacional-\> dispositivos-impressoras & scanners"
    - Você deve ver um link "Pesquisar impressoras de nuvem"
    - Clique no link
    - Clique no link "Selecione um local de pesquisa"
        - Você deve ver a hierarquia de localização do dispositivo
    - Escolha um local e clique em **OK** e, em seguida, clique no botão **Pesquisar** para localizar as impressoras
    - Selecione a impressora e clique no botão **Adicionar dispositivo**
    - Após a instalação bem-sucedida da impressora, imprima na impressora do seu aplicativo favorito

>   Observação: Se estiver usando a impressora "EcpPrintTest", você poderá encontrar o arquivo de saída na máquina servidor de impressão no local "\\C\\: ECPTestOutput EcpTestPrint. XPS".
