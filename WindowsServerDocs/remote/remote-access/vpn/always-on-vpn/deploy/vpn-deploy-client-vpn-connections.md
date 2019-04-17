---
title: Configurar conexões de VPN Always On em cliente do Windows 10
description: Nesta etapa, você saiba mais sobre as opções de ProfileXML e esquema e configurar os computadores de cliente do Windows 10 para se comunicar com essa infraestrutura com uma conexão VPN.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 05/29/2018
ms.assetid: d165822d-b65c-40a2-b440-af495ad22f42
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.reviewer: deverette
ms.openlocfilehash: 6b383076686092e20448977bed3766f7d7d1c2b8
ms.sourcegitcommit: 4147e092d77d9458696e6686bccefe3788c90508
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/28/2019
ms.locfileid: "9031310"
---
# Etapa 6. Configurar as conexões de cliente VPN Always On Windows 10

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10


& #171;  [ **Anterior:** etapa 5. Definir as configurações de Firewall e DNS](vpn-deploy-dns-firewall.md)<br>
& #187; [ **Próximo:** etapa 7. (Opcional) Acesso condicional para conectividade VPN usando o Azure AD](../../ad-ca-vpn-connectivity-windows10.md)

Nesta etapa, você saiba mais sobre as opções de ProfileXML e esquema e configurar os computadores de cliente do Windows 10 para se comunicar com essa infraestrutura com uma conexão VPN. 

Você pode configurar o cliente VPN Always On por meio do PowerShell, SCCM ou Intune. Todos os três exigem um perfil de VPN de XML para definir as configurações de VPN apropriadas. Automatizando PowerShell registro para organizações sem SCCM ou o Intune é possível.

>[!NOTE]
>Política de grupo não inclui modelos administrativos para configurar o cliente do Windows 10 remoto acesso VPN Always On.  No entanto, você pode usar scripts de logon.

## Visão geral de ProfileXML

ProfileXML é um nó URI dentro do CSP VPNv2. Em vez de configurar cada nó do CSP VPNv2 individualmente — como gatilhos, rotear listas e protocolos de autenticação — use esse nó para configurar um cliente VPN do Windows 10, fornecendo todas as configurações como um único bloco XML para um único nó CSP. O esquema ProfileXML coincide com o esquema de nós do CSP VPNv2 quase idêntica, mas alguns termos são ligeiramente diferentes.

Você usa ProfileXML em todos os métodos de entrega que descreve essa implantação, incluindo o Windows PowerShell, System Center Configuration Manager e Intune. Há duas maneiras de configurar o nó ProfileXML CSP de VPNv2 nesta implantação:

