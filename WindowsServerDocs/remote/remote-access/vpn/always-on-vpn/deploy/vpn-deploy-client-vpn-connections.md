---
title: Configurar conexões de VPN Always On em cliente do Windows 10
description: Nesta etapa, saiba mais sobre as opções de ProfileXML e o esquema e configurar os computadores de clientes do Windows 10 para se comunicar com que a infraestrutura com uma conexão VPN.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 05/29/2018
ms.assetid: d165822d-b65c-40a2-b440-af495ad22f42
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.reviewer: deverette
ms.openlocfilehash: fdf640d7e98781ca054ab982027bdfe8c3fb64d6
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749630"
---
# <a name="step-6-configure-windows-10-client-always-on-vpn-connections"></a>Etapa 6. Configurar as conexões de cliente VPN Always On Windows 10

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior:** Etapa 5. Definir as configurações de DNS e de firewall](vpn-deploy-dns-firewall.md)<br>
- [**Avançar:** Etapa 7. (Opcional) Acesso condicional para conectividade VPN usando o Azure AD](../../ad-ca-vpn-connectivity-windows10.md)

Nesta etapa, você aprenderá sobre as opções de ProfileXML e o esquema e configurar os computadores de clientes do Windows 10 para se comunicar com que a infraestrutura com uma conexão VPN.

Você pode configurar o cliente VPN Always On, por meio do PowerShell, o SCCM ou do Intune. Todos os três exigem um perfil VPN XML para definir as configurações de VPN apropriadas. Automatizando PowerShell registro para organizações sem o SCCM ou do Intune é possível.

>[!NOTE]
>Diretiva de grupo não inclui modelos administrativos para configurar o cliente do Windows 10 remoto acesso VPN Always On.  No entanto, você pode usar scripts de logon.

## <a name="profilexml-overview"></a>Visão geral do ProfileXML

ProfileXML é um nó URI dentro do CSP de VPNv2. Em vez de configurar cada nó de CSP de VPNv2 individualmente — como gatilhos, rotear os protocolos de autenticação e listas — use esse nó para configurar um cliente de VPN do Windows 10, fornecendo todas as configurações como um único bloco XML para um único nó CSP. O esquema do ProfileXML coincide com o esquema de nós do CSP de VPNv2 maneira quase idêntica, mas alguns termos são ligeiramente diferentes.

Você pode usar ProfileXML em todos os métodos de entrega que descreve esta implantação, incluindo o Windows PowerShell, o System Center Configuration Manager e o Intune. Há duas maneiras de configurar o nó do ProfileXML VPNv2 CSP nesta implantação:

