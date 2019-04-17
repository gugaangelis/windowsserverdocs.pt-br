---
ms.assetid: 35de490f-c506-4b73-840c-b239b72decc2
title: Configurar acesso condicional baseado no dispositivo local
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/11/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 47afd0c6963bd8b8b4dde82650cf807c1954b40b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="configure-on-premises-conditional-access-using-registered-devices"></a>Configurar local acesso condicional usando dispositivos registrados

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2  

Este documento orientará instalar e configurar o acesso condicional no local com dispositivos registrados.

![Acesso condicional](media/Using-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  

## <a name="infrastructure-pre-requisites"></a>Pré-requisitos de infraestrutura
Os requisitos por seguintes são necessários antes de começar com acesso condicional no local. 

|Requisito|Descrição
|-----|-----
|Uma assinatura do Azure AD com o Azure AD Premium | Para habilitar a gravação de dispositivo voltar para em acesso condicional local - [serve uma avaliação gratuita](https://azure.microsoft.com/en-us/trial/get-started-active-directory/)  
|Assinatura do Intune|Só é necessário para a integração de MDM para cenários de conformidade do dispositivo -[serve uma avaliação gratuita](https://portal.office.com/Signup/Signup.aspx?OfferId=40BE278A-DFD1-470a-9EF7-9F2596EA7FF9&dl=INTUNE_A&ali=1#0)
|Azure AD Connect|QFE de novembro de 2015 ou posterior.  Obtenha a última versão [aqui](https://www.microsoft.com/en-us/download/details.aspx?id=47594).  
|Windows Server 2016|Compilação 10586 ou mais recente do AD FS  
|Esquema do Active Directory do Windows Server 2016|Nível de esquema 85 ou superior é necessária.
|Controlador de domínio do Windows Server 2016|Isso só é necessário para implantações de confiança de chave de Hello para empresas.  Informações adicionais podem ser encontradas em [aqui](https://aka.ms/whfbdocs).  
|Cliente do Windows 10|Compilação 10586 ou mais recente, associado ao domínio acima é necessária para o ingresso no domínio do Windows 10 e o Microsoft Passport para cenários de trabalho somente  
|Conta de usuário do Azure AD com o Azure AD Premium licença atribuída|Para registrar o dispositivo  


 
## <a name="upgrade-your-active-directory-schema"></a>Atualizar seu esquema do Active Directory
Para usar o acesso condicional no local com dispositivos registrados, primeiro você deve atualizar seu esquema do AD.  As seguintes condições devem ser atendidas:
    - O esquema deve ser versão 85 ou posterior
    - Isso só é necessária para floresta do AD FS está associado a

> [!NOTE]
> Se você instalou o Azure AD Connect antes de atualizar para a versão de esquema (nível de 85 ou superior) no Windows Server 2016, você precisará executar novamente a instalação do Azure AD Connect e atualizar o local esquema do AD para garantir que a regra de sincronização para msDS-KeyCredentialLink está configurado.

### <a name="verify-your-schema-level"></a>Verifique se o seu nível de esquema
Para verificar o nível de esquema, faça o seguinte:

1.  Você pode usar ADSIEdit ou LDP e conectar-se para o contexto de nomenclatura do esquema.  
2.  Usando ADSIEdit, clique em "CN = Schema, CN = Configuration, DC =<domain>, DC =<com> e selecione Propriedades.  Domínio Relpace e as partes com suas informações de floresta.
3.  Sob o Editor de atributo localize o atributo objectVersion e ele informará, sua versão.  

![ADSI Edit](media/Configure-Device-Based-Conditional-Access-on-Premises/adsiedit.png)  

Você também pode usar o seguinte cmdlet do PowerShell (substituir o objeto com o esquema de informações de contexto de nomenclatura):

``` powershell
Get-ADObject "cn=schema,cn=configuration,dc=domain,dc=local" -Property objectVersion
    
```

![PowerShell](media/Configure-Device-Based-Conditional-Access-on-Premises/pshell1.png) 

Para obter informações adicionais sobre a atualização, consulte [atualizar controladores de domínio para o Windows Server 2016](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2016.md). 

## <a name="enable-azure-ad-device-registration"></a>Habilitar o registro de dispositivo do Azure AD  
Para configurar esse cenário, você deve configurar a funcionalidade de registro do dispositivo no Azure AD.  

Para fazer isso, siga as etapas em [Configurando ingressar Azure AD em sua organização](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-setup/)  

## <a name="setup-ad-fs"></a>Instalação do AD FS  
1. Criar a um [novo farm AD FS 2016](https://technet.microsoft.com/library/dn486775.aspx).   
2.  Ou [migrar](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md) um farm AD FS 2016 do AD FS 2012 R2  
4. Implantar [Azure AD Connect](https://azure.microsoft.com/documentation/articles/active-directory-aadconnectfed-whatis/) usando o caminho personalizado para conectar o AD FS no Azure AD.  

## <a name="configure-device-write-back-and-device-authentication"></a>Configurar novamente a gravação de dispositivo e autenticação de dispositivo  
> [!NOTE]
> Se você executou o Azure AD Connect, usando configurações expressas, os objetos de anúncio corretos tiverem sido criados para você.  No entanto, na maioria dos cenários do AD FS, Azure AD Connect foi executado com configurações personalizadas para configurar o AD FS, portanto as etapas a seguir são necessários.  

### <a name="create-ad-objects-for-ad-fs-device-authentication"></a>Criar objetos do AD para autenticação de dispositivo do AD FS  
Se seu farm AD FS já não estiver configurado para autenticação de dispositivo (você pode ver isso no console de gerenciamento do AD FS em serviço -> registro de dispositivo), siga estas etapas para criar os objetos do AD DS corretos e configuração.  

![Registro de dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device1.png)

>Observação: O abaixo comandos exigem ferramentas de administração do Active Directory, por isso se o servidor de Federação também não é um controlador de domínio, primeiro instale as ferramentas usando a etapa 1 abaixo.  Caso contrário, você pode ignorar a etapa 1.  

1.  Execute o **adicionar funções e recursos** assistente e selecione recurso **ferramentas de administração de servidor remoto** -> **ferramentas de administração de função** -> **AD DS e ferramentas do AD LDS** -> escolher os dois **módulo do Active Directory para Windows PowerShell** e o **ferramentas do AD DS**.

![Registro de dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device2.png)
  
2.  No servidor do AD FS principal, certifique-se de que você efetuou logon como usuário do AD DS com privilégios de administrador da empresa (EA) e abra um prompt do powershell com privilégios elevados.  Em seguida, execute os seguintes comandos do PowerShell:  
    
    `Import-module activedirectory`  
    `PS C:\> Initialize-ADDeviceRegistration -ServiceAccountName "<your service account>" ` 
3.  Na janela pop-up pressione Sim.

>Observação: Se seu serviço AD FS está configurado para usar uma conta GMSA, insira o nome da conta no formato "domain\accountname$"

![Registro de dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device3.png)  

O PSH acima cria os objetos a seguir:  


- Recipiente RegisteredDevices sob a partição de domínio do AD  
- Contêiner de serviço de registro do dispositivo e o objeto em Configuração--> Serviços--> Configuração do registro de dispositivo  
- Contêiner de DKM de serviço de registro do dispositivo e o objeto em Configuração--> Serviços--> Configuração do registro de dispositivo  

![Registro de dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device4.png)  

4.  Depois disso, você verá uma mensagem de conclusão bem-sucedida.

![Registro de dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device5.png) 

###        <a name="create-service-connection-point-scp-in-ad"></a>Criar um ponto de Conexão de serviço (SCP) no AD  
Se você pretende usar o Windows 10 ingresso em domínio (com o registro automático no Azure AD) conforme descrito aqui, execute os seguintes comandos para criar um ponto de conexão de serviço no AD DS  
1.  Abra o Windows PowerShell e execute o seguinte:
    
    `PS C:>Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1" ` 

>Observação: se necessário, copie o arquivo AdSyncPrep.psm1 do seu servidor do Azure AD Connect.  Esse arquivo está localizado no programa de Programas\Microsoft Azure Active Directory Connect\AdPrep

![Registro de dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device6.png)   

2. Fornecer suas credenciais de administrador global do Azure AD  

    `PS C:>$aadAdminCred = Get-Credential`

![Registro de dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device7.png) 

3.  Execute o seguinte comando do PowerShell 

    `PS C:>Initialize-ADSyncDomainJoinedComputerSync -AdConnectorAccount [AD connector account name] -AzureADCredentials $aadAdminCred ` 

Onde o [AD conector conta name] é o nome da conta que você configurou no Azure AD Connect ao adicionar suas instalações diretório do AD DS.
  
Os comandos acima permitem que os clientes do Windows 10 encontrar a correta domínio do Azure AD para participar, criando o objeto serviceConnectionpoint no AD DS.  

### <a name="prepare-ad-for-device-write-back"></a>Preparar AD para voltar a gravação de dispositivo   
Para garantir a contêineres e objetos do AD DS estejam no estado correto para gravação atrás dispositivos do Azure AD, faça o seguinte.

1.  Abra o Windows PowerShell e execute o seguinte:  

    `PS C:>Initialize-ADSyncDeviceWriteBack -DomainName <AD DS domain name> -AdConnectorAccount [AD connector account name] ` 

Onde o [AD conector conta name] é o nome da conta que você configurou no Azure AD Connect ao adicionar suas instalações diretório do AD DS no formato domain\accountname  

O comando acima cria os objetos a seguir para gravação de dispositivo volta para o AD DS, se eles não existir e permite o acesso ao nome da conta especificado do conector AD  

- Contêiner RegisteredDevices na partição de domínio do AD  
- Contêiner de serviço de registro do dispositivo e o objeto em Configuração--> Serviços--> Configuração do registro de dispositivo  

### <a name="enable-device-write-back-in-azure-ad-connect"></a>Permitir gravação de dispositivo novamente no Azure AD Connect  
Se você não tiver feito assim antes, habilitar a gravação de dispositivo no Azure AD Connect, executando o Assistente uma segunda vez e selecionando **"Personalizar opções de sincronização"**, marcando a caixa para voltar a gravação de dispositivo e selecionando a floresta em que você executou os cmdlets acima  

### <a name="configure-device-authentication-in-ad-fs"></a>Configurar a autenticação de dispositivo no AD FS  
Usando uma janela de comando do PowerShell com privilégios elevados, configurar a política do AD FS executando o seguinte comando  

`PS C:>Set-AdfsGlobalAuthenticationPolicy -DeviceAuthenticationEnabled $true -DeviceAuthenticationMethod All` 

### <a name="check-your-configuration"></a>Verifique a configuração  
Para referência, veja a seguir uma lista abrangente de dispositivos o AD DS, contêineres e as permissões necessárias para autenticação e dispositivo gravação-voltar para trabalhar
 


- Objeto de tipo ms-DS-DeviceContainer em CN = RegisteredDevices, DC =&lt;domínio&gt;        
    - Acesso de leitura para a conta de serviço do AD FS   
    - Acesso de leitura/gravação para a sincronização conta conector do AD do Azure AD Connect</br></br>

- Contêiner CN = Configuração de registro do dispositivo, CN = Serviços, CN = Configuration, DC =&lt;domínio&gt;  
- DKM de serviço de registro do contêiner dispositivo sob o contêiner acima

![Registro de dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device8.png) 
 


- Objeto de tipo serviceConnectionpoint em CN =&lt;guid&gt;, CN = registro de dispositivo

- Configuração, CN = Serviços, CN = Configuration, DC =&lt;domínio&gt;  
 - Acesso de leitura/gravação para o nome da conta de conector do AD especificado no novo objeto</br></br> 


- Objeto de tipo msDS-DeviceRegistrationServiceContainer em CN = Serviços de registro de dispositivo, CN = Configuração de registro do dispositivo, CN = Serviços, CN = Configuration, DC = & ltdomain >  


- Objeto de tipo msDS-DeviceRegistrationService no contêiner acima  

### <a name="see-it-work"></a>Vê-lo funcionar  
Para avaliar as políticas e novas declarações, primeiro registre um dispositivo.  Por exemplo, você pode participar do Azure AD um computador Windows 10 usando o aplicativo configurações em sistema -> sobre, ou você pode configurar o Windows 10 ingresso em domínio com seguindo as etapas adicionais de registro de dispositivo automático [aqui](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/).  Para obter informações sobre como associar o Windows 10, dispositivos móveis, consulte o documento [aqui](https://technet.microsoft.com/itpro/windows/manage/join-windows-10-mobile-to-azure-active-directory).  

Para avaliação mais fácil, entra AD FS usando um aplicativo de teste que mostra uma lista das declarações. Você poderá ver novas declarações incluindo isManaged, isCompliant e trusttype.  Se você habilitar o Microsoft Passport para trabalhar, você também verá o prt reivindicar.  
 

## <a name="configure-additional-scenarios"></a>Configurar cenários adicionais  
### <a name="automatic-registration-for-windows-10-domain-joined-computers"></a>Computadores de registro para Windows 10 integrado ao domínio automáticas  
Para ativar o registro de dispositivo automático para o domínio do Windows 10 ingressa computadores, siga as etapas 1 e 2 [aqui](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/).   
Isso ajudará você a alcançar o seguinte:  

1. Certifique-se de seu ponto de conexão de serviço no AD DS existe e tem as permissões adequadas (criamos esse objeto acima, mas ele não prejudicar confirmamos).  
2. Certifique-se de que o AD FS está configurado corretamente  
3. Certifique-se de que o sistema AD FS tem os pontos de extremidade corretos habilitados e solicite as regras configuradas   
4. Definir as configurações de política de grupo necessárias para o registro de dispositivo automático de computadores ingressados no domínio   

### <a name="microsoft-passport-for-work"></a>Microsoft Passport for Work   
Para obter informações sobre como ativar o Windows 10 com o Microsoft Passport for Work, consulte [habilitar o Microsoft Passport for Work em sua organização.](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-passport-deployment/)  

### <a name="automatic-mdm-enrollment"></a>Registro automático no MDM   
Para habilitar o registro automático no MDM de dispositivos registrados para que você pode usar a declaração isCompliant em sua política de controle de acesso, siga as etapas [aqui.](https://blogs.technet.microsoft.com/ad/2015/08/14/windows-10-azure-ad-and-microsoft-intune-automatic-mdm-enrollment-powered-by-the-cloud/)  

## <a name="troubleshooting"></a>Solução de problemas  
1.  Se você receber um erro `Initialize-ADDeviceRegistration`que reclamar sobre um objeto já existente no estado errado, como "o objeto de serviço drs encontrado sem todos os atributos necessários", você pode ter executado o Azure AD Connect powershell comandos anteriormente e ter uma configuração parcial no AD DS.  Tente excluir manualmente os objetos sob **CN = Configuração de registro do dispositivo, CN = Serviços, CN = Configuration, DC =&lt;domínio&gt;** e tentar novamente.  
2.  No caso de domínio do Windows 10 ingressou clientes  
    1. Para verificar que a autenticação do dispositivo está funcionando, conecte-se para o cliente de participantes de domínio como uma conta de usuário de teste. Para disparar o provisionamento rapidamente, bloquear e desbloquear a área de trabalho pelo menos uma vez.   
    2. Instruções para verificar se há credenciais principais stk vincular no objeto do AD DS (Sincronizar ainda precisa ser executado duas vezes?)  
3.  Se você receber um erro ao tentar registrar um computador com Windows que o dispositivo já foi registrado, mas você não puder ou já ter registro cancelado o dispositivo, você pode ter um fragmento de configuração de registro do dispositivo no registro.  Para investigar e removê-la, use as seguintes etapas:  
    1. No computador do Windows, abra Regedit e navegue até **HKLM\Software\Microsoft\Enrollments**   
    2. Nessa chave, haverá muitas subchaves na forma GUID.  Navegue até a subchave que tem valores de ~ 17 nele e tem "EnrollmentType" de [MDM ingressou] "6" ou "13" (ingressado Azure AD)  
    3. Modificar **EnrollmentType** para **0** 
    4. Tente o registro de dispositivos ou o registro novamente  

### <a name="related-articles"></a>Artigos relacionados  
* [Proteger o acesso ao Office 365 e outros aplicativos conectados ao Azure Active Directory](https://azure.microsoft.com/en-us/documentation/articles/active-directory-conditional-access/)  
* [Políticas de dispositivo de acesso condicional para serviços do Office 365](https://azure.microsoft.com/en-us/documentation/articles/active-directory-conditional-access-device-policies/)  
* [Configurar o acesso condicional no local usando o registro de dispositivo do Azure Active Directory](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-device-registration-on-premises-setup)  
* [Conectar dispositivos ingressados em domínio ao Azure AD para experiências do Windows 10](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-devices-group-policy/)  
