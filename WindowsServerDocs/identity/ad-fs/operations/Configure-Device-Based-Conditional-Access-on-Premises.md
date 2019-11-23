---
ms.assetid: 35de490f-c506-4b73-840c-b239b72decc2
title: Configurar acesso condicional com base em dispositivo no local
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/11/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: a7646144b591fd7327f881cb54489201140e9287
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358152"
---
# <a name="configure-on-premises-conditional-access-using-registered-devices"></a>Configurar o acesso condicional local usando dispositivos registrados


O documento a seguir irá orientá-lo na instalação e configuração do acesso condicional local com dispositivos registrados.

![acesso condicional](media/Using-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  

## <a name="infrastructure-pre-requisites"></a>Pré-requisitos de infraestrutura
Os seguintes pré-requisitos são necessários para que você possa começar com o acesso condicional local. 

|Requisito|Descrição
|-----|-----
|Uma assinatura do Azure AD com o Azure AD Premium | Para habilitar o Write-back do dispositivo para acesso condicional local- [uma avaliação gratuita está boa](https://azure.microsoft.com/trial/get-started-active-directory/)  
|Assinatura do Intune|necessário apenas para a integração do MDM para cenários de conformidade do dispositivo-[uma avaliação gratuita está correta](https://portal.office.com/Signup/Signup.aspx?OfferId=40BE278A-DFD1-470a-9EF7-9F2596EA7FF9&dl=INTUNE_A&ali=1#0)
|Azure AD Connect|2015 QFE de novembro ou posterior.  Obtenha a versão mais recente [aqui](https://www.microsoft.com/en-us/download/details.aspx?id=47594).  
|Windows Server 2016|Build 10586 ou mais recente para AD FS  
|Esquema de Active Directory do Windows Server 2016|O nível de esquema 85 ou superior é necessário.
|Controlador de domínio do Windows Server 2016|Isso só é necessário para implantações de confiança de chave de Hello para empresas.  Informações adicionais podem ser encontradas [aqui](https://aka.ms/whfbdocs).  
|Cliente do Windows 10|A compilação 10586 ou mais recente, unida ao domínio acima, é necessária somente para cenários de ingresso no domínio e Microsoft Passport for Work do Windows 10  
|Conta de usuário do Azure AD com licença Azure AD Premium atribuída|Para registrar o dispositivo  


 
## <a name="upgrade-your-active-directory-schema"></a>Atualizar seu esquema de Active Directory
Para usar o acesso condicional local com dispositivos registrados, você deve primeiro atualizar o esquema do AD.  As seguintes condições devem ser atendidas:
    - O esquema deve ser a versão 85 ou posterior
    - Isso só é necessário para a floresta à qual AD FS está associado

> [!NOTE]
> Se você instalou o Azure AD Connect antes de atualizar para a versão do esquema (nível 85 ou superior) no Windows Server 2016, será necessário executar novamente a instalação do Azure AD Connect e atualizar o esquema do AD local para garantir a regra de sincronização para o msDS-KeyCredentialLink está configurado.

### <a name="verify-your-schema-level"></a>Verificar seu nível de esquema
Para verificar seu nível de esquema, faça o seguinte:

1.  Você pode usar o ADSIEdit ou o LDP e conectar-se ao contexto de nomenclatura do esquema.  
2.  Usando o ADSIEdit, clique com o botão direito do mouse em "CN = Schema, CN = Configuration, DC =<domain>, DC =<com> e selecione Propriedades.  Domínio relpace e as partes com com suas informações de floresta.
3.  No editor de atributos, localize o atributo objectVersion e ele dirá sua versão.  

![ADSI Edit](media/Configure-Device-Based-Conditional-Access-on-Premises/adsiedit.png)  

Você também pode usar o seguinte cmdlet do PowerShell (substitua o objeto com suas informações de contexto de nomenclatura de esquema):

``` powershell
Get-ADObject "cn=schema,cn=configuration,dc=domain,dc=local" -Property objectVersion
    
```

![PowerShell](media/Configure-Device-Based-Conditional-Access-on-Premises/pshell1.png) 

Para obter informações adicionais sobre a atualização, consulte [Atualizar controladores de domínio para o Windows Server 2016](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2016.md). 

## <a name="enable-azure-ad-device-registration"></a>Habilitar Registro de Dispositivos do Azure AD  
Para configurar esse cenário, você deve configurar o recurso de registro de dispositivo no Azure AD.  

Para fazer isso, siga as etapas em [Configurando o ingresso no Azure AD em sua organização](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-setup/)  

## <a name="setup-ad-fs"></a>AD FS de instalação  
1. Crie um [novo farm AD FS 2016](https://technet.microsoft.com/library/dn486775.aspx).   
2.  Ou [migre](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md) um farm para AD FS 2016 do AD FS 2012 R2  
4. Implante [Azure ad Connect](https://azure.microsoft.com/documentation/articles/active-directory-aadconnectfed-whatis/) usando o caminho personalizado para conectar AD FS ao Azure AD.  

## <a name="configure-device-write-back-and-device-authentication"></a>Configurar write-back de dispositivo e autenticação de dispositivo  
> [!NOTE]
> Se você executou Azure AD Connect usando as configurações expressas, os objetos do AD corretos foram criados para você.  No entanto, na maioria dos cenários de AD FS, Azure AD Connect foi executado com configurações personalizadas para configurar AD FS, para que as etapas a seguir sejam necessárias.  

### <a name="create-ad-objects-for-ad-fs-device-authentication"></a>Criar objetos do AD para a autenticação de dispositivo do AD FS  
Se o farm do AD FS já não estiver configurado para Autenticação de dispositivo (você pode ver isso no console de Gerenciamento do AD FS em Serviço -> Registro de dispositivo), siga estas etapas para criar a configuração e os objetos corretos do AD DS.  

![Registro do dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device1.png)

>Observação: os comandos a seguir exigem ferramentas de administração de Active Directory, portanto, se o servidor de Federação não for também um controlador de domínio, primeiro instale as ferramentas usando a etapa 1 abaixo.  Caso contrário, você pode ignorar a etapa 1.  

1.  Execute o assistente de **Adicionar funções e recursos** e selecione recurso **Ferramentas de administração de servidor remoto** -> **Ferramentas de administração de função** -> **Ferramentas do AD DS e AD LDS** -> Escolha o **Módulo do Active Directory para Windows PowerShell** e as **Ferramentas do AD DS**.

![Registro do dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device2.png)
  
2. Em seu AD FS servidor primário, verifique se você está conectado como AD DS usuário com os privilégios de EA (administrador corporativo) e abra um prompt do PowerShell com privilégios elevados.  Em seguida, execute os seguintes comandos do PowerShell:  
    
   `Import-module activedirectory`  
   `PS C:\> Initialize-ADDeviceRegistration -ServiceAccountName "<your service account>" ` 
3. Na janela pop-up, clique em Sim.

>Observação: se o serviço de AD FS estiver configurado para usar uma conta do GMSA, insira o nome da conta no formato "DOMAIN\ACCOUNTNAME $"

![Registro do dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device3.png)  

O PSH acima cria os seguintes objetos:  


- O contêiner RegisteredDevices na partição do domínio do AD  
- Contêiner de Serviço de registro do dispositivo e o objeto em Configuração--> Serviços --> Configuração do registro de dispositivo  
- Contêiner DKM de serviço de registro do dispositivo e o objeto em Configuração --> Serviços --> Configuração do registro de dispositivo  

![Registro do dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device4.png)  

4. Depois disso, você verá uma mensagem de conclusão bem-sucedida.

![Registro do dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device5.png) 

###        <a name="create-service-connection-point-scp-in-ad"></a>Criar ponto de conexão de serviço (SCP) no AD  
Se você pretende usar o ingresso em domínio no Windows 10 (com o registro automático no Azure AD) como descrito aqui, execute os seguintes comandos para criar um ponto de conexão de serviço no AD DS  
1.  Abra o Windows PowerShell e execute o seguinte:
    
    `PS C:>Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1" ` 

>Observação: se necessário, copie o arquivo AdSyncPrep. psm1 de seu servidor de Azure AD Connect.  O arquivo está localizado em Arquivos de Programas\Microsoft Azure Active Directory Connect\AdPrep

![Registro do dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device6.png)   

2. Forneça suas credenciais de administrador global do Azure AD  

    `PS C:>$aadAdminCred = Get-Credential`

![Registro do dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device7.png) 

3. Execute o seguinte comando do PowerShell 

   `PS C:>Initialize-ADSyncDomainJoinedComputerSync -AdConnectorAccount [AD connector account name] -AzureADCredentials $aadAdminCred ` 

Onde o [nome da conta do conector do AD] é o nome da conta configurada no Azure AD Connect ao adicionar o diretório do AD DS local.
  
Os comandos acima permitem que os clientes do Windows 10 encontrem o domínio correto do Azure AD para ingressar ao criar o objeto serviceConnectionpoint no AD DS.  

### <a name="prepare-ad-for-device-write-back"></a>Preparar AD para write-back de dispositivo   
Para garantir que os objetos e contêineres do AD DS estejam no estado correto para o write-back de dispositivos do Azure AD, faça o seguinte.

1.  Abra o Windows PowerShell e execute o seguinte:  

    `PS C:>Initialize-ADSyncDeviceWriteBack -DomainName <AD DS domain name> -AdConnectorAccount [AD connector account name] ` 

Onde o [nome da conta do conector do AD] é o nome da conta configurada no Azure AD Connect ao adicionar o diretório do AD DS local no formato domínio\nome da conta.  

O comando acima cria os seguintes objetos para write-back no AD DS, se eles não existir e permite o acesso ao nome da conta do conector do AD especificado  

- O contêiner RegisteredDevices na partição do domínio do AD  
- Contêiner de Serviço de registro do dispositivo e o objeto em Configuração--> Serviços --> Configuração do registro de dispositivo  

### <a name="enable-device-write-back-in-azure-ad-connect"></a>Habilitar o write-back de dispositivo no Azure AD Connect  
Se você não tiver feito assim antes, habilite a gravação de dispositivo no Azure AD Connect executando o assistente uma segunda vez e selecionando **"Personalizar opções de sincronização"** e, em seguida, marque a caixa de write-back do dispositivo e selecione a floresta em que você executou os cmdlets acima  

### <a name="configure-device-authentication-in-ad-fs"></a>Configurar a autenticação de dispositivo no AD FS  
Usando uma janela de comando do PowerShell com privilégios elevados, configure a política do AD FS executando o seguinte comando  

`PS C:>Set-AdfsGlobalAuthenticationPolicy -DeviceAuthenticationEnabled $true -DeviceAuthenticationMethod All` 

### <a name="check-your-configuration"></a>Verificar a configuração  
Para referência, veja a seguir uma lista abrangente de dispositivos, contêineres e permissões do AD DS necessárias para write-back do dispositivo e trabalho de autenticação
 


- objeto do tipo ms-DS-DeviceContainer em CN=RegisteredDevices, DC=&lt;domínio&gt;        
    - acesso de leitura à conta de serviço do AD FS   
    - acesso de leitura/gravação para a sincronização da conta de conector do AD ao Azure AD Connect</br></br>

- Contêiner CN=Configuração de registro de dispositivo,CN=Serviços,CN=Configuração, DC=&lt;domínio&gt;.  
- DKM de serviço de registro de dispositivo do contêiner sob o contêiner acima

![Registro do dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device8.png) 
 


- objeto do tipo serviceConnectionpoint em CN =&lt;guid&gt;, CN=Registro de dispositivo

- Configuração,CN=Serviços,CN=Configuration,DC=&lt;domínio&gt;  
  - acesso de leitura/gravação para o nome da conta de conector do AD especificado no novo objeto</br></br> 


- objeto do tipo msDS-DeviceRegistrationServiceContainer em CN = Serviços de registro de dispositivo, CN = Configuração de registro de dispositivo, CN = Services, CN = Configuration, DC = & ltdomain >  


- objeto do tipo msDS-DeviceRegistrationService no contêiner acima  

### <a name="see-it-work"></a>Veja o trabalho de ti  
Para avaliar as novas declarações e políticas, primeiro registre um dispositivo.  Por exemplo, você pode ingressar no Azure AD em um computador com Windows 10 usando o aplicativo de configurações em System-> sobre, ou você pode configurar o ingresso no domínio do Windows 10 com o registro de dispositivo automático seguindo as etapas adicionais [aqui](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/).  Para obter informações sobre como ingressar dispositivos Windows 10 Mobile, consulte o documento [aqui](https://technet.microsoft.com/itpro/windows/manage/join-windows-10-mobile-to-azure-active-directory).  

Para uma avaliação mais fácil, faça logon para AD FS usando um aplicativo de teste que mostra uma lista de declarações. Você poderá ver novas declarações, incluindo isgerencied, IsCompliant e TrustType.  Se você habilitar Microsoft Passport para o trabalho, você também verá a declaração de PRT.  
 

## <a name="configure-additional-scenarios"></a>Configurar cenários adicionais  
### <a name="automatic-registration-for-windows-10-domain-joined-computers"></a>Registro automático para computadores ingressados no domínio do Windows 10  
Para habilitar o registro de dispositivo automático para computadores ingressados no domínio do Windows 10, siga as etapas 1 e 2 [aqui](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/).   
Isso irá ajudá-lo a obter o seguinte:  

1. Verifique se o ponto de conexão de serviço no AD DS existe e se tem as permissões corretas (criamos este objeto acima, mas isso não prejudica a verificação dupla).  
2. Verifique se AD FS está configurado corretamente  
3. Verifique se o sistema de AD FS tem os pontos de extremidade corretos habilitados e as regras de declaração configuradas   
4. Definir as configurações de política de grupo necessárias para o registro automático de dispositivo de computadores ingressados no domínio   

### <a name="microsoft-passport-for-work"></a>Microsoft Passport for Work   
Para obter informações sobre como habilitar o Windows 10 com o Microsoft Passport for Work, consulte [habilitar Microsoft Passport for Work em sua organização.](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)  

### <a name="automatic-mdm-enrollment"></a>Registro automático do MDM   
Para habilitar o registro automático de MDM de dispositivos registrados para que você possa usar a declaração IsCompliant em sua política de controle de acesso, siga as etapas [aqui.](https://blogs.technet.microsoft.com/ad/2015/08/14/windows-10-azure-ad-and-microsoft-intune-automatic-mdm-enrollment-powered-by-the-cloud/)  

## <a name="troubleshooting"></a>Solução de problemas  
1.  Se você receber um erro em `Initialize-ADDeviceRegistration` que reclama sobre um objeto já existente no estado incorreto, como "o objeto do serviço DRS foi encontrado sem todos os atributos necessários", você pode ter executado Azure AD Connect comandos do PowerShell anteriormente e ter uma configuração parcial no AD DS.  Tente excluir manualmente os objetos em **CN = Configuração de registro de dispositivo, CN = Services, CN = Configuration, DC =&lt;domínio&gt;** e tente novamente.  
2.  Para clientes ingressados no domínio do Windows 10  
    1. Para verificar se a autenticação do dispositivo está funcionando, entre no cliente ingressado no domínio como uma conta de usuário de teste. Para disparar o provisionamento rapidamente, bloqueie e desbloqueie a área de trabalho pelo menos uma vez.   
    2. Instruções para verificar o link de credenciais de chave STK no objeto AD DS (a sincronização ainda precisa ser executada duas vezes?)  
3.  Se você receber um erro ao tentar registrar um computador Windows em que o dispositivo já estava registrado, mas você não tiver ou não registrado o registro do dispositivo, você poderá ter um fragmento da configuração de registro do dispositivo no registro.  Para investigar e remover isso, use as seguintes etapas:  
    1. No computador com Windows, abra regedit e navegue até **HKLM\Software\Microsoft\Enrollments**   
    2. Nessa chave, haverá muitas subchaves no formulário GUID.  Navegue até a subchave que tem aproximadamente 17 valores e tem "Enrollmentid" de "6" [ingressado no MDM] ou "13" (ingressado no Azure AD)  
    3. Modificar o **registrotype** como **0** 
    4. Experimente o registro ou registro do dispositivo novamente  

### <a name="related-articles"></a>Artigos relacionados  
* [Protegendo o acesso ao Office 365 e a outros aplicativos conectados ao Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access/)  
* [Políticas de dispositivo de acesso condicional para serviços do Office 365](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access-device-policies/)  
* [Configurando o acesso condicional local usando Registro de Dispositivos do Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup)  
* [Conectar dispositivos ingressados no domínio ao Azure AD para experiências do Windows 10](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/)  