- **OMA-DM**. Uma maneira é usar um provedor MDM usando o OMA-DM, conforme discutido anteriormente na seção [nós CSP de VPNv2](../always-on-vpn-technology-overview.md#vpnv2-csp-nodes). Usando esse método, você pode inserir facilmente a marcação XML de configuração de perfil VPN para o nó de CSP ProfileXML ao usar o Intune.

- **Windows Management Instrumentation (WMI) - para - ponte CSP**. O segundo método de configuração de nó ProfileXML CSP é usar a ponte de WMI para CSP — chamado de uma classe do WMI **MDM_VPNv2_01**— que pode acessar o CSP de VPNv2 e o nó do ProfileXML. Quando você cria uma nova instância da classe WMI, o WMI usa o CSP para criar o perfil VPN ao usar o Windows PowerShell e o System Center Configuration Manager.

Embora esses métodos de configuração forem diferentes, ambos requerem um perfil VPN XML formatado corretamente. Para usar a configuração do ProfileXML VPNv2 CSP, você pode construir XML usando o esquema do ProfileXML as marcas necessárias para o cenário de implantação simples de configurar. Para obter mais informações, consulte [ProfileXML XSD](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-profile-xsd).

Abaixo você encontrar cada uma das configurações necessárias e sua marca ProfileXML correspondente. Definir cada configuração em uma marca específica dentro do esquema ProfileXML, e nem todos eles são encontrados sob o perfil nativo. Para o posicionamento de marca adicionais, consulte o esquema do ProfileXML.

[!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]

**Tipo de Conexão:** Native IKEv2

Elemento ProfileXML:

```xml
<NativeProtocolType>IKEv2</NativeProtocolType>
```

**Roteamento:** Túnel dividido

Elemento ProfileXML:

```xml
<RoutingPolicyType>SplitTunnel</RoutingPolicyType>
```

**Resolução de nomes:** Sufixo DNS e a lista de informações de nome de domínio

Elementos do ProfileXML:

```xml
<DomainNameInformation>
<DomainName>.corp.contoso.com</DomainName>
<DnsServers>10.10.1.10,10.10.1.50</DnsServers>
</DomainNameInformation>

<DnsSuffix>corp.contoso.com</DnsSuffix>
```

**Disparar:** Detecção de rede sempre ativado e confiável

Elementos do ProfileXML:

```xml
<AlwaysOn>true</AlwaysOn>
<TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection>
```

**Autenticação:** PEAP-TLS com certificados de usuário protegidas por TPM

Elementos do ProfileXML:

```xml
<Authentication>
<UserMethod>Eap</UserMethod>
<Eap>
<Configuration>...</Configuration>
</Eap>
</Authentication>
```

Você pode usar marcas simples para configurar alguns mecanismos de autenticação de VPN. No entanto, o EAP e PEAP são mais envolvido. A maneira mais fácil de criar a marcação XML é configurar um cliente VPN com suas configurações de EAP e, em seguida, exportar essa configuração em XML.

Para obter mais informações sobre as configurações de EAP, consulte [da configuração EAP](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/eap-configuration).

## <a name="manually-create-a-template-connection-profile"></a>Criar manualmente um perfil de conexão de modelo

Nesta etapa, você pode usar Protected Extensible Authentication Protocol (PEAP) para proteger a comunicação entre o cliente e o servidor. Ao contrário de um nome de usuário simples e uma senha, essa conexão requer uma seção EAPConfiguration exclusiva no perfil de VPN para funcionar.

Em vez de descrever como criar a marcação XML a partir do zero, você usar configurações no Windows para criar um modelo de perfil de VPN. Depois de criar o modelo de perfil de VPN, use o Windows PowerShell para consumir a parte EAPConfiguration base nesse modelo para criar o ProfileXML final que você implantar mais tarde na implantação.

### <a name="record-nps-certificate-settings"></a>Configurações de registro de certificado NPS

Antes de criar o modelo, anote o nome do host ou nome de domínio totalmente qualificado (FQDN) do servidor de certificado do servidor NPS e o nome da autoridade de certificação que emitiu o certificado.

**Procedimento:**

1. No servidor NPS, abra o servidor de políticas de rede.

2. No console do NPS, em políticas, clique em **diretivas de rede**.

3. Clique com botão direito **conexões de rede Virtual privada (VPN)** e clique em **propriedades**.

4. Clique o **restrições** guia e, em seguida, clique em **métodos de autenticação**.

5. Em tipos de EAP, clique em **Microsoft: EAP protegido (PEAP)** e clique em **editar**.

6. Registre os valores para **certificado emitido para** e **emissor**.

    Você pode usar esses valores na configuração de modelo VPN futura. Por exemplo, se o FQDN do servidor é nps01.corp.contoso.com e o nome do host for NPS01, o nome do certificado baseia-se o nome DNS ou FQDN do servidor — por exemplo, nps01.corp.contoso.com.

7. Cancele a caixa de diálogo Editar propriedades de EAP protegidas.

8. Cancele a caixa de diálogo de propriedades de conexões de rede Virtual privada (VPN).

9. Feche o servidor de políticas de rede.

>[!NOTE]
>Se você tiver vários servidores NPS, conclua estas etapas em cada um para que o perfil de VPN pode verificar se cada um deles devem ser usados.

### <a name="configure-the-template-vpn-profile-on-a-domain-joined-client-computer"></a>Configurar o modelo de perfil de VPN em um computador cliente ingressado no domínio

Agora que você tem as informações necessárias a configurar o modelo de perfil de VPN em um computador cliente ingressado no domínio. O tipo de usuário conta que você use (ou seja, o usuário padrão ou o administrador) para essa parte do processo não importa.

No entanto, se você ainda não tiver reiniciado o computador desde como configurar o registro automático de certificado, fazer isso antes de configurar o modelo de conexão de VPN para garantir que você tenha um certificado utilizável registrado nele.

>[!NOTE]
>Não é possível adicionar manualmente todas as propriedades avançadas de VPN, como as regras NRPT, Always On confiável de detecção de rede, etc. Na próxima etapa, você cria uma conexão de VPN de teste para verificar a configuração do servidor VPN e que você pode estabelecer uma conexão VPN para o servidor.

**Criar manualmente um único teste de conexão de VPN**

1.  Entre em um computador cliente ingressado no domínio como um membro do **usuários da VPN** grupo.

2.  No menu Iniciar, digite **VPN**, e pressione Enter.

3.  No painel de detalhes, clique em **adicionar uma conexão VPN**.

4.  Na lista de provedor de VPN, clique em **(interno) do Windows**.

5.  Em nome de Conexão, digite **modelo**.

6.  Em nome do servidor ou endereço, digite o **externos** FQDN do servidor VPN (por exemplo, **vpn.contoso.com**).

7.  Clique em **Salvar**.

8.  Em configurações relacionadas, clique em **alterar as opções de adaptador**.

9.  Clique com botão direito **modelo**e clique em **propriedades**.

10. Sobre o **segurança** guia **tipo de VPN**, clique em **IKEv2**.

11. Na criptografia de dados, clique em **criptografia de segurança máxima**.

12. Clique em **usar protocolo EAP (Extensible Authentication)** ; em seguida, na **usar protocolo EAP (Extensible Authentication)** , clique em **Microsoft: EAP protegido (PEAP) (criptografia ativada)** .

13. Clique em **propriedades** para abrir a caixa de diálogo Propriedades EAP protegidas e conclua as seguintes etapas:

    a. No **conectar a estes servidores** , digite o nome do servidor NPS que você recuperou das configurações de autenticação do servidor NPS no início desta seção (por exemplo, NPS01).

    >[!NOTE]
    >O nome do servidor digitado deve corresponder ao nome no certificado. Esse nome no início desta seção é recuperado. Se o nome não corresponder, a conexão falhará, dizendo que "a conexão foi impedida devido a uma política configurada no servidor RAS/VPN."

    b.  Em autoridades de certificação raiz confiáveis, selecione a autoridade de certificação que emitiu o certificado do servidor NPS (por exemplo, contoso-AC) raiz.

    c.  Em notificações antes de se conectar, clique em **não perguntar para o usuário autorizar novos servidores ou CAs confiáveis**.

    d.  Em Selecionar método de autenticação, clique em **do cartão inteligente ou outro certificado**e clique em **configurar**. O cartão inteligente ou outra caixa de diálogo Propriedades do certificado é aberto.

    e.  Clique em **usar um certificado neste computador**.

    f.  Na caixa conectar a esses servidores, digite o nome do servidor NPS que você recuperou das configurações de autenticação do servidor NPS nas etapas anteriores.

    g.  Em autoridades de certificação raiz confiáveis, selecione a raiz da autoridade de certificação que emitiu o certificado do servidor NPS.

    h.  Selecione o **não avisar o usuário autorizar novos servidores ou autoridades de certificação confiáveis** caixa de seleção.

    i.  Clique em **Okey** para fechar o cartão inteligente ou outra caixa de diálogo Propriedades do certificado.

    j.  Clique em **Okey** para fechar a caixa de diálogo Propriedades EAP protegidas.

14. Clique em **Okey** para fechar a caixa de diálogo Propriedades do modelo.

15. Feche a janela Conexões de rede.

16. Em configurações, teste o VPN clicando **modelo**e clicando em **Connect**.

>[!IMPORTANT]
>Certifique-se de que o modelo de conexão de VPN para o servidor VPN foi bem-sucedida. Isso garante que as configurações de EAP estão corretas antes de usá-los no exemplo a seguir. Você deve se conectar pelo menos uma vez antes de continuar; Caso contrário, o perfil não conterá todas as informações necessárias para se conectar à VPN.

## <a name="create-the-profilexml-configuration-files"></a>Criar os arquivos de configuração do ProfileXML

Antes de concluir esta seção, certifique-se de ter criado e testado o modelo de conexão de VPN que a seção [criar um perfil de conexão de modelo manualmente](#manually-create-a-template-connection-profile) descreve. Teste a conexão VPN é necessário para garantir que o perfil contém todas as informações necessárias para se conectar à VPN.

O script do Windows PowerShell na listagem 1 cria dois arquivos na área de trabalho, e ambos contêm **EAPConfiguration** marcas com base no perfil de conexão de modelo criado anteriormente:

- **VPN_Profile.xml.** Esse arquivo contém a marcação XML necessária para configurar o nó do ProfileXML VPNv2 CSP. Use esse arquivo com os serviços de OMA-DM – compatível com o MDM, como o Intune.

- **VPN_Profile.ps1.** Esse arquivo é um script do Windows PowerShell que podem ser executados em computadores cliente para configurar o nó do ProfileXML no VPNv2 CSP. Você também pode configurar o CSP ao implantar esse script por meio do System Center Configuration Manager. É possível executar esse script em uma sessão de área de trabalho remota, incluindo uma sessão aprimorada do Hyper-V.

>[!IMPORTANT]
>Os comandos de exemplo a seguir exigem o Build do Windows 10 1607 ou posterior.

**Criar VPN_Profile.xml e VPN_Proflie.ps1**

1. Entre no computador cliente ingressado no domínio que contém o modelo de perfil VPN com o mesmo usuário que conta a seção [criar um perfil de conexão de modelo manualmente](#manually-create-a-template-connection-profile) descrito.

2. Cole a listagem 1 no ambiente de script integrado do Windows PowerShell (ISE) e personalizar os parâmetros descritos nos comentários. Esses são $Template, $ProfileName, $Servers, $DnsSuffix, $DomainName, $TrustedNetwork e $DNSServers. É uma descrição completa de cada configuração nos comentários.

3. Execute o script para gerar **VPN_Profile.xml** e **VPN_Profile.ps1** na área de trabalho.

#### <a name="listing-1-understanding-makeprofileps1"></a>Listagem 1. Noções básicas sobre MakeProfile.ps1

Esta seção explica o código de exemplo que você pode usar para obter uma compreensão de como criar um perfil de VPN, especificamente para configurar o ProfileXML VPNv2 CSP.

Depois de montar um script desse código de exemplo e execute o script, o script gera dois arquivos: VPN_Profile.XML e VPN_Profile.ps1. Use VPN_Profile.xml para configurar ProfileXML nos serviços MDM em conformidade com OMA DM, como o Microsoft Intune.

Use o **VPN_Profile.ps1** script no Windows PowerShell ou o System Center Configuration Manager para configurar ProfileXML na área de trabalho do Windows 10.

>[!NOTE]
>Para exibir o script de exemplo completo, consulte a seção [MakeProfile.ps1 Full Script](#makeprofileps1-full-script).

#### <a name="parameters"></a>Parâmetros

Configure os seguintes parâmetros:

**$Template**. O nome do modelo do qual recuperar a configuração do EAP.

**$ProfileName**. Identificador alfanumérico exclusivo para o perfil. O nome do perfil não deve incluir uma barra (/). Se o nome do perfil tem um espaço ou outros caracteres não alfanuméricos, devem ser escapado corretamente acordo com o padrão de codificação de URL.

**$Servers**. Endereço IP público ou roteável ou nome DNS para o gateway de VPN. Ele pode apontar para o externo IP de um gateway ou um IP virtual para um farm de servidores. Exemplos, 208.147.66.130 ou vpn.contoso.com.

**$DnsSuffix**. Especifica que uma ou mais vírgulas separadas sufixos DNS. O primeiro item na lista também é usado como o sufixo DNS específico da conexão primário para a Interface VPN. Toda a lista também será adicionada no SuffixSearchList.

**$DomainName**. Usado para indicar o namespace ao qual a política se aplica. Quando uma consulta de nome é emitida, o cliente DNS compara o nome da consulta para todos os namespaces em DomainNameInformationList para encontrar uma correspondência. Esse parâmetro pode ser um dos seguintes tipos:

- FQDN - nome de domínio totalmente qualificado
- Sufixo - um sufixo de domínio que será acrescentado à consulta de nome curto para a resolução DNS. Para especificar um sufixo, preceda um ponto (.) para o sufixo DNS.

**$DNSServers**. Lista de IP do servidor DNS separados por vírgulas de endereços a ser usado para o namespace.

**$TrustedNetwork**. Cadeia separada por vírgulas para identificar a rede confiável. VPN não será conectado automaticamente quando o usuário estiver em sua rede sem fio corporativa em que os recursos protegidos são diretamente acessíveis para o dispositivo.

A seguir estão os valores de exemplo para os parâmetros usados os comandos a seguir. Certifique-se de que você altere esses valores para o seu ambiente.

```xml
$TemplateName = 'Template'
$ProfileName = 'Contoso AlwaysOn VPN'
$Servers = 'vpn.contoso.com'
$DnsSuffix = 'corp.contoso.com'
$DomainName = '.corp.contoso.com'
$DNSServers = '10.10.0.2,10.10.0.3'
$TrustedNetwork = 'corp.contoso.com'
```

### <a name="prepare-and-create-the-profile-xml"></a>Preparar e criar o perfil XML

Os seguintes comandos de exemplo obtém configurações de EAP do perfil do modelo:

```xml
$Connection = Get-VpnConnection -Name $TemplateName
if(!$Connection)
{
$Message = "Unable to get $TemplateName connection profile: $_"
Write-Host "$Message"
exit
}
$EAPSettings= $Connection.EapConfigXmlStream.InnerXml
```

### <a name="create-the-profile-xml"></a>Criar o perfil XML

[!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]

```xml
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
```

### <a name="output-vpnprofilexml-for-intune"></a>VPN_Profile.xml de saída para o Intune

Você pode usar o comando de exemplo a seguir para salvar o arquivo XML do perfil:

```xml
$ProfileXML | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.xml')
```

### <a name="output-vpnprofileps1-for-the-desktop-and-system-center-configuration-manager"></a>VPN_Profile.ps1 de saída para a área de trabalho e o System Center Configuration Manager

O exemplo de código a seguir configura uma Conexão de VPN IKEv2 AlwaysOn usando o nó do ProfileXML no CSP de VPNv2.

Você pode usar esse script na área de trabalho do Windows 10 ou no System Center Configuration Manager.

### <a name="define-key-vpn-profile-parameters"></a>Definir chave parâmetros de perfil VPN

```xml
$Script = '$ProfileName = ''' + $ProfileName + '''
$ProfileNameEscaped = $ProfileName -replace ' ', '%20'
```

## <a name="define-vpn-profilexml"></a>Definir ProfileXML VPN

```xml
$ProfileXML = ''' + $ProfileXML + '''
```

### <a name="escape-special-characters-in-the-profile"></a>Escapar caracteres especiais no perfil

```xml
$ProfileXML = $ProfileXML -replace '<', '&lt;'
$ProfileXML = $ProfileXML -replace '>', '&gt;'
$ProfileXML = $ProfileXML -replace '"', '&quot;'
```

### <a name="define-wmi-to-csp-bridge-properties"></a>Definir propriedades de ponte de WMI para CSP

```xml
$nodeCSPURI = "./Vendor/MSFT/VPNv2"
$namespaceName = "root\cimv2\mdm\dmmap"
$className = "MDM_VPNv2_01"
```

### <a name="determine-user-sid-for-vpn-profile"></a>Determine a SID do usuário para o perfil VPN:

```xml
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
```

### <a name="define-wmi-session"></a>Defina a sessão de WMI:

```xml
$session = New-CimSession
$options = New-Object Microsoft.Management.Infrastructure.Options.CimOperationOptions
$options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Type", "PolicyPlatform_UserContext", $false)
$options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Id", "$SidValue", $false)
```

### <a name="detect-and-delete-previous-vpn-profile"></a>Detectar e excluir o perfil VPN anterior:

```xml
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
```

### <a name="create-the-vpn-profile"></a>Crie o perfil VPN:

```xml
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
```

### <a name="save-the-profile-xml-file"></a>Salve o arquivo XML de perfil

```xml
$Script | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.ps1')

$Message = "Successfully created VPN_Profile.xml and VPN_Profile.ps1 on the desktop."
Write-Host "$Message"
```

## <a name="makeprofileps1-full-script"></a>MakeProfile.ps1 Full Script

A maioria dos exemplos usa o cmdlet Set-WmiInstance Windows PowerShell para inserir ProfileXML em uma nova instância da classe WMI de MDM_VPNv2_01. 

No entanto, isso não funciona no System Center Configuration Manager porque você não pode executar o pacote no contexto dos usuários finais. Portanto, esse script usa o modelo de informação comum para criar uma sessão WMI no contexto do usuário e, em seguida, ele cria uma nova instância da classe WMI de MDM_VPNv2_01 nessa sessão. Essa classe WMI usa a ponte de WMI para o CSP para configurar o CSP de VPNv2. Portanto, ao adicionar a instância da classe, configurar o CSP. 

>[!IMPORTANT]
>Ponte do WMI para CSP requer direitos de administrador local, por design. Para implantar por perfis de VPN do usuário, você deverá usar o SCCM ou o MDM.

>[!NOTE]
>O script VPN_Profile.ps1 usa o SID do usuário atual para identificar o contexto do usuário. Como Nenhum SID está disponível em uma sessão de área de trabalho remota, o script não funciona em uma sessão de área de trabalho remota. Da mesma forma, ele não funciona em uma sessão aprimorada do Hyper-V. Se você estiver testando um remoto acesso VPN Always On em máquinas virtuais, desabilite a sessão avançada em suas VMs de cliente antes de executar esse script.

O script de exemplo a seguir inclui todos os exemplos de código das seções anteriores. Certifique-se de que você altere os valores de exemplo para valores que são apropriados para seu ambiente.
    
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

## <a name="configure-the-vpn-client-by-using-windows-powershell"></a>Configurar o cliente VPN usando o Windows PowerShell

Para configurar o CSP de VPNv2 em um computador de cliente do Windows 10, execute o script VPN_Profile.ps1 Windows PowerShell que você criou na [criar o perfil XML](#create-the-profile-xml) seção. Abra o Windows PowerShell como administrador; Caso contrário, você receberá um erro dizendo _acesso negado_.

Depois de executar VPN_Profile.ps1 para configurar o perfil VPN, você pode verificar a qualquer momento que ela foi bem-sucedida, executando o seguinte comando no ISE do Windows PowerShell:

```powershell
Get-WmiObject -Namespace root\cimv2\mdm\dmmap -Class MDM_VPNv2_01
```

**Resultados bem-sucedidos do cmdlet Get-WmiObject**

```powershell
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
```

A configuração do ProfileXML deve estar correta na estrutura, ortografia, configuração e, às vezes, diferenciar maiusculas e minúsculas. Se você vir algo diferente na estrutura à listagem 1, a marcação do ProfileXML provavelmente contém um erro.

Se você precisar solucionar problemas de marcação, é mais fácil colocá-lo em um editor de XML que solucionar problemas-lo no ISE do Windows PowerShell. Em ambos os casos, começar com a versão mais simples do perfil e adicionar componentes de volta um de cada vez até que o problema ocorre novamente.

## <a name="configure-the-vpn-client-by-using-system-center-configuration-manager"></a>Configurar o cliente VPN usando o System Center Configuration Manager

No System Center Configuration Manager, você pode implantar perfis VPN usando o nó do ProfileXML CSP, exatamente como você fez no Windows PowerShell. Aqui, você pode usar o script VPN_Profile.ps1 Windows PowerShell que você criou na seção [criar os arquivos de configuração do ProfileXML](#create-the-profilexml-configuration-files).

Para usar o System Center Configuration Manager para implantar um perfil de acesso remoto sempre em VPN em computadores cliente com Windows 10, você deve iniciar com a criação de um grupo de computadores ou usuários aos quais você implantar o perfil. Nesse cenário, crie um grupo de usuários para implantar o script de configuração.

### <a name="create-a-user-group"></a>Criar um grupo de usuários

1.  No console do Configuration Manager, abra ativos e conformidade\\coleções de usuários.

2.  No **Home** faixa de opções, no **Create** , clique em **Criar coleção do usuário**.

3.  Na página geral, conclua as seguintes etapas:

    a. Na **nome**, digite **usuários da VPN**.

    b. Clique em **navegue**, clique em **todos os usuários** e clique em **Okey**.

    c. Clique em **Avançar**.

4.  Na página regras de associação, conclua as seguintes etapas:

    a.  Na **regras de associação**, clique em **Adicionar regra**e clique em **regra direta**. Neste exemplo, você está adicionando usuários individuais para a coleção de usuários. No entanto, você pode usar uma regra de consulta para adicionar usuários a esta coleção dinamicamente para uma implantação de grande escala.

    b.  Na página **Bem-vindo**, clique em **Avançar**.

    c.  Na pesquisa para a página de recursos, em **valor**, digite o nome do usuário que você deseja adicionar. O nome do recurso inclui o domínio do usuário. Para incluir resultados com base em uma correspondência parcial, insira o **%** caractere em ambas as extremidades de seu critério de pesquisa. Por exemplo, para localizar todos os usuários que contém a cadeia de caracteres "lori", digite **% % lori**. Clique em **Avançar**.

    d.  Na página Selecionar recursos, selecione os usuários que você deseja adicionar ao grupo e, em seguida, clique em **próxima**.

    e.  Na página Resumo, clique em **próxima**.

    f.  Na página conclusão, clique em **fechar**.

6.  Na página de regras de associação do Assistente para criar coleção de usuário, clique em **próxima**.

7.  Na página Resumo, clique em **próxima**.

8.  Na página conclusão, clique em **fechar**.

Depois de criar o grupo de usuários para receber o perfil de VPN, você pode criar um pacote e programa para implantar o script de configuração do Windows PowerShell que você criou na seção [criar os arquivos de configuração do ProfileXML](#create-the-profilexml-configuration-files).

### <a name="create-a-package-containing-the-profilexml-configuration-script"></a>Criar um pacote que contém o script de configuração do ProfileXML

1.  Hospede o script VPN_Profile.ps1 em um compartilhamento de rede que a conta de computador do servidor de site pode acessar.

2.  No console do Configuration Manager, abra **biblioteca de Software\\gerenciamento de aplicativos\\pacotes**.

3.  No **Home** faixa de opções, na **Create** , clique em **criar pacote** para iniciar o Assistente de programa e criar pacote.

4.  Na página do pacote, conclua as seguintes etapas:

    a. Na **nome**, digite **Windows 10 sempre no perfil de VPN**.

    b. Selecione o **esse pacote contém arquivos de origem** caixa de seleção e, em seguida, clique em **procurar**.

    c. Na caixa de diálogo Definir pasta de origem, clique em **navegue**, selecione o compartilhamento de arquivos que contém VPN_Profile.ps1 e clique em **Okey**.
        Verifique se que você selecionar um caminho de rede, não um caminho local. Em outras palavras, o caminho deve ser algo como  *\\fileserver\\vpnscript*, e não *c:\\vpnscript*.

1.  Clique em **Avançar**.

2.  Na página tipo de programa, clique em **próxima**.

3.  Na página do programa padrão, conclua as seguintes etapas:

    a.  Na **nome**, digite **Script de perfil de VPN**.

    b.  Na **linha de comando**, digite **PowerShell.exe - ExecutionPolicy Bypass - arquivo "VPN_Profile.ps1"** .

    c.  Na **modo de execução**, clique em **executar com direitos administrativos**.

    d.  Clique em **Avançar**.

4.  Na página de requisitos, conclua as seguintes etapas:

    a.  Selecione **este programa pode ser executado somente nas plataformas especificadas**.

    b.  Selecione o **todos os Windows 10 (32 bits)** e **todos os Windows 10 (64 bits)** caixas de seleção.

    c.  Na **espaço em disco estimado**, digite **1**.

    d.  Na **máximo permitido (minutos) de tempo de execução**, digite **15**.

    e.  Clique em **Avançar**.

5.  Na página Resumo, clique em **próxima**.

6.  Na página conclusão, clique em **fechar**.

Com o pacote e programa criado, você precisará implantá-lo para o **usuários da VPN** grupo.

### <a name="deploy-the-profilexml-configuration-script"></a>Implantar o script de configuração do ProfileXML

1.  No console do Configuration Manager, abra a biblioteca de Software\\gerenciamento de aplicativos\\pacotes.

2.  Na **pacotes**, clique em **Windows 10 sempre no perfil de VPN**.

3.  Sobre o **programas** guia, na parte inferior do painel de detalhes, clique com botão direito **Script de perfil de VPN**, clique em **propriedades**e conclua as seguintes etapas:

    a.  Sobre o **Advanced** guia de **quando esse programa é atribuído a um computador**, clique em **uma vez para cada usuário que fizer logon**.

    b.  Clique em **OK**.

4.  Clique com botão direito **Script de perfil de VPN** e clique em **implantar** para iniciar o Assistente de implantação de Software.

5.  Na página geral, conclua as seguintes etapas:

    a.  Ao lado **coleta**, clique em **procurar**.

    b.  No **tipos de coleção** lista (canto superior esquerdo), clique em **coleções de usuários**.

    c.  Clique em **usuários de VPN**e clique em **Okey**.

    d.  Clique em **Avançar**.

6.  Na página de conteúdo, conclua as seguintes etapas:

    a.  Clique em **Add**e clique em **ponto de distribuição**.

    b.  Na **pontos de distribuição disponíveis**, selecione os pontos de distribuição para o qual você deseja distribuir o script de configuração do ProfileXML e, em seguida, clique em **Okey**.

    c.  Clique em **Avançar**.

7.  Na página de configurações de implantação, clique em **próxima**.

8.  Na página agendamento, conclua as seguintes etapas:

    a.  Clique em **New** para abrir a caixa de diálogo de agendamento de atribuição.

    b.  Clique em **atribuir imediatamente após este evento**e clique em **Okey**.

    c.  Clique em **Avançar**.

9.  Na página da experiência do usuário, conclua as seguintes etapas:

    1.  Selecione o **instalação do Software** caixa de seleção.

    2.  Clique em **resumo**.

10. Na página Resumo, clique em **próxima**.

11. Na página conclusão, clique em **fechar**.

Com o script de configuração do ProfileXML implantado, entre um computador de cliente do Windows 10 com a conta de usuário que você selecionou ao criar a coleção de usuários. Verifique a configuração do cliente VPN.

>[!NOTE]
>O script VPN_Profile.ps1 não funciona em uma sessão de área de trabalho remota. Da mesma forma, ele não funciona em uma sessão aprimorada do Hyper-V. Se você estiver testando um remoto acesso VPN Always On em máquinas virtuais, desabilite a sessão avançada em suas VMs de cliente antes de continuar.

### <a name="verify-the-configuration-of-the-vpn-client"></a>Verifique a configuração do cliente VPN

1.  No painel de controle, sob **System\\Security**, clique em **Configuration Manager**. 

2.  Na caixa de diálogo Propriedades do Configuration Manager sobre as **ações** guia, conclua as seguintes etapas:

    a.  Clique em **ciclo de avaliação e recuperação de política de máquina**, clique em **Run Now**e clique em **Okey**.

    b.  Clique em **ciclo de avaliação e de recuperação de política de usuário**, clique em **Run Now**e clique em **Okey**.

    c.  Clique em **OK**.

3.  Feche o painel de controle.

Você deve ver o novo perfil VPN em breve.

## <a name="configure-the-vpn-client-by-using-intune"></a>Configurar o cliente VPN usando o Intune

Para usar o Intune para implantar o Windows 10 remoto acesso sempre em perfis de VPN, você pode configurar o nó do ProfileXML CSP usando o perfil VPN que você criou na seção [criar os arquivos de configuração do ProfileXML](#create-the-profilexml-configuration-files), ou você pode usar o protocolo EAP base Exemplo de XML fornecido abaixo.

>[!NOTE]
>Agora, o Intune usa grupos do Azure AD. Se o Azure AD Connect sincronizado o grupo de usuários de VPN do local para o Azure AD e os usuários são atribuídos ao grupo de usuários de VPN, você está pronto para continuar.

Crie a política de configuração de dispositivo VPN para configurar os computadores de cliente do Windows 10 para todos os usuários adicionados ao grupo. Como o modelo do Intune fornece parâmetros VPN, copie somente o \<EapHostConfig > \</EapHostConfig > parte do arquivo VPN_ProfileXML.

### <a name="create-the-always-on-vpn-configuration-policy"></a>Criar a política de configuração de VPN Always On

1.  Entrar a [portal do Azure](https://portal.azure.com/).

2.  Vá para **Intune** > **configuração do dispositivo** > **perfis**.

3.  Clique em **criação de perfil** para iniciar o assistente Criar perfil.

4.  Insira um **nome** para o perfil de VPN e (opcionalmente) uma descrição.

1.   Sob **plataforma**, selecione **Windows 10 ou posterior**e escolha **VPN** suspenso tipo de perfil.

     >[!TIP]
     >Se você estiver criando um profileXML VPN personalizado, consulte [ProfileXML aplicam-se usando o Intune](https://docs.microsoft.com/windows/security/identity-protection/vpn/vpn-profile-options#apply-profilexml-using-intune) para obter instruções.

2. Sob o **VPN de Base** guia, verifique ou defina as seguintes configurações:

    - **Nome da Conexão:** Insira o nome da conexão de VPN como ele aparece no computador cliente na **VPN** guia sob **configurações**, por exemplo, _Contoso AutoVPN_.  
    
    - **servidores:** Adicione um ou mais servidores VPN, clicando em **adicionar.**
    
    - **Descrição** e **endereço IP ou FQDN:** Insira a descrição e o endereço IP ou FQDN do servidor VPN. Esses valores devem estar alinhadas com o nome do assunto no certificado de autenticação do servidor VPN. 
    
    - **Servidor padrão:** Se esse for o servidor VPN padrão, definido como **verdadeira**. Isso permite que este servidor como o servidor padrão que os dispositivos usam para estabelecer a conexão. 
    
    - **Tipo de Conexão:** Definido como **IKEv2**.  
    
    - **Sempre ativo:** Definido como **habilitar** para se conectar à VPN automaticamente no sign-in e permanecer conectado até que o usuário se desconecta manualmente.
    
    - **Lembrar credenciais a cada logon**:  Valor booliano (true ou false) para armazenar em cache as credenciais. Se definido como true, credenciais é armazenadas em cache sempre que possível.

3.  Copie a seguinte cadeia de caracteres XML em um editor de texto:
 
    [!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]
    
    
    ```XML
    <EapHostConfig xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><EapMethod><Type xmlns="https://www.microsoft.com/provisioning/EapCommon">25</Type><VendorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorId><VendorType xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorType><AuthorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</AuthorId></EapMethod><Config xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>25</Type><EapType xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV1"><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><FastReconnect>true</FastReconnect><InnerEapOptional>false</InnerEapOptional><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>13</Type><EapType xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV1"><CredentialsSource><CertificateStore><SimpleCertSelection>true</SimpleCertSelection></CertificateStore></CredentialsSource><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><DifferentUsername>false</DifferentUsername><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</AcceptServerName></EapType></Eap><EnableQuarantineChecks>false</EnableQuarantineChecks><RequireCryptoBinding>false</RequireCryptoBinding><PeapExtensions><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</AcceptServerName></PeapExtensions></EapType></Eap></Config></EapHostConfig>
    ```

4.  Substitua os  **\<TrustedRootCA > 5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1C c2 68 ser 4b <\/TrustedRootCA >** do exemplo com a impressão digital do certificado de sua autoridade de certificação de raiz no local em ambos os locais.
  
    >[!Important]
    >Não use a impressão digital de exemplo na \<TrustedRootCA >\</TrustedRootCA > seção abaixo.  O TrustedRootCA deve ser a impressão digital do certificado da autoridade de certificação local raiz que emitiu o certificado de autenticação de servidor para servidores NPS e RRAS. **Isso não deve ser o certificado de raiz de nuvem, nem intermediário emissora da autoridade de certificação impressão digital do certificado**.

5.  Substitua os  **\<ServerNames > NPS.contoso.com\</ServerNames >** no exemplo de XML com o FQDN do NPS associados ao domínio em que a autenticação é feita. 

6.  Copie a cadeia de caracteres XML revisada e cole a **Xml EAP** caixa sob a guia de VPN de Base e clique em **Okey**.
    Uma política de sempre na configuração do dispositivo VPN usando o EAP é criada no Intune.


### <a name="sync-the-always-on-vpn-configuration-policy-with-intune"></a>Sincronizar a política de configuração de VPN Always On com o Intune

Para testar a política de configuração, entre em um computador de cliente do Windows 10 como o usuário é adicionado à **sempre em usuários da VPN** grupo e, em seguida, sincronizar com o Intune.

1.  No menu Iniciar, clique em **configurações**.

2.  Em configurações, clique em **contas**e clique em **acessar trabalho ou escola**.

3.  Clique no perfil MDM e, em seguida, clique em **Info**.

4.  Clique em **sincronização** para forçar uma avaliação de política do Intune e a recuperação.

5.  Feche as configurações. Após a sincronização, você pode ver o perfil VPN disponível no computador.

## <a name="next-steps"></a>Próximas etapas

Você concluiu implantação VPN Always On.  Para outros recursos que você pode configurar, consulte a tabela a seguir:

|Se você quiser...  |Em seguida, consulte...  |
|---------|---------|
|Configurar o acesso condicional para VPN    |[Etapa 7. (Opcional) Configurar o acesso condicional para conectividade VPN usando o Azure AD](../../ad-ca-vpn-connectivity-windows10.md): Nesta etapa, você pode ajustar como VPN os usuários autorizados acessam seus recursos usando o [acesso condicional do Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). Com acesso condicional do Azure AD para conectividade de rede virtual privada (VPN), você pode ajudar a proteger as conexões VPN. O Acesso Condicional é um mecanismo de avaliação com base em política que permite que você crie regras de acesso para qualquer aplicativo conectado ao Azure Active Directory (Azure AD).         |
|Saiba mais sobre os recursos avançados de VPN  |[Recursos VPN avançados](always-on-vpn-adv-options.md#advanced-vpn-features): Esta página fornece diretrizes sobre como habilitar filtros de tráfego de VPN, como configurar conexões VPN automáticas usando gatilhos de aplicativo e como configurar o NPS para permitir somente conexões VPN de clientes que usam certificados emitidos pelo AD do Azure.        |