- **OMA DM**. Uma maneira é usar um provedor MDM usando OMA DM, conforme discutido anteriormente na seção [nós do CSP VPNv2](../always-on-vpn-technology-overview.md#vpnv2-csp-nodes). Usando esse método, você pode inserir facilmente a marcação XML de configuração de perfil VPN para o nó ProfileXML CSP ao usar o Intune.

- **Windows Management Instrumentation (WMI) - to - ponte CSP**. O segundo método de configurar o nó ProfileXML CSP é usar a ponte WMI a CSP — uma classe WMI chamado **MDM_VPNv2_01**— que podem acessar o CSP de VPNv2 e o nó ProfileXML. Quando você cria uma nova instância da classe WMI, o WMI usa o CSP para criar o perfil VPN ao usar o Windows PowerShell e o System Center Configuration Manager.

Mesmo que esses métodos de configuração forem diferentes, ambos exigem um perfil de VPN de XML formatado corretamente. Para usar a configuração do CSP de VPNv2 ProfileXML, você pode construir XML usando o esquema ProfileXML para configurar as marcas necessárias para o cenário de implantação simples. Para obter mais informações, consulte [ProfileXML XSD](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-profile-xsd).

Abaixo você encontra cada uma das configurações necessárias e sua marca ProfileXML correspondente. Definir cada configuração em uma tag específica dentro do esquema ProfileXML, e nem todos eles são encontrados sob o perfil nativo. Para posicionamento de marca adicionais, consulte o esquema ProfileXML.

[!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]

**Tipo de Conexão:** IKEv2 nativo

ProfileXML elemento:

    <NativeProtocolType>IKEv2</NativeProtocolType>

**Roteamento:** Encapsulamento de divisão

ProfileXML elemento:

    <RoutingPolicyType>SplitTunnel</RoutingPolicyType>

**Resolução de nomes:** Sufixo DNS e a lista de informações de nome de domínio

ProfileXML elementos:

    <DomainNameInformation>
    <DomainName>.corp.contoso.com</DomainName>
    <DnsServers>10.10.1.10,10.10.1.50</DnsServers>
    </DomainNameInformation>
    
    <DnsSuffix>corp.contoso.com</DnsSuffix>


**Disparo:** Detecção de rede sempre ativado e confiável

ProfileXML elementos:

    <AlwaysOn>true</AlwaysOn>
    <TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection>


**Autenticação:** PEAP-TLS com certificados de usuário protegido pelo TPM

ProfileXML elementos:

    <Authentication>
    <UserMethod>Eap</UserMethod>
    <Eap>
    <Configuration>...</Configuration>
    </Eap>
    </Authentication>

Você pode usar marcas simples para configurar alguns mecanismos de autenticação VPN. No entanto, EAP e PEAP são mais complicadas. É a maneira mais fácil de criar a marcação XML configurar um cliente VPN com suas configurações de EAP e, em seguida, exportar essa configuração em XML. 

Para obter mais informações sobre as configurações de EAP, veja [Configuração EAP](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/eap-configuration). 

## <a name="bkmk_profile"></a>Criar um perfil de conexão do modelo manualmente

Nesta etapa, você pode usar Protected Extensible Authentication Protocol \(PEAP\) para proteger a comunicação entre o cliente e o servidor. Ao contrário de um nome de usuário simples e uma senha, essa conexão requer uma seção EAPConfiguration exclusiva no perfil da VPN funcione. 

Em vez de explicar como criar a marcação XML do zero, você usar configurações no Windows para criar um modelo de perfil de VPN. Depois de criar o modelo de perfil de VPN, usar o Windows PowerShell para consumir a parte EAPConfiguration do modelo para criar o ProfileXML final que você implantar posteriormente na implantação.

### Configurações de registro de certificado NPS

Antes de criar o modelo, anote o nome do host ou o nome de domínio totalmente qualificado (FQDN) do servidor NPS de certificado do servidor e o nome da autoridade de certificação que emitiu o certificado.

**Procedimento:**

1.  No seu servidor NPS, abra o servidor de políticas de rede.

2.  No console do NPS, políticas, clique em **Políticas de rede**.

3.  Clique com botão direito **conexões \(VPN\) de rede Virtual privada**e clique em **Propriedades**.

4.  Clique na guia **restrições** e clique em **Métodos de autenticação**.

5.  Em tipos de EAP, clique em **Microsoft: EAP protegido (PEAP)** e clique em **Editar**.

6.  Registre os valores para o **certificado emitido para** e o **emissor**.<p>Você pode usar esses valores na configuração do modelo VPN futura. Por exemplo, se o FQDN do servidor é nps01.corp.contoso.com e o nome do host é NPS01, o nome do certificado baseia-se o nome DNS ou o FQDN do servidor - por exemplo, nps01.corp.contoso.com.

7.  Cancele a caixa de diálogo Editar propriedades de EAP protegido.

8.  Cancele a caixa de diálogo de propriedades de conexões de rede Virtual privada (VPN).

9.  Feche o servidor de políticas de rede.

>[!NOTE]
>Se você tiver vários servidores NPS, execute estas etapas em cada um para que o perfil VPN possa verificar cada uma delas devem ser usados.

### Configurar o modelo de perfil de VPN em um computador cliente ingressado no domain\

Agora que você tem as informações necessárias a configurar o modelo de perfil de VPN em um computador cliente ingressado no domínio. O tipo de conta de usuário que você usar \ (ou seja, o usuário padrão ou administrator\) para essa parte do processo não importa. 

No entanto, se você ainda não reiniciou o computador desde Configurando o registro automático de certificado, fazer isso antes de configurar o modelo de conexão de VPN para garantir que você tenha um certificado utilizável registrado nele.

>[!NOTE]
>Não há nenhuma maneira de adicionar manualmente todas as propriedades avançadas de VPN, como regras NRPT, sempre ativado, confiável de detecção de rede, etc. Na próxima etapa, você cria uma conexão de VPN de teste para verificar a configuração do servidor VPN e que você pode estabelecer uma conexão VPN para o servidor.

**Criar manualmente um único teste conexão VPN**

1.  Entrar um computador cliente ingressado no domínio como um membro do grupo de **Usuários da VPN** .

2.  No menu Iniciar, digite **VPN**e pressione Enter.

3.  No painel de detalhes, clique em **Adicionar uma conexão VPN**.

4.  Na lista de provedor de VPN, clique em **Windows (interno)**.

5.  No nome da Conexão, digite o **modelo**.

6.  Em nome do servidor ou endereço, digite o **externo** FQDN do servidor VPN \ (por exemplo,**vpn.contoso.com**\).

7.  Clique em **Salvar**.

8.  Em configurações relacionadas, clique em **Opções de adaptador de alteração**.

9.  Clique com botão direito **modelo**e clique em **Propriedades**.

10. Na guia **segurança** , no **Tipo de VPN**, clique em **IKEv2**.

11. Na criptografia de dados, clique em **criptografia de segurança máxima**.

12. Clique em **uso Extensible Authentication Protocol (EAP)**; em seguida, no **Uso protocolo EAP (Extensible Authentication)**, clique em **Microsoft: EAP protegido (PEAP) (criptografia ativada)**.

13. Clique em **Propriedades** para abrir a caixa de diálogo Propriedades EAP protegidas e conclua as seguintes etapas:

    a. Na caixa **conectar a esses servidores** , digite o nome do servidor NPS que você recuperou das configurações de autenticação de servidor NPS no início desta seção (por exemplo, NPS01).

    >[!NOTE]
    >Você digitar o nome do servidor deve corresponder ao nome no certificado. Você recuperou esse nome no início desta seção. Se o nome não corresponder, a conexão falhará, informando que "a conexão foi impedida devido a uma política configurada no servidor RAS/VPN."

    b.  Em autoridades de certificação raiz confiáveis, selecione a autoridade de certificação que emitiu o certificado do servidor NPS (por exemplo, contoso-autoridade de certificação) raiz.

    c.  Nas notificações antes de se conectar, clique em **não peça ao usuário para autorizar novos servidores ou autoridades de certificação confiáveis**.

    d.  Selecione o método de autenticação, clique em **cartão inteligente ou outro certificado**e clique em **Configurar**. Abre o cartão inteligente ou outra caixa de diálogo de propriedades do certificado.

    e.  Clique em **usar um certificado neste computador**.

    f.  Na caixa conectar a esses servidores, insira o nome do servidor NPS que você recuperou das configurações de autenticação de servidor NPS nas etapas anteriores.

    g.  Em autoridades de certificação raiz confiáveis, selecione a autoridade de certificação que emitiu o certificado do servidor NPS raiz.

    h.  Selecione o **não avisar o usuário para autorizar novos servidores ou autoridades de certificação confiáveis** caixa de seleção.

    i.  Clique **Okey** para fechar o cartão inteligente ou outra caixa de diálogo de propriedades do certificado.

    j.  Clique **Okey** para fechar a caixa de diálogo Propriedades EAP protegidas.

10. Clique **Okey** para fechar a caixa de diálogo de propriedades de modelo.

11. Feche a janela de conexões de rede.

12. Em configurações, teste a VPN clicando em **Template**, e clique em **Conectar**.

>[!IMPORTANT]
>Certifique-se de que o modelo de conexão de VPN para o servidor VPN foi bem-sucedida. Isso garante que as configurações de EAP estão corretas antes de usá-los no próximo exemplo. Você deve se conectar pelo menos uma vez antes de continuar; Caso contrário, o perfil não irá conter todas as informações necessárias para se conectar à VPN.

## <a name="bkmk_ProfileXML"></a>Criar os arquivos de configuração ProfileXML

Antes de concluir esta seção, certifique-se de ter criado e testado o modelo de conexão VPN que descreve a seção [criar manualmente um perfil de conexão do modelo](#bkmk_profile) . Teste a conexão VPN é necessária para garantir que o perfil contém todas as informações necessárias para se conectar à VPN.

O script do Windows PowerShell na listagem 1 cria dois arquivos na área de trabalho, que contêm marcas **EAPConfiguration** com base no perfil de conexão do modelo criado anteriormente:

-   **VPN_Profile.XML.** Esse arquivo contém a marcação XML necessária para configurar o nó ProfileXML no CSP VPNv2. Use esse arquivo com serviços OMA DM – compatível com o MDM, como Intune.

-   **VPN_Profile.ps1.** Esse arquivo é um script do Windows PowerShell que podem ser executados em computadores cliente para configurar o nó ProfileXML no CSP VPNv2. Você também pode configurar o CSP Implantando esse script por meio do System Center Configuration Manager. Você não pode executar esse script em uma sessão de área de trabalho remota, incluindo uma sessão do Hyper-V avançado.

>[!IMPORTANT]
>Os comandos de exemplo abaixo exigem compilação do Windows 10 1607 ou posterior.

**Criar VPN_Profile.xml e VPN_Proflie.ps1**

1. Para entrar no computador cliente ingressado no domínio que contém o modelo de perfil VPN com o mesmo usuário da conta que a seção "Criar manualmente um perfil de conexão do modelo" descrito.

2. Colar listagem 1 no ambiente de script integrado do Windows PowerShell \(ISE\) e personalizar os parâmetros descritos nos comentários. Esses são $Template, $ProfileName, $Servers, $DnsSuffix, $DomainName, $TrustedNetwork e $DNSServers. Uma descrição completa de cada configuração é nos comentários.

3.  Execute o script para gerar **VPN_Profile.xml** e **VPN_Profile.ps1** na área de trabalho.

#### Listagem 1. Noções básicas sobre MakeProfile.ps1

Esta seção explica o código de exemplo que você pode usar para obter uma compreensão de como criar um perfil de VPN, especificamente para a configuração ProfileXML no CSP VPNv2.

Depois que você monta um script deste exemplo de código e executar o script, o script gera dois arquivos: VPN_Profile.xml e VPN_Profile.ps1. Use VPN_Profile.xml para definir ProfileXML em serviços MDM compatível com OMA DM, como Microsoft Intune.

Use o script **VPN_Profile.ps1** no Windows PowerShell ou o System Center Configuration Manager para configurar o ProfileXML na área de trabalho do Windows 10.

>[!NOTE]
>Para exibir o script de exemplo completo, consulte a seção [MakeProfile.ps1 Full Script](#bkmk_fullscript).

#### Parâmetros

Configure os parâmetros a seguir:

**$Template**. O nome do modelo do qual recuperar a configuração EAP.

**$ProfileName**. Identificador alfanumérico exclusivo para o perfil. O nome do perfil não deve incluir uma barra (/). Se o nome do perfil tem um espaço ou outros caracteres não alfanuméricos, devem ser precedida corretamente acordo com o padrão de codificação de URL.

**$Servers**. Endereço IP público ou roteável ou nome DNS do gateway VPN. Ele pode apontar para o external IP de um gateway ou um IP virtual para um farm de servidores. Exemplos, 208.147.66.130 ou vpn.contoso.com.

**$DnsSuffix**. Especifica os sufixos DNS separados por vírgulas de um ou mais. O primeiro item na lista também é usado como o sufixo DNS específico da conexão primário para a Interface VPN. A lista inteira também será adicionada em SuffixSearchList.

**$DomainName**. Usado para indicar o namespace aos quais a política se aplica. Quando uma consulta de nome é emitida, o cliente DNS compara o nome da consulta para todos os namespaces em /domainnameinformationlist para encontrar uma correspondência. Esse parâmetro pode ser um dos seguintes tipos:

- FQDN - nome de domínio totalmente qualificado
- Sufixo - um sufixo de domínio que será acrescentado a consulta shortname para resolução DNS. Para especificar um sufixo, coloque um período \ (.) para o sufixo DNS.

**$DNSServers**. Lista separada por vírgulas IP do servidor DNS endereços usar para o namespace.

**$TrustedNetwork**. String separados por vírgula para identificar a rede confiável. VPN não se conectará automaticamente quando o usuário estiver em sua rede sem fio corporativa, onde a recursos protegidos são acessados diretamente ao dispositivo.

Estes são os valores de exemplo para parâmetros usados nos comandos a seguir. Certifique-se de que você alterar esses valores para seu ambiente.

    $TemplateName = 'Template'
    $ProfileName = 'Contoso AlwaysOn VPN'
    $Servers = 'vpn.contoso.com'
    $DnsSuffix = 'corp.contoso.com'
    $DomainName = '.corp.contoso.com'
    $DNSServers = '10.10.0.2,10.10.0.3'
    $TrustedNetwork = 'corp.contoso.com'


### Preparar e criar o XML de perfil

Os seguintes comandos de exemplo obtém configurações de EAP do perfil do modelo:


    $Connection = Get-VpnConnection -Name $TemplateName
    if(!$Connection)
    {
    $Message = "Unable to get $TemplateName connection profile: $_"
    Write-Host "$Message"
    exit
    }
    $EAPSettings= $Connection.EapConfigXmlStream.InnerXml


### Criar o XML de perfil

[!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]

    $ProfileXML =
    '<VPNProfile>
      <DnsSuffix>' + $DnsSuffix + '</DnsSuffix>
      <NativeProfile>
    <Servers>' + $Servers + '</Servers>
    <NativeProtocolType>IKEv2</NativeProtocolType>
    <Authentication>
      <UserMethod>Eap</UserMethod>
      <Eap>
       <Configuration>
     '+ $EAPSettings + '
       </Configuration>
      </Eap>
    </Authentication>
    <RoutingPolicyType>SplitTunnel</RoutingPolicyType>
      </NativeProfile>
     <AlwaysOn>true</AlwaysOn>
     <RememberCredentials>true</RememberCredentials>
     <TrustedNetworkDetection>' + $TrustedNetwork + '</TrustedNetworkDetection>
      <DomainNameInformation>
    <DomainName>' + $DomainName + '</DomainName>
    <DnsServers>' + $DNSServers + '</DnsServers>
    </DomainNameInformation>
    </VPNProfile>'


### VPN_Profile.xml de saída para o Intune

Você pode usar o seguinte comando para salvar o arquivo XML de perfil:

    $ProfileXML | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.xml')

### VPN_Profile.ps1 de saída para a área de trabalho e o System Center Configuration Manager

O código de exemplo a seguir configura uma Conexão de VPN IKEv2 AlwaysOn usando o nó ProfileXML no CSP VPNv2.

Você pode usar esse script na área de trabalho do Windows 10 ou no System Center Configuration Manager.

### Definir parâmetros chave de perfil VPN

    $Script = '$ProfileName = ''' + $ProfileName + '''
    $ProfileNameEscaped = $ProfileName -replace ' ', '%20'

## Definir ProfileXML VPN

    $ProfileXML = ''' + $ProfileXML + '''
    
### Caracteres de escape especiais no perfil

    $ProfileXML = $ProfileXML -replace '<', '&lt;'
    $ProfileXML = $ProfileXML -replace '>', '&gt;'
    $ProfileXML = $ProfileXML -replace '"', '&quot;'
    
### Definir propriedades de ponte WMI a CSP

    $nodeCSPURI = "./Vendor/MSFT/VPNv2"
    $namespaceName = "root\cimv2\mdm\dmmap"
    $className = "MDM_VPNv2_01"

### Determine o SID de usuário para o perfil VPN:

    try
    {
    $username = Gwmi -Class Win32_ComputerSystem | select username
    $objuser = New-Object System.Security.Principal.NTAccount($username.username)
    $sid = $objuser.Translate([System.Security.Principal.SecurityIdentifier])
    $SidValue = $sid.Value
    $Message = "User SID is $SidValue."
    Write-Host "$Message"
    }
    catch [Exception]
    {
    $Message = "Unable to get user SID. User may be logged on over Remote Desktop: $_"
    Write-Host "$Message"
    exit
    }
    

### Defina a sessão WMI:

    $session = New-CimSession
    $options = New-Object Microsoft.Management.Infrastructure.Options.CimOperationOptions
    $options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Type", "PolicyPlatform_UserContext", $false)
    $options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Id", "$SidValue", $false)


### Detectar e excluir o perfil VPN anterior:

    try
    {
        $deleteInstances = $session.EnumerateInstances($namespaceName, $className, $options)
        foreach ($deleteInstance in $deleteInstances)
        {
            $InstanceId = $deleteInstance.InstanceID
            if ("$InstanceId" -eq "$ProfileNameEscaped")
            {
                $session.DeleteInstance($namespaceName, $deleteInstance, $options)
                $Message = "Removed $ProfileName profile $InstanceId"
                Write-Host "$Message"
            } else {
                $Message = "Ignoring existing VPN profile $InstanceId"
                Write-Host "$Message"
            }
        }
    }
    catch [Exception]
    {
        $Message = "Unable to remove existing outdated instance(s) of $ProfileName profile: $_"
        Write-Host "$Message"
        exit
    }
    

### Crie o perfil VPN:

    try
    {
        $newInstance = New-Object Microsoft.Management.Infrastructure.CimInstance $className, $namespaceName
        $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ParentID", "$nodeCSPURI", "String", "Key")
        $newInstance.CimInstanceProperties.Add($property)
        $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("InstanceID", "$ProfileNameEscaped", "String", "Key")
        $newInstance.CimInstanceProperties.Add($property)
        $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ProfileXML", "$ProfileXML", "String", "Property")
        $newInstance.CimInstanceProperties.Add($property)
        $session.CreateInstance($namespaceName, $newInstance, $options)
        $Message = "Created $ProfileName profile."


        Write-Host "$Message"
    }
    catch [Exception]
    {
    $Message = "Unable to create $ProfileName profile: $_"
    Write-Host "$Message"
    exit
    }
    
    $Message = "Script Complete"
    Write-Host "$Message"'

### Salve o arquivo XML de perfil

    $Script | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.ps1')
    
    $Message = "Successfully created VPN_Profile.xml and VPN_Profile.ps1 on the desktop."
    Write-Host "$Message"

## <a name="bkmk_fullscript"></a>Script completo MakeProfile.ps1

A maioria dos exemplos usam o cmdlet Set-WmiInstance Windows PowerShell para inserir ProfileXML em uma nova instância da classe MDM_VPNv2_01 WMI. 

No entanto, isso não funciona no System Center Configuration Manager porque você não pode executar o pacote no contexto dos usuários finais. Portanto, esse script usa o modelo comum de informações para criar uma sessão WMI no contexto do usuário e, em seguida, ele cria uma nova instância da classe WMI MDM_VPNv2_01 na sessão. Essa classe WMI usa a ponte WMI a CSP para configurar o CSP de VPNv2. Portanto, adicionando a instância de classe, você configura o CSP. 

>[!IMPORTANT]
>Ponte WMI a CSP requer direitos de administrador local, por design. Para implantar por perfis de VPN do usuário você deve estar usando o SCCM ou MDM.

>[!NOTE]
>O script VPN_Profile.ps1 usa o SID do usuário atual para identificar o contexto do usuário. Como nenhuma SID está disponível em uma sessão de área de trabalho remota, o script não funciona em uma sessão de área de trabalho remota. Da mesma forma, ele não funciona em uma sessão do Hyper-V avançado. Se você estiver testando um acesso remoto sempre em VPN em máquinas virtuais, desabilite a sessão avançado em seu cliente VMs antes de executar esse script.

O script de exemplo a seguir inclui todos os exemplos de código nas seções anteriores. Certifique-se de que você alterar valores de exemplo para valores que são apropriados para seu ambiente.
    
   ```XML
    $TemplateName = 'Template'
    $ProfileName = 'Contoso AlwaysOn VPN'
    $Servers = 'vpn.contoso.com'
    $DnsSuffix = 'corp.contoso.com'
    $DomainName = '.corp.contoso.com'
    $DNSServers = '10.10.0.2,10.10.0.3'
    $TrustedNetwork = 'corp.contoso.com'

    
    $Connection = Get-VpnConnection -Name $TemplateName
    if(!$Connection)
    {
    $Message = "Unable to get $TemplateName connection profile: $_"
    Write-Host "$Message"
    exit
    }
    $EAPSettings= $Connection.EapConfigXmlStream.InnerXml
    
    $ProfileXML =
    '<VPNProfile>
      <DnsSuffix>' + $DnsSuffix + '</DnsSuffix>
      <NativeProfile>
    <Servers>' + $Servers + '</Servers>
    <NativeProtocolType>IKEv2</NativeProtocolType>
    <Authentication>
      <UserMethod>Eap</UserMethod>
      <Eap>
       <Configuration>
     '+ $EAPSettings + '
       </Configuration>
      </Eap>
    </Authentication>
    <RoutingPolicyType>SplitTunnel</RoutingPolicyType>
      </NativeProfile>
    <AlwaysOn>true</AlwaysOn>
    <RememberCredentials>true</RememberCredentials>
    <TrustedNetworkDetection>' + $TrustedNetwork + '</TrustedNetworkDetection>
      <DomainNameInformation>
    <DomainName>' + $DomainName + '</DomainName>
    <DnsServers>' + $DNSServers + '</DnsServers>
    </DomainNameInformation>
    </VPNProfile>'
    
    $ProfileXML | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.xml')
    
    $Script = '$ProfileName = ''' + $ProfileName + '''
    $ProfileNameEscaped = $ProfileName -replace ' ', '%20'
    
    $ProfileXML = ''' + $ProfileXML + '''
    
    $ProfileXML = $ProfileXML -replace '<', '&lt;'
    $ProfileXML = $ProfileXML -replace '>', '&gt;'
    $ProfileXML = $ProfileXML -replace '"', '&quot;'
    
    $nodeCSPURI = "./Vendor/MSFT/VPNv2"
    $namespaceName = "root\cimv2\mdm\dmmap"
    $className = "MDM_VPNv2_01"
    
    try
    {
    $username = Gwmi -Class Win32_ComputerSystem | select username
    $objuser = New-Object System.Security.Principal.NTAccount($username.username)
    $sid = $objuser.Translate([System.Security.Principal.SecurityIdentifier])
    $SidValue = $sid.Value
    $Message = "User SID is $SidValue."
    Write-Host "$Message"
    }
    catch [Exception]
    {
    $Message = "Unable to get user SID. User may be logged on over Remote Desktop: $_"
    Write-Host "$Message"
    exit
    }
    
    $session = New-CimSession
    $options = New-Object Microsoft.Management.Infrastructure.Options.CimOperationOptions
    $options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Type", "PolicyPlatform_UserContext", $false)
    $options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Id", "$SidValue", $false)
    
        try
    {
        $deleteInstances = $session.EnumerateInstances($namespaceName, $className, $options)
        foreach ($deleteInstance in $deleteInstances)
        {
            $InstanceId = $deleteInstance.InstanceID
            if ("$InstanceId" -eq "$ProfileNameEscaped")
            {
                $session.DeleteInstance($namespaceName, $deleteInstance, $options)
                $Message = "Removed $ProfileName profile $InstanceId"
                Write-Host "$Message"
            } else {
                $Message = "Ignoring existing VPN profile $InstanceId"
                Write-Host "$Message"
            }
        }
    }
    catch [Exception]
    {
        $Message = "Unable to remove existing outdated instance(s) of $ProfileName profile: $_"
        Write-Host "$Message"
        exit
    }
    
    try
    {
        $newInstance = New-Object Microsoft.Management.Infrastructure.CimInstance $className, $namespaceName
        $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ParentID", "$nodeCSPURI", "String", "Key")
        $newInstance.CimInstanceProperties.Add($property)
        $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("InstanceID", "$ProfileNameEscaped", "String", "Key")
        $newInstance.CimInstanceProperties.Add($property)
        $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ProfileXML", "$ProfileXML", "String", "Property")
        $newInstance.CimInstanceProperties.Add($property)
        $session.CreateInstance($namespaceName, $newInstance, $options)
        $Message = "Created $ProfileName profile."

        Write-Host "$Message"
    }
    catch [Exception]
    {
        $Message = "Unable to create $ProfileName profile: $_"
        Write-Host "$Message"
        exit
    }
    
    $Message = "Script Complete"
    Write-Host "$Message"'
    
    $Script | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.ps1')
    
    $Message = "Successfully created VPN_Profile.xml and VPN_Profile.ps1 on the desktop."
    Write-Host "$Message"
   ```

## Configurar o cliente VPN usando o Windows PowerShell

Para configurar o CSP de VPNv2 em um computador de cliente do Windows 10, execute o script VPN_Profile.ps1 Windows PowerShell que você criou na seção [criar o XML de perfil](#create-the-profile-xml) . Abra o Windows PowerShell como administrador; Caso contrário, você receberá um erro informando, _acesso negado_.

Depois de executar VPN_Profile.ps1 para configurar o perfil VPN, você pode verificar a qualquer momento que ele foi bem-sucedida, executando o seguinte comando no ISE do Windows PowerShell:

    Get-WmiObject -Namespace root\cimv2\mdm\dmmap -Class MDM_VPNv2_01

**Bem-sucedida resultados do cmdlet Get-WmiObject**


    __GENUS : 2
    __CLASS : MDM_VPNv2_01
    __SUPERCLASS:
    __DYNASTY   : MDM_VPNv2_01
    __RELPATH   : MDM_VPNv2_01.InstanceID="Contoso%20AlwaysOn%20VPN",ParentID
      ="./Vendor/MSFT/VPNv2"
    __PROPERTY_COUNT: 10
    __DERIVATION: {}
    __SERVER: WIN01
    __NAMESPACE : root\cimv2\mdm\dmmap
    __PATH  : \\WIN01\root\cimv2\mdm\dmmap:MDM_VPNv2_01.InstanceID="Conto
      so%20AlwaysOn%20VPN",ParentID="./Vendor/MSFT/VPNv2"
    AlwaysOn: True
    ByPassForLocal  :
    DnsSuffix   : corp.contoso.com
    EdpModeId   :
    InstanceID  : Contoso%20AlwaysOn%20VPN
    LockDown:
    ParentID: ./Vendor/MSFT/VPNv2
    ProfileXML  : <VPNProfile><RememberCredentials>true</RememberCredentials>
      <AlwaysOn>true</AlwaysOn><DnsSuffix>corp.contoso.com</DnsSu
      ffix><TrustedNetworkDetection>corp.contoso.com</TrustedNetw
      orkDetection><NativeProfile><Servers>vpn.contoso.com;vpn.co
      ntoso.com</Servers><RoutingPolicyType>SplitTunnel</RoutingP
      olicyType><NativeProtocolType>Ikev2</NativeProtocolType><Au
      thentication><UserMethod>Eap</UserMethod><MachineMethod>Eap
      </MachineMethod><Eap><Configuration><EapHostConfig xmlns="h
      ttp://www.microsoft.com/provisioning/EapHostConfig"><EapMet
      hod><Type xmlns="https://www.microsoft.com/provisioning/EapC
      ommon">25</Type><VendorId xmlns="https://www.microsoft.com/p
      rovisioning/EapCommon">0</VendorId><VendorType xmlns="http:
      //www.microsoft.com/provisioning/EapCommon">0</VendorType><
      AuthorId xmlns="https://www.microsoft.com/provisioning/EapCo
      mmon">0</AuthorId></EapMethod><Config xmlns="https://www.mic
      rosoft.com/provisioning/EapHostConfig"><Eap xmlns="https://w
      ww.microsoft.com/provisioning/BaseEapConnectionPropertiesV1
      "><Type>25</Type><EapType xmlns="https://www.microsoft.com/p
      rovisioning/MsPeapConnectionPropertiesV1"><ServerValidation
      ><DisableUserPromptForServerValidation>true</DisableUserPro
      mptForServerValidation><ServerNames>NPS</ServerNames><Trust
      edRootCA>3f 07 88 e8 ac 00 32 e4 06 3f 30 f8 db 74 25 e1
      2e 5b 84 d1 </TrustedRootCA></ServerValidation><FastReconne
      ct>true</FastReconnect><InnerEapOptional>false</InnerEapOpt
      ional><Eap xmlns="https://www.microsoft.com/provisioning/Bas
      eEapConnectionPropertiesV1"><Type>13</Type><EapType xmlns="
      https://www.microsoft.com/provisioning/EapTlsConnectionPrope
      rtiesV1"><CredentialsSource><CertificateStore><SimpleCertSe
      lection>true</SimpleCertSelection></CertificateStore></Cred
      entialsSource><ServerValidation><DisableUserPromptForServer
      Validation>true</DisableUserPromptForServerValidation><Serv
      erNames>NPS</ServerNames><TrustedRootCA>3f 07 88 e8 ac 00
      32 e4 06 3f 30 f8 db 74 25 e1 2e 5b 84 d1 </TrustedRootCA><
      /ServerValidation><DifferentUsername>false</DifferentUserna
      me><PerformServerValidation xmlns="https://www.microsoft.com
      /provisioning/EapTlsConnectionPropertiesV2">true</PerformSe
      rverValidation><AcceptServerName xmlns="https://www.microsof
      t.com/provisioning/EapTlsConnectionPropertiesV2">true</Acce
      ptServerName></EapType></Eap><EnableQuarantineChecks>false<
      /EnableQuarantineChecks><RequireCryptoBinding>false</Requir
      eCryptoBinding><PeapExtensions><PerformServerValidation xml
      ns="https://www.microsoft.com/provisioning/MsPeapConnectionP
      ropertiesV2">true</PerformServerValidation><AcceptServerNam
      e xmlns="https://www.microsoft.com/provisioning/MsPeapConnec
      tionPropertiesV2">true</AcceptServerName></PeapExtensions><
      /EapType></Eap></Config></EapHostConfig></Configuration></E
      ap></Authentication></NativeProfile><DomainNameInformation>
      <DomainName>corp.contoso.com</DomainName><DnsServers>10.10.
      0.2,10.10.0.3</DnsServers><AutoTrigger>true</AutoTrigger></
      DomainNameInformation></VPNProfile>
    RememberCredentials : True
    TrustedNetworkDetection : corp.contoso.com
    PSComputerName  : WIN01

A configuração ProfileXML deve ser correta na estrutura, ortografia, configuração e, às vezes, diferenciar maiusculas de minúsculas. Se você vir algo diferente na estrutura para listagem 1, a marcação de ProfileXML provavelmente contém um erro.

Se você precisar solucionar problemas de marcação, é mais fácil de colocá-lo em um editor de XML que ao solucioná-lo no Windows PowerShell ISE. Em ambos os casos, comece com a versão mais simples do perfil e adicionar componentes volta um de cada vez até que o problema ocorre novamente.

## <a name="vpn-deploy-client-sccm"></a>Configurar o cliente VPN usando o System Center Configuration Manager

No System Center Configuration Manager, você pode implantar perfis de VPN usando o nó ProfileXML CSP, assim como fez no Windows PowerShell. Aqui, você pode usar o script VPN_Profile.ps1 Windows PowerShell que você criou na seção [criar os arquivos de configuração ProfileXML](#bkmk_ProfileXML).

Para usar o System Center Configuration Manager para implantar um perfil de acesso remoto sempre em VPN em computadores cliente Windows 10, você deve começar com a criação de um grupo de computadores ou usuários a quem você implantar o perfil. Nesse cenário, crie um grupo de usuários para implantar o script de configuração.

### Criar um grupo de usuários

1.  No console do Configuration Manager, abra ativos e Compliance\\User coleções.

2.  Na faixa **início** , no grupo de **criar** , clique em **Criar coleção de usuário**.

3.  Na página General, conclua as seguintes etapas:

    a. Em **nome**, digite **Os usuários da VPN**.

    b. Clique em **Procurar**, clique em **Todos os usuários** e clique em **Okey**.

    c. Clique em **Avançar**.

4.  Na página regras de associação, conclua as seguintes etapas:

    a.  Nas **regras de associação**, clique em **Adicionar regra**e clique em **Regra direta**. Neste exemplo, você está adicionando usuários individuais à coleção de usuário. No entanto, você pode usar uma regra de consulta para adicionar usuários a essa coleção dinamicamente para uma implantação de escala maior.

    b.  Na página **Bem-vindo** , clique em **Avançar**.

    c.  Na pesquisa para a página de recursos, no **valor**, digite o nome do usuário que você deseja adicionar. O nome do recurso inclui o domínio do usuário. Para incluir resultados com base em uma correspondência parcial, insira o **%** caractere em ambas as extremidades de seu critério de pesquisa. Por exemplo, para localizar todos os usuários que contém a cadeia de caracteres "lori", digite **% lori %**. Clique em **Avançar**.

    d.  Na página Selecionar recursos, selecione os usuários que você deseja adicionar ao grupo e clique em **Avançar**.

    e.  Na página de resumo, clique em **Avançar**.

    f.  Na página Conclusão, clique em **Fechar**.

6.  Voltar na página de regras de associação do Assistente para criação de coleção de usuário, clique em **Avançar**.

7.  Na página de resumo, clique em **Avançar**.

8.  Na página Conclusão, clique em **Fechar**.

Depois de criar o grupo de usuários para receber o perfil VPN, você pode criar um pacote e o programa para implantar o script de configuração do Windows PowerShell que você criou na seção [criar os arquivos de configuração ProfileXML](#bkmk_ProfileXML).

### Criar um pacote que contém o script de configuração ProfileXML

1.  Hospede o script VPN_Profile.ps1 em um compartilhamento de rede que pode acessar a conta de computador de servidor do site.

2.  No console do Configuration Manager, abra o **Software Library\\Application aplicativo\\pacotes**.

3.  Na faixa **início** , no grupo de **criar** , clique em **Criar pacote** para iniciar o Assistente de programa e criar pacote.

4.  Na página Package, conclua as seguintes etapas:

    a. Em **nome**, digite o **Windows 10 sempre no perfil de VPN**.

    b. Marque a caixa de seleção **esse pacote contém arquivos de origem** e clique em **Procurar**.

    c. Na caixa de diálogo Definir a pasta de origem, clique em **Procurar**, selecione o compartilhamento de arquivo que contém VPN_Profile.ps1 e clique em **Okey**.<p>Certifique-se de que selecionar um caminho de rede, não é um caminho local. Em outras palavras, o caminho deve ser algo parecido com *\\fileserver\\vpnscript*, *c:\\vpnscript*.

1.  Clique em **Avançar**.

2.  Na página tipo de programa, clique em **Avançar**.

3.  Na página programa padrão, conclua as seguintes etapas:

    a.  Em **nome**, digite o **Script de perfil de VPN**.

    b.  Na **linha de comando**, digite **PowerShell.exe - ExecutionPolicy Ignorar - arquivo "VPN_Profile.ps1"**.

    c.  No **modo de execução**, clique em **executar com direitos administrativos**.

    d.  Clique em **Avançar**.

4.  Na página requisitos, conclua as seguintes etapas:

    a.  Selecione **esse programa pode ser executado somente em plataformas especificadas**.

    b.  Marque as caixas de seleção de **todos os Windows 10 (32 bits)** e **todos os Windows 10 (64 bits)** .

    c.  **Espaço em disco estimado**, digite **1**.

    d.  No **número máximo permitido de tempo de execução (minutos)**, digite **15**.

    e.  Clique em **Avançar**.

5.  Na página de resumo, clique em **Avançar**.

6.  Na página Conclusão, clique em **Fechar**.

Com o pacote e o programa criado, você precisará implantá-lo ao grupo de **Usuários da VPN** .

### Implantar o script de configuração ProfileXML

1.  No console do Configuration Manager, abra o Software Library\\Application aplicativo\\pacotes.

2.  Em **pacotes**, clique em **Windows 10 sempre no perfil de VPN**.

3.  Na guia **programas** , na parte inferior do painel de detalhes, clique com botão direito **Script de perfil de VPN**, clique em **Propriedades**e conclua as seguintes etapas:

    a.  Na guia **Avançado** , no **quando este programa é atribuído a um computador**, clique em **uma vez para cada usuário que fez logon**.

    b.  Clique em **OK**.

4.  Clique com botão direito **Script de perfil de VPN** e clique em **Deploy** para iniciar o Assistente de implantação de Software.

5.  Na página General, conclua as seguintes etapas:

    a.  Ao lado de **coleção**, clique em **Procurar**.

    b.  Na lista de **Tipos de coleção** (superior esquerda), clique em **Coleções de usuário**.

    c.  Clique em **Usuários de VPN**e clique em **Okey**.

    d.  Clique em **Avançar**.

6.  Na página de conteúdo, conclua as seguintes etapas:

    a.  Clique em **Adicionar**e clique em **Ponto de distribuição**.

    b.  Em **pontos de distribuição disponíveis**, selecione os pontos de distribuição para o qual você deseja distribuir o script de configuração ProfileXML e clique em **Okey**.

    c.  Clique em **Avançar**.

7.  Na página de configurações de implantação, clique em **Avançar**.

8.  Na página agendamento, conclua as seguintes etapas:

    a.  Clique em **novo** para abrir a caixa de diálogo de agendamento de atribuição.

    b.  Clique em **atribuir imediatamente após este evento**e clique em **Okey**.

    c.  Clique em **Avançar**.

9.  Na página experiência do usuário, conclua as seguintes etapas:

    1.  Marque a caixa de seleção de **Instalação de Software** .

    2.  Clique em **Resumo**.

10. Na página de resumo, clique em **Avançar**.

11. Na página Conclusão, clique em **Fechar**.

Com o script de configuração ProfileXML implantado, fazer logon em um computador de cliente do Windows 10 com a conta de usuário que você selecionou quando você criou a coleção de usuário. Verifique se a configuração do cliente VPN.

>[!NOTE]
>O script VPN_Profile.ps1 não funciona em uma sessão de área de trabalho remota. Da mesma forma, ele não funciona em uma sessão do Hyper-V avançado. Se você estiver testando um acesso remoto sempre em VPN em máquinas virtuais, desabilite a sessão avançado em seu cliente VMs antes de continuar.

### Verificar a configuração do cliente VPN

1.  No painel de controle, em **System\\Security**, clique em **Configuration Manager**. 

2.  Na caixa de diálogo Properties do Configuration Manager, na guia **ações** , conclua as seguintes etapas:

    a.  Clique **& Machine Policy Retrieval Evaluation Cycle**, clique em **Executar agora**e clique em **Okey**.

    b.  Clique em **usuário Policy Retrieval & Evaluation Cycle**, clique em **Run Now**e clique em **Okey**.

    c.  Clique em **OK**.

3.  Feche o painel de controle.

Você deve ver o novo perfil VPN em breve.

## Configurar o cliente VPN usando o Intune

Para usar o Intune para implantar o Windows 10 remoto acesso sempre em perfis de VPN, você pode configurar o nó ProfileXML CSP usando o perfil VPN que você criou na seção [criar os arquivos de configuração ProfileXML](#bkmk_ProfileXML), ou você pode usar o exemplo de XML do EAP base fornecido abaixo.

>[!NOTE]
>Agora, o Intune usa grupos do Azure AD. Se o Azure AD Connect sincronizados o grupo de usuários de VPN de local ao Azure AD e os usuários são atribuídos ao grupo de usuários de VPN, você estará pronto para continuar.

Crie a política de configuração do dispositivo VPN para configurar os computadores de cliente do Windows 10 para todos os usuários adicionados ao grupo. Como o modelo Intune fornece parâmetros VPN, copie apenas a parte \<EapHostConfig> \</EapHostConfig> do arquivo VPN_ProfileXML. 


### Criar a política de configuração de VPN Always On

1.  Entrar no [portal do Azure](https://portal.azure.com/).

2.  Vá para o **Intune** > **configuração do dispositivo** > **perfis**.

3.  Clique em **Criar perfil** para iniciar o Assistente para criar perfil.

4.  Insira um **nome** para o perfil VPN e (opcionalmente) uma descrição.

5.   Em **plataforma**, selecione o **Windows 10 ou posterior**e escolher suspensa tipo o perfil **VPN** .

     >[!TIP]
     >Se você estiver criando um profileXML VPN personalizado, consulte [Aplicar ProfileXML usando o Intune](https://docs.microsoft.com/windows/security/identity-protection/vpn/vpn-profile-options#apply-profilexml-using-intune) para as instruções.

6. Na guia **Base VPN** , verifique ou defina as seguintes configurações:

    - **Nome da Conexão:** Insira o nome da conexão VPN como ele aparece no computador cliente na guia **VPN** em **configurações**, por exemplo, _Contoso AutoVPN_.  
    
    - **Servidores:** Adicione um ou mais servidores VPN, clicando em **Adicionar.**
    
    - **Descrição** e **endereço IP ou FQDN:** inserir a descrição e o endereço IP ou o FQDN do servidor VPN. Esses valores devem se alinhar com o nome do assunto no certificado de autenticação do servidor VPN. 
    
    - **Servidor padrão:** Se esse for o servidor VPN padrão, definida como **True**. Isso permite que este servidor como o servidor padrão que usam dispositivos para estabelecer a conexão. 
    
    - **Tipo de Conexão:** Defina como **IKEv2**.  
    
    - **Sempre em:** Defina como **Habilitar** para se conectar à VPN automaticamente à entrar e permanecer conectado até que o usuário se desconectar manualmente.
    
    - **Lembrar credenciais em cada login**: valor booliano (verdadeiro ou falso) para armazenar em cache as credenciais. Se definida como true, credenciais é armazenadas em cache sempre que possível.

7.  Copie a seguinte cadeia de caracteres XML para um editor de texto:<p>
 
    [!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]
    <p>
    
    ```XML
    <EapHostConfig xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><EapMethod><Type xmlns="https://www.microsoft.com/provisioning/EapCommon">25</Type><VendorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorId><VendorType xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorType><AuthorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</AuthorId></EapMethod><Config xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>25</Type><EapType xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV1"><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><FastReconnect>true</FastReconnect><InnerEapOptional>false</InnerEapOptional><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>13</Type><EapType xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV1"><CredentialsSource><CertificateStore><SimpleCertSelection>true</SimpleCertSelection></CertificateStore></CredentialsSource><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><DifferentUsername>false</DifferentUsername><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</AcceptServerName></EapType></Eap><EnableQuarantineChecks>false</EnableQuarantineChecks><RequireCryptoBinding>false</RequireCryptoBinding><PeapExtensions><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</AcceptServerName></PeapExtensions></EapType></Eap></Config></EapHostConfig>
    ```

8.  Substitua o **\<TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1C c2 68 ser 4b<\/TrustedRootCA>** no exemplo com a impressão digital do certificado de autoridade de certificação raiz locais em ambos os lugares.
  
    >[!Important]
    >Não use a impressão digital de exemplo na seção \<TrustedRootCA>\</TrustedRootCA> abaixo.  O TrustedRootCA deve ser a impressão digital do certificado da autoridade de certificado raiz locais que emitiu o certificado de autenticação de servidor para servidores RRAS e NPS. **Isso não deve ser o certificado raiz de nuvem, nem a impressão digital certificado intermediária emissão CA**.

10. Substitua o FQDN do NPS ingressado no domínio em que a autenticação é feita a **\<ServerNames>NPS.contoso.com\</ServerNames>** no exemplo de XML. 

11. Copie a cadeia de caracteres XML revisada e cole na caixa **Xml do EAP** na guia Base VPN e clique em **Okey**.<p>Uma política sempre no dispositivo configuração de VPN usando o EAP é criada no Intune.


### Sincronizar a política de configuração de VPN sempre ativa com o Intune

Para testar a política de configuração, entre um computador de cliente do Windows 10 como o usuário que você adicionou ao grupo **Sempre em usuários da VPN** e, em seguida, sincronizar com o Intune.

1.  No menu Iniciar, clique em **configurações**.

2.  Em configurações, clique em **contas**e clique em **acessar trabalho ou escola**.

3.  Clique no perfil do MDM e clique em **informações**.

4.  Clique em **sincronização** para forçar uma avaliação de diretiva Intune e a recuperação.

5.  Feche configurações. Após a sincronização, você pode ver o perfil VPN disponíveis no computador.

## Próximas etapas
Você terminou de implantação VPN Always On.  Para outros recursos que você pode configurar, consulte a tabela a seguir:

|Se você quiser...  |Em seguida, consulte...  |
|---------|---------|
|Configurar o acesso condicional para VPN    |[Etapa 7. (Opcional) Configurar o acesso condicional para conectividade VPN usando o Azure AD](../../ad-ca-vpn-connectivity-windows10.md): nesta etapa, você pode ajustar seus recursos usando o [acesso condicional do Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal)de acesso de usuários autorizado como VPN. Com acesso condicional do Azure AD para conectividade de rede virtual privada (VPN), você pode ajudar a proteger as conexões VPN. O Acesso Condicional é um mecanismo de avaliação com base em política que permite que você crie regras de acesso para qualquer aplicativo conectado ao Azure Active Directory (Azure AD).         |
|Saiba mais sobre os recursos avançados de VPN  |[Recursos avançados de VPN](always-on-vpn-adv-options.md#advanced-vpn-features): esta página fornece orientações sobre como habilitar filtros de tráfego de VPN, como configurar conexões VPN automática usando o aplicativo gatilhos e como configurar o NPS para permitir somente conexões VPN de clientes usando certificados emitidos pelo Azure AD.        |


---
