---
title: Implantar o Windows Server Hybrid Cloud Print - autenticação de passagem
description: Como configurar a impressão da nuvem híbrida Microsoft com autenticação de passagem
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
ms.openlocfilehash: 95cced8dc06cc4ee3768addf65e17cf379e20454
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66435756"
---
# <a name="deploy-windows-server-hybrid-cloud-print-with-passthrough-authentication"></a>Implantar o Windows Server Hybrid Cloud Print com autenticação de passagem

>Aplica-se a: Windows Server 2016

Este tópico, para os administradores de TI, descreve a implantação de ponta a ponta da solução Microsoft Hybrid Cloud Print. Camadas essa solução na parte superior de servidores existentes do Windows em execução como servidor de impressão e permite que Azure Active Directory associado e gerenciados por MDM, os dispositivos para descobrir e imprimir a organização gerenciados impressoras.

## <a name="pre-requisites"></a>Pré-requisitos

Há um número de assinaturas, serviços e computadores, que você precisará adquirir antes de iniciar esta instalação. Elas são as seguintes:

-   Assinatura do Azure AD premium
    
    Ver [começar com uma assinatura do Azure](https://azure.microsoft.com/trial/get-started-active-directory/), para uma assinatura de avaliação do Azure. 

-   Serviço MDM, como o Intune
    
    Ver [Microsoft Intune](https://www.microsoft.com/en-us/cloud-platform/microsoft-intune), para uma assinatura de avaliação do Intune.

-   Windows Server em execução como o Active Directory

    Consulte [passo a passo: Configurando o Active Directory no Windows Server 2016](https://blogs.technet.microsoft.com/canitpro/2017/02/22/step-by-step-setting-up-active-directory-in-windows-server-2016/), para obter ajuda sobre como configurar o Active Directory.

-   Executando como servidor de impressão do Windows Server 2016 ingressados no domínio
    
    Ver [instalar funções, serviços de função e recursos usando o Assistente de recursos e adicionar funções](https://docs.microsoft.com/windows-server/administration/server-manager/install-or-uninstall-roles-role-services-or-features#BKMK_installarfw), para obter informações sobre como instalar funções e serviços de função no Windows Server.

-   Azure AD Connect
    
    Ver [instalação personalizada do Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-get-started-custom), para obter ajuda sobre como configurar o Azure AD Connect.

-   Conector de Proxy de aplicativo do Azure em um domínio separado Unido máquina Windows Server
    
    Ver [Introdução ao Proxy de aplicativo e instalar o conector](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable), para obter ajuda sobre como configurar o conector de Proxy de aplicativo do Azure.

-   Nome de domínio voltado para o público
    
    Você pode usar o nome de domínio criado para você pelo Azure ou comprar seu próprio nome de domínio.

## <a name="deployment-steps"></a>Etapas de implantação

Este guia descreve as etapas de instalação cinco (5):

- Etapa 1: Instalar o Azure AD Connect para sincronizar entre o Azure AD e AD local
- Etapa 2: Instalar o pacote de impressão de nuvem híbrida no servidor de impressão
- Etapa 3: Instalar o Proxy de aplicativo do Azure (AAP) com autenticação de passagem
- Etapa 4: Configurar as políticas MDM necessárias
- Etapa 5: Publicar impressoras compartilhadas

### <a name="step-1---install-azure-ad-connect-to-sync-between-azure-ad-and-on-premises-ad"></a>Etapa 1: instalar o Azure AD Connect para sincronizar entre o Azure AD e AD local
1. No computador do Windows Server Active Directory, baixe o software do Azure AD Connect
2. Inicie o pacote de instalação do Azure AD Connect e selecione **usar configurações expressas**
3. Insira as informações solicitadas no processo de instalação e clique em **instalar**

### <a name="step-2---install-hybrid-cloud-print-package-on-the-print-server"></a>Etapa 2 - pacote de instalação do Hybrid Cloud Print no servidor de impressão

1. Instalar os módulos do PowerShell de impressão de nuvem híbrida
   - Execute os seguintes comandos em um prompt de comando com privilégios elevados do PowerShell
      - `find-module -Name "PublishCloudPrinter"` para confirmar que o computador possa acessar a Galeria do PowerShell (PSGallery)
      - `install-module -Name "PublishCloudPrinter"`

     > OBSERVAÇÃO: Você poderá ver uma mensagem declarando que 'PSGallery' é um repositório não confiável.  Digite 'y' para continuar com a instalação.

2. Instalar a solução de impressão de nuvem híbrida
    - O prompt de comando do PowerShell com privilégios elevados mesmo, altere o diretório para `C:\Program Files\WindowsPowerShell\Modules\PublishCloudPrinter\1.0.0.0`
    - Run <br>
        `CloudPrintDeploy.ps1 -AzureTenant <Domain name used by Azure AD Connect> -AzureTenantGuid <Azure AD Directory ID>`
3. Configurar os pontos de 2 extremidade do IIS para dar suporte a SSL
   -   O certificado SSL pode ser um certificado autoassinado ou um emitido de alguma autoridade de certificação confiável (CA)
   -  Se usando um certificado autoassinado, verifique se que o certificado é importado para os computadores cliente
4. Instalar o pacote SQLite
   - Abra um prompt de comando com privilégios elevados do PowerShell
   - Execute o seguinte comando para baixar os pacotes do nuget System.Data.SQLite <br>
       `Register-PackageSource -Name nuget.org -ProviderName NuGet -Location https://www.nuget.org/api/v2/ -Trusted -Force`
   - Execute o seguinte comando para instalar os pacotes<br>
   `Install-Package system.data.sqlite [-requiredversion x.x.x.x] -providername nuget`

   > OBSERVAÇÃO: É recomendável baixar e instalar a versão mais recente, omitindo o "-requiredversion" opção.

5. Copiar as dlls do SQLite para a MopriaCloudService Webapp \<bin\> pasta (**c:\\inetpub\\wwwroot\\MopriaCloudService\\bin**): <br>
   - Os binários do SQLite devem estar na "\\Program Files\\PackageManagement\\NuGet\\pacotes"

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

   > Observação: x.x.x. x é a versão do SQLite instalada acima.

6. Atualizar o `c:\inetpub\wwwroot\MopriaCloudService\web.config` arquivo para incluir a versão do SQLite x.x.x. x na seguinte \<tempo de execução\>/\<assemblyBinding\> seções:

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
    -  Baixar e instalar os binários SQLite ferramentas de <https://www.sqlite.org/>
    -  Vá para **c:\\inetpub\\wwwroot\\MopriaCloudService\\banco de dados** diretório
    -  Execute o seguinte comando para criar o banco de dados neste diretório:  `sqlite3.exe MopriaDeviceDb.db ".read MopriaSQLiteDb.sql"`
    -  No Explorador de arquivos, abra as propriedades de arquivo MopriaDeviceDb.db para adicionar usuários/grupos que têm permissão para publicar banco de dados do Mopria na guia Segurança
        - Recomendável apenas adicionar o grupo de usuário de administrador necessário.
8. Registrar o aplicativo 2 web com o Azure AD para dar suporte à autenticação de OAuth2
   - Faça logon como o Administrador Global para o portal de gerenciamento de locatário do AD do Azure
     1. Acesse a guia "Registros do aplicativo" Adicionar ponto de extremidade de impressão
        - Adicionar o aplicativo, selecione "Novo registro de aplicativo"
        - Forneça um nome apropriado e selecione "aplicativo Web / API"
        - URL de logon = "<http://MicrosoftEnterpriseCloudPrint/CloudPrint>"
     2. Repita para o ponto de extremidade de descoberta
        - URL de logon = "<http://MopriaDiscoveryService/CloudPrint>"
     3. Repita para o aplicativo cliente nativo
        -   Ao fornecer o nome do aplicativo, certifique-se de que selecionar o "Aplicativo de cliente nativo" como "tipo de aplicativo"
        -   Redirect URI = "https://\<services-machine-endpoint\>/RedirectUrl"
     4. Ir para o aplicativo cliente nativo "Configurações"
        -   Copie o valor de "ID do aplicativo" a ser usada para etapas posteriores do programa de instalação
        -   Selecione "Permissões necessárias"
            1.  Clique em "Adicionar"
            2.  Clique em "Selecionar uma API"
            3.  Procurar o ponto de extremidade de impressão e o ponto de extremidade de descoberta pelo nome que você definiu ao criar o ponto de extremidade do aplicativo
            4.  Adicione os pontos de 2 extremidade
            5.  Verifique se que a opção de "Permissões delegadas" para cada ponto de extremidade do aplicativo está habilitada
            6.  Verifique se que você clicar no botão "Done" na parte inferior
            7.  Clique em "Conceder permissões", após a adição de ambos os pontos de extremidade.  Selecione "Sim" quando for solicitado a aprovar a solicitação.
        -   Vá para "REDIRECIONAR URIS" e adicione o seguinte URIs de redirecionamento à lista: `ms-appx-web://Microsoft.AAD.BrokerPlugin/\<NativeClientAppID\>`
            `ms-appx-web://Microsoft.AAD.BrokerPlugin/S-1-15-2-3784861210-599250757-1266852909-3189164077-45880155-1246692841-283550366`

### <a name="step-3---install-azure-application-proxy-aap-with-passthrough-authentication"></a>Etapa 3: instalar o Proxy de aplicativo do Azure (AAP) com autenticação de passagem
1. Faça logon em seu portal de gerenciamento de locatário do AAD (Azure AD)
    - Na lista de menus do AAD, selecione "Proxy de aplicativo"
    - Clique em "Habilitar proxy de aplicativo" na parte superior da tela
    - Baixe o "conector de Proxy de aplicativo" para uma máquina Windows Server ingressado no domínio que atuará como Proxy de aplicativo Web (WAP).
2. No computador WAP, faça logon como administrador e instale o "conector de Proxy de aplicativo"
    - Durante a instalação, dê o conector de proxy de aplicativo as credenciais para sua filosofia do Azure que você deseja habilitar AAP em
    - Verifique se que o computador WAP é integrado ao Active Directory local ao domínio
3. Volte para o portal de gerenciamento de locatário do AAD e adicione os proxies de aplicativo
   - Vá para o **aplicativos empresariais** guia
   - Clique em **novo aplicativo**
   - Selecione **aplicativo local** e preencha os campos
       - Nome: Qualquer nome desejado
       - URL interna: Esta é a URL interna para o serviço de nuvem de descoberta do Mopria que pode acessar a sua máquina WAP
       - URL externa: Configurar conforme apropriado para sua organização
       - Método de pré-autenticação: Passagem

     >   Observação: Se você não encontrar todas as configurações acima, adicione o proxy com as configurações disponíveis e, em seguida, selecione o proxy de aplicativo que você acabou de criar e vá para o **proxy de aplicativo** guia e adicionar todas as informações acima.

4. Repita 3, acima, para o serviço de impressão do Enterprise Cloud e forneça a URL interna ao serviço de impressão de nuvem empresarial

    >   Observação: O https://&lt;serviços de máquina-ponto de extremidade &gt; /mcs URL mencionada a seguir deve ser a URL externa que você configurar para seu serviço de nuvem do Mopria e/ou o serviço de impressão de nuvem empresarial.


### <a name="step-4---configure-the-required-mdm-policies"></a>Etapa 4: configurar as diretivas necessárias do MDM
- Faça logon no seu provedor de MDM
- Localize o grupo de políticas do Enterprise Cloud Print e configurar as políticas a seguir as diretrizes abaixo:
  - CloudPrintOAuthAuthority = https://login.microsoftonline.com/\<Azure AD Directory ID\>
  - CloudPrintOAuthClientId = valor "ID do aplicativo" do aplicativo nativo da Web que você registrou no portal de gerenciamento do Azure AD
  - CloudPrinterDiscoveryEndPoint = URL externa do Mopria descoberta de serviço do Azure do Proxy de aplicativo criado na etapa 3.3 (deve ser exatamente o mesmo, mas à direita /)
  - MopriaDiscoveryResourceId = "URI de ID do aplicativo" do aplicativo Web / API para o ponto de extremidade de descoberta registrados na etapa 2.8.  Você pode encontrá-lo nas configurações -> Propriedades do aplicativo
  - CloudPrintResourceId = "URI de ID do aplicativo" do aplicativo Web / API para o ponto de extremidade de impressão é registrado na etapa 2.8. Você pode encontrá-lo nas configurações -> Propriedades do aplicativo
  - DiscoveryMaxPrinterLimit = \<a positive integer\>

>   Observação: Se você estiver usando o serviço Microsoft Intune, você pode encontrar essas configurações na categoria "Nuvem Printer".

|Nome de exibição do Intune                     |Política                         |
|----------------------------------------|-------------------------------|
|URL de descoberta de impressora                   |CloudPrinterDiscoveryEndpoint  |
|URL de autoridade de acesso de impressora            |CloudPrintOAuthAuthority       |
|GUID do aplicativo cliente nativo do Azure            |CloudPrintOAuthClientId        |
|URI de recurso do serviço de impressão              |CloudPrintResourceId           |
|Máximo de impressoras para consulta (somente móvel)  |DiscoveryMaxPrinterLimit       |
|URI de recurso da serviço de descoberta de impressora  |MopriaDiscoveryResourceId      |


>   Observação: Se o grupo de política de impressão de nuvem não está disponível, mas o provedor de MDM dá suporte a configurações OMA-URI, em seguida, você pode definir as mesmas políticas.  Consulte este <a href="https://docs.microsoft.com/windows/client-management/mdm/policy-csp-enterprisecloudprint#enterprisecloudprint-cloudprintoauthauthority">artigo</a> para obter informações adicionais.

- OMA-URI
    - `CloudPrintOAuthAuthority = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintOAuthAuthority`
        - Valor = `https://login.microsoftonline.com/` \<ID de diretório do Azure AD\>
    - `CloudPrintOAuthClientId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintOAuthClientId`
        - Valor = \<ID do aplicativo do aplicativo nativo do Azure AD\>
    - `CloudPrinterDiscoveryEndPoint = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrinterDiscoveryEndPoint`
        - Valor = URL externa do Mopria descoberta de serviço do Azure do Proxy de aplicativo criado na etapa 3.3 (deve ser exatamente o mesmo, mas à direita /)
    - `MopriaDiscoveryResourceId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/MopriaDiscoveryResourceId`
        - Valor = "URI de ID do aplicativo" do aplicativo Web / API para o ponto de extremidade de descoberta registrados na etapa 2.8.  Você pode encontrá-lo nas configurações -> Propriedades do aplicativo
    - `CloudPrintResourceId = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/CloudPrintResourceId`
        - Valor = "URI de ID do aplicativo" do aplicativo Web / API para o ponto de extremidade de impressão é registrado na etapa 2.8. Você pode encontrá-lo nas configurações -> Propriedades do aplicativo
    - `DiscoveryMaxPrinterLimit = ./Vendor/MSFT/Policy/Config/EnterpriseCloudPrint/DiscoveryMaxPrinterLimit`
        - Valor = \<um número inteiro positivo\>
  
### <a name="step-5---publish-desired-shared-printers"></a>Etapa 5: publicar impressoras compartilhadas desejadas
1. Instalar a impressora desejada no servidor de impressão
2. Compartilhar a impressora por meio da UI de propriedades da impressora
3. Selecione o conjunto desejado de usuários para conceder acesso
4. Salve as alterações e fechar a janela de propriedades da impressora
5. Em um computador de atualização do criador de outono do Windows 10, abra um prompt de comando com privilégios elevados do Windows PowerShell
   1. Execute os seguintes comandos
      - `find-module -Name "PublishCloudPrinter"` para confirmar que o computador possa acessar a Galeria do PowerShell (PSGallery)
      - `install-module -Name "PublishCloudPrinter"`

        >   OBSERVAÇÃO: Você poderá ver uma mensagem declarando que 'PSGallery' é um repositório não confiável.  Digite 'y' para continuar com a instalação.

      - Publish-CloudPrinter
        - Impressora = nome da impressora compartilhada que foi definido
        - Fabricante = fabricante da impressora
        - Modelo = modelo de impressora
        - OrgLocation = JSON de uma cadeia de caracteres especificando o local da impressora, por exemplo:   `{"attrs": [{"category":"country", "vs":"USA", "depth":0}, {"category":"organization", "vs":"Microsoft", "depth":1}, {"category":"site", "vs":"Redmond, WA", "depth":2}, {"category":"building", "vs":"Building 1", "depth":3}, {"category":"floor\_number", "vn":1, "depth":4}, {"category":"room\_name", "vs":"1111", "depth":5}]}`
        - SDDL = cadeia de caracteres SDDL que representa as permissões para a impressora. Isso pode ser obtido, modificando a guia de segurança de propriedades da impressora adequadamente e, em seguida, executando o seguinte comando em um prompt de comando: `(Get-Printer PrinterName -full).PermissionSDDL`
            Por exemplo "G:DUD:(A;OICI;FA;;;WD)"

          > OBSERVAÇÃO: Você precisará adicionar **`O:BA`** como prefixo para o resultado do comando do prompt de comando acima antes de definir o valor como a configuração de SDDL.  Exemplo: SDDL = `O:BAG:DUD:(A;OICI;FA;;;WD)`

        - DiscoveryEndpoint = https://&lt;services-machine-endpoint&gt;/mcs
        - PrintServerEndpoint = https://&lt;services-machine-endpoint&gt;/ecp
        - AzureClientId = ID do aplicativo do valor registrado do aplicativo nativo da Web acima
        - AzureTenantGuid = ID de diretório do locatário do Azure AD
        - DiscoveryResourceId = [opcional] ID de aplicativo do serviço de nuvem de descoberta do Mopria com proxy

        > OBSERVAÇÃO: Você pode inserir todos os valores de parâmetro necessário na linha de comando.<br>
        **Publicar-CloudPrinter** sintaxe de comando do PowerShell: <br>
        CloudPrinter publicar-impressora \<cadeia de caracteres\> -fabricante \<cadeia de caracteres\> -modelo \<cadeia de caracteres\> - OrgLocation \<cadeia de caracteres\> - Sddl \<cadeia de caracteres\> - DiscoveryEndpoint \<cadeia de caracteres\> - PrintServerEndpoint \<cadeia de caracteres\> - AzureClientId \<cadeia de caracteres\> - AzureTenantGuid \<cadeia de caracteres\> [-DiscoveryResourceId \<cadeia de caracteres\>] <br>
        Comando de exemplo: `publish-cloudprinter -Printer EcpPrintTest -Manufacturer Microsoft -Model FilePrinterEcp -OrgLocation '{"attrs": [{"category":"country", "vs":"USA", "depth":0}, {"category":"organization", "vs":"MyCompany", "depth":1}, {"category":"site", "vs":"MyCity, State", "depth":2}, {"category":"building", "vs":"Building 1", "depth":3}, {"category":"floor\_number", "vn":1, "depth":4}, {"category":"room\_name", "vs":"1111", "depth":5}]}' -Sddl "O:BAG:DUD:(A;OICI;FA;;;WD)" -DiscoveryEndpoint https://<services-machine-endpoint>/mcs -PrintServerEndpoint https://<services-machine-endpoint>/ecp -AzureClientId <Native Web App ID> -AzureTenantGuid <Azure AD Directory ID> -DiscoveryResourceId <Proxied Mopria Discovery Cloud Service App ID>`


## <a name="verifing-the-deployment"></a>Verificando a implantação
Em um dispositivo de ingressado no Azure AD que tenha configuradas as políticas de MDM:
- Abra um navegador da web e ir para https://&lt;serviços de máquina-ponto de extremidade &gt; /mcs/serviços
- Você deve ver o texto JSON que descreve o conjunto de funcionalidades do ponto de extremidade
- Vá para "configurações do sistema operacional -\> dispositivos –\> impressoras e scanners"
    - Você verá um link de "Pesquisar por impressoras em nuvem"
    - Clique no link
    - Clique no link "Selecione um local de pesquisa"
        - Você deve ver a hierarquia de local do dispositivo
    - Escolha um local e clique em **Okey** e, em seguida, clique em **pesquisa** botão para localizar as impressoras
    - Selecione a impressora e clique em **adicionar dispositivo** botão
    - Após a instalação bem-sucedida de impressora, imprimir na impressora do seu aplicativo favorito

>   Observação: Se usando a impressora "EcpPrintTest", você pode encontrar o arquivo de saída na máquina do servidor de impressão, sob "c:\\ECPTestOutput\\EcpTestPrint.xps" local.