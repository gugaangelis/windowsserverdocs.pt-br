---
ms.assetid: 35de490f-c506-4b73-840c-b239b72decc2
title: Configurar acesso condicional com base em dispositivo no local
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/11/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: fa96fbeed1445b1add2e5de3aad45ad369a6cafa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847217"
---
# <a name="configure-on-premises-conditional-access-using-registered-devices"></a>Configurar locais acesso condicional usando dispositivos registrados

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2  

O documento a seguir orientará você durante a instalação e configuração de acesso condicional no local com dispositivos registrados.

![Acesso condicional](media/Using-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  

## <a name="infrastructure-pre-requisites"></a>Pré-requisitos de infraestrutura
Os seguintes pré-requisitos são necessários antes de começar com acesso condicional no local. 

|Requisito|Descrição
|-----|-----
|Uma assinatura do Azure AD com o Azure AD Premium | Para habilitar a gravação de dispositivo no acesso condicional do local - [serve uma avaliação gratuita](https://azure.microsoft.com/trial/get-started-active-directory/)  
|Assinatura do Intune|necessário apenas para integração com o MDM para cenários de conformidade do dispositivo -[serve uma avaliação gratuita](https://portal.office.com/Signup/Signup.aspx?OfferId=40BE278A-DFD1-470a-9EF7-9F2596EA7FF9&dl=INTUNE_A&ali=1#0)
|Azure AD Connect|QFE de novembro de 2015 ou posterior.  Obter a versão mais recente [aqui](https://www.microsoft.com/en-us/download/details.aspx?id=47594).  
|Windows Server 2016|Build 10586 ou mais recente para o AD FS  
|Esquema do Active Directory do Windows Server 2016|Nível de esquema 85 ou superior é necessária.
|Controlador de domínio do Windows Server 2016|Isso só é necessário para implantações de confiança de chave Hello For Business.  Informações adicionais podem ser encontradas em [aqui](https://aka.ms/whfbdocs).  
|Cliente do Windows 10|Build 10586 ou mais recente, ingressado no domínio acima é necessária para o ingresso no domínio do Windows 10 e o Microsoft Passport para cenários de trabalho apenas  
|Conta de usuário do Azure AD com o Azure AD Premium licença atribuída|Para registrar o dispositivo  


 
## <a name="upgrade-your-active-directory-schema"></a>Atualizar o esquema do Active Directory
Para usar o acesso condicional no local com dispositivos registrados, você deve primeiramente atualizar a esquema do AD.  As seguintes condições devem ser atendidas:
    - O esquema deve ser a versão 85 ou posterior
    - Isso só é necessária para a floresta do AD FS associado a

> [!NOTE]
> Se você instalou o Azure AD Connect antes de atualizar para a versão do esquema (nível 85 ou superior) no Windows Server 2016, você precisará executar novamente a instalação do Azure AD Connect e o local de atualização para garantir que a regra de sincronização para o esquema do AD msDS-KeyCredentialLink está configurado.

### <a name="verify-your-schema-level"></a>Verifique se o seu nível de esquema
Para verificar seu nível de esquema, faça o seguinte:

1.  Você pode usar ADSIEdit ou LDP e conecte-se para o contexto de nomenclatura do esquema.  
2.  Uso do ADSIEdit, clique duas vezes em "CN = Schema, CN = Configuration, DC =<domain>, DC =<com> e selecione Propriedades.  Domínio Relpace e as partes com suas informações de floresta.
3.  Em Editor de atributo localize o atributo objectVersion e ele lhe dirá, sua versão.  

![ADSI Edit](media/Configure-Device-Based-Conditional-Access-on-Premises/adsiedit.png)  

Você também pode usar o seguinte cmdlet do PowerShell (substitua o objeto com seu esquema de informações de contexto de nomenclatura):

``` powershell
Get-ADObject "cn=schema,cn=configuration,dc=domain,dc=local" -Property objectVersion
    
```

![PowerShell](media/Configure-Device-Based-Conditional-Access-on-Premises/pshell1.png) 

Para obter informações adicionais sobre como atualizar, consulte [atualizar controladores de domínio para o Windows Server 2016](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2016.md). 

## <a name="enable-azure-ad-device-registration"></a>Habilitar o registro de dispositivo do AD do Azure  
Para configurar esse cenário, você deve configurar a funcionalidade de registro do dispositivo no Azure AD.  

Para fazer isso, siga as etapas descritas em [Configurando a junção do Azure AD em sua organização](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-setup/)  

## <a name="setup-ad-fs"></a>Instalação do AD FS  
1. Criar a um [novo farm do AD FS 2016](https://technet.microsoft.com/library/dn486775.aspx).   
2.  Ou [migrar](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md) um farm de AD FS 2016 do AD FS 2012 R2  
4. Implante [do Azure AD Connect](https://azure.microsoft.com/documentation/articles/active-directory-aadconnectfed-whatis/) usando o caminho personalizado para conectar-se o AD FS para o Azure AD.  

## <a name="configure-device-write-back-and-device-authentication"></a>Configurar novamente a gravação de dispositivo e a autenticação de dispositivo  
> [!NOTE]
> Se você executou o Azure AD Connect usando configurações expressas, os objetos corretos do AD tem sido criados para você.  No entanto, na maioria dos cenários do AD FS, Azure AD Connect foi executado com configurações personalizadas para configurar o AD FS, as etapas a seguir são necessárias.  

### <a name="create-ad-objects-for-ad-fs-device-authentication"></a>Criar objetos do AD para a autenticação de dispositivo do AD FS  
Se o farm do AD FS já não estiver configurado para Autenticação de dispositivo (você pode ver isso no console de Gerenciamento do AD FS em Serviço -> Registro de dispositivo), siga estas etapas para criar a configuração e os objetos corretos do AD DS.  

![Registro do dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device1.png)

>Observação: Os comandos abaixo exigem as ferramentas de administração do Active Directory, por isso, se o servidor de federação também não for um controlador de domínio, instale as ferramentas usando a etapa 1 abaixo.  Caso contrário, você pode ignorar a etapa 1.  

1.  Execute o assistente de **Adicionar funções e recursos** e selecione recurso **Ferramentas de administração de servidor remoto** -> **Ferramentas de administração de função** -> **Ferramentas do AD DS e AD LDS** -> Escolha o **Módulo do Active Directory para Windows PowerShell** e as **Ferramentas do AD DS**.

![Registro do dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device2.png)
  
2.  No servidor primário do AD FS, verifique se você está conectado como usuário do AD DS com privilégios de administrador Enterprise (EA) e abra um prompt do powershell com privilégios elevados.  Em seguida, execute os seguintes comandos do PowerShell:  
    
    `Import-module activedirectory`  
    `PS C:\> Initialize-ADDeviceRegistration -ServiceAccountName "<your service account>" ` 
3.  Na janela pop-up pressione Sim.

>Observação: Se o serviço do AD FS está configurado para usar uma conta GMSA, insira o nome da conta no formato "domain\accountname$"

![Registro do dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device3.png)  

O PSH acima cria os seguintes objetos:  


- O contêiner RegisteredDevices na partição do domínio do AD  
- Contêiner de Serviço de registro do dispositivo e o objeto em Configuração--> Serviços --> Configuração do registro de dispositivo  
- Contêiner DKM de serviço de registro do dispositivo e o objeto em Configuração --> Serviços --> Configuração do registro de dispositivo  

![Registro do dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device4.png)  

4.  Depois disso, você verá uma mensagem de conclusão bem-sucedida.

![Registro do dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device5.png) 

###        <a name="create-service-connection-point-scp-in-ad"></a>Criar ponto de Conexão de serviço (SCP) no AD  
Se você pretende usar o ingresso em domínio no Windows 10 (com o registro automático no Azure AD) como descrito aqui, execute os seguintes comandos para criar um ponto de conexão de serviço no AD DS  
1.  Abra o Windows PowerShell e execute o seguinte:
    
    `PS C:>Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1" ` 

>Observação: se necessário, copie o arquivo AdSyncPrep.psm1 do seu servidor do Azure AD Connect.  O arquivo está localizado em Arquivos de Programas\Microsoft Azure Active Directory Connect\AdPrep

![Registro do dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device6.png)   

2. Forneça suas credenciais de administrador global do Azure AD  

    `PS C:>$aadAdminCred = Get-Credential`

![Registro do dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device7.png) 

3.  Execute o seguinte comando do PowerShell 

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


- objeto do tipo msDS-DeviceRegistrationServiceContainer em CN = Serviços de registro de dispositivo, CN = Device Registration Configuration, CN = Services, CN = Configuration, DC = & ltdomain >  


- objeto do tipo msDS-DeviceRegistrationService no contêiner acima  

### <a name="see-it-work"></a>Ver como isso funciona  
Para avaliar as políticas e novas declarações, primeiro registre um dispositivo.  Por exemplo, é possível que um computador com Windows 10 usando o aplicativo de configurações em um sistema -> sobre o ingresso no Azure AD, ou você pode configurar o ingresso no domínio Windows 10 com o registro automático de dispositivo seguindo as etapas adicionais [aqui](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/).  Para obter informações sobre como ingressar o Windows 10 dispositivos móveis, consulte o documento [aqui](https://technet.microsoft.com/itpro/windows/manage/join-windows-10-mobile-to-azure-active-directory).  

Para uma avaliação mais fácil, faça logon no AD FS usando um aplicativo de teste que mostra uma lista de declarações. Você será capaz de ver novas declarações incluindo isManaged, isCompliant e trusttype.  Se você habilitar o Microsoft Passport para o trabalho, você também verá o prt de declaração.  
 

## <a name="configure-additional-scenarios"></a>Configurar cenários adicionais  
### <a name="automatic-registration-for-windows-10-domain-joined-computers"></a>Computadores automática de registro para Windows 10 ingressado no domínio  
Para habilitar o registro automático de domínio do Windows 10 em computadores ingressados, siga as etapas 1 e 2 [aqui](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/).   
Isso ajudará você a obter o seguinte:  

1. Verifique se o ponto de conexão de serviço no AD DS existe e tem as permissões apropriadas (criamos este objeto acima, mas não custa verificar duas vezes).  
2. Verifique se o que AD FS está configurado corretamente  
3. Verifique se que seu sistema de AD FS tem os pontos de extremidade corretos habilitados e configuradas de regras de declaração   
4. Definir as configurações de política de grupo necessárias para o registro automático de computadores ingressados no domínio   

### <a name="microsoft-passport-for-work"></a>Microsoft Passport for Work   
Para obter informações sobre como habilitar o Windows 10 com o Microsoft Passport for Work, consulte [habilitar o Microsoft Passport for Work em sua organização.](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)  

### <a name="automatic-mdm-enrollment"></a>Registro automático do MDM   
Para habilitar o registro automático do MDM de dispositivos registrados para que você possa usar a declaração isCompliant em sua política de controle de acesso, siga as etapas [aqui.](https://blogs.technet.microsoft.com/ad/2015/08/14/windows-10-azure-ad-and-microsoft-intune-automatic-mdm-enrollment-powered-by-the-cloud/)  

## <a name="troubleshooting"></a>Solução de problemas  
1.  Se você receber um erro `Initialize-ADDeviceRegistration` que reclama sobre um objeto já existente no estado incorreto, tal como "o objeto de serviço do drs foi encontrado sem todos os atributos necessários", você pode ter executado comandos do powershell do Azure AD Connect anteriormente e ter uma configuração parcial no AD DS.  Tente excluir manualmente os objetos sob **CN = Device Registration Configuration, CN = Services, CN = Configuration, DC =&lt;domínio&gt;**  e tentar novamente.  
2.  Para o domínio do Windows 10 ingressados clientes  
    1. Para verificar se a autenticação de dispositivo que está funcionando, faça logon no cliente ingressado no domínio como uma conta de usuário de teste. Para disparar o provisionamento rapidamente, bloquear e desbloquear a área de trabalho pelo menos uma vez.   
    2. Instruções para verificar se há stk credencial de chave contêm um link no objeto do AD DS (sincronização ainda tem de executar duas vezes?)  
3.  Se você receber um erro ao tentar registrar um computador Windows que o dispositivo já foi registrado, mas não for possível ou já ter cancelar o registro de dispositivo, você pode ter um fragmento de configuração de registro do dispositivo no registro.  Para investigar e removê-lo, use as seguintes etapas:  
    1. No computador Windows, abra o Regedit e navegue até **HKLM\Software\Microsoft\Enrollments**   
    2. Sob essa chave, haverá muitas subchaves no formato GUID.  Navegue até a subchave que tem valores de ~ 17 nele e tem "EnrollmentType" de "6" [MDM Unido] ou "13" (ingressado no Azure AD)  
    3. Modifique **EnrollmentType** para **0** 
    4. Tente o registro de dispositivo ou o registro novamente  

### <a name="related-articles"></a>Artigos relacionados  
* [Proteger o acesso ao Office 365 e outros aplicativos conectados ao Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access/)  
* [Políticas de dispositivo de acesso condicional para serviços do Office 365](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access-device-policies/)  
* [Configurando o acesso condicional no local, usando o registro de dispositivo do Active Directory do Azure](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup)  
* [Conectar dispositivos ingressados no domínio ao Azure AD para experiências com Windows 10](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/)  
