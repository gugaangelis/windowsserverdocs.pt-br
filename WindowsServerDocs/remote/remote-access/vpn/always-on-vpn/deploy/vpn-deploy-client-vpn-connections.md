---
title: Configurar conexões de VPN Always On em cliente do Windows 10
description: Nesta etapa, você aprenderá sobre as opções e o esquema do ProfileXML e configurará os computadores cliente do Windows 10 para se comunicar com essa infraestrutura com uma conexão VPN.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 05/29/2018
ms.assetid: d165822d-b65c-40a2-b440-af495ad22f42
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.reviewer: deverette
ms.openlocfilehash: c853926157a4efa414c56d97affa0a1d2faad629
ms.sourcegitcommit: 1bc3c229e9688ac741838005ec4b88e8f9533e8a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/18/2019
ms.locfileid: "68314355"
---
# <a name="step-6-configure-windows-10-client-always-on-vpn-connections"></a>Etapa 6. Configurar conexões VPN Always On cliente do Windows 10

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Anterior** Etapa 5. Definir as configurações de DNS e de firewall](vpn-deploy-dns-firewall.md)<br>
- [**Última** Etapa 7. Adicional Acesso condicional para conectividade VPN usando o Azure AD](../../ad-ca-vpn-connectivity-windows10.md)

Nesta etapa, você aprenderá sobre as opções e o esquema do ProfileXML e configurará os computadores cliente do Windows 10 para se comunicar com essa infraestrutura com uma conexão VPN.

Você pode configurar o cliente VPN Always On por meio do PowerShell, SCCM ou Intune. Todos os três exigem um perfil de VPN XML para definir as configurações de VPN apropriadas. É possível automatizar o registro do PowerShell para organizações sem o SCCM ou o Intune.

>[!NOTE]
>Política de Grupo não inclui modelos administrativos para configurar o cliente VPN Always On acesso remoto do Windows 10.  No entanto, você pode usar scripts de logon.

## <a name="profilexml-overview"></a>Visão geral do ProfileXML

ProfileXML é um nó de URI dentro do CSP VPNv2. Em vez de configurar cada nó do VPNv2 CSP individualmente, como gatilhos, listas de rotas e protocolos de autenticação, use este nó para configurar um cliente de VPN do Windows 10 fornecendo todas as configurações como um único bloco XML para um único nó CSP. O esquema ProfileXML corresponde ao esquema dos nós do CSP do VPNv2 quase de forma idêntica, mas alguns termos são ligeiramente diferentes.

Você usa ProfileXML em todos os métodos de entrega que esta implantação descreve, incluindo o Windows PowerShell, System Center Configuration Manager e Intune. Há duas maneiras de configurar o nó ProfileXML VPNv2 CSP nesta implantação:

- **OMA-DM**. Uma maneira é usar um provedor de MDM usando o OMA-DM, conforme discutido anteriormente na seção [nós do CSP VPNv2](../always-on-vpn-technology-overview.md#vpnv2-csp-nodes). Usando esse método, você pode facilmente inserir a marcação XML de configuração do perfil de VPN no nó do CSP ProfileXML ao usar o Intune.

- **Instrumentação de gerenciamento do Windows (WMI) para a ponte do CSP**. O segundo método de configuração do nó do CSP ProfileXML é usar a ponte WMI-to-CSP — uma classe WMI chamada **MDM_VPNv2_01**— que pode acessar o CSP VPNv2 e o nó ProfileXML. Quando você cria uma nova instância dessa classe WMI, o WMI usa o CSP para criar o perfil de VPN ao usar o Windows PowerShell e o System Center Configuration Manager.

Embora esses métodos de configuração sejam diferentes, ambos exigem um perfil de VPN XML formatado corretamente. Para usar a configuração do CSP ProfileXML VPNv2, você constrói XML usando o esquema ProfileXML para configurar as marcas necessárias para o cenário de implantação simples. Para obter mais informações, consulte [XSD ProfileXML](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-profile-xsd).

Abaixo, você encontrará cada uma das configurações necessárias e sua marca ProfileXML correspondente. Você define cada configuração em uma marca específica dentro do esquema ProfileXML e nem todas elas são encontradas no perfil nativo. Para um posicionamento de marca adicional, consulte o esquema ProfileXML.

[!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]

**Tipo de conexão:** IKEv2 nativo

Elemento ProfileXML:

```xml
<NativeProtocolType>IKEv2</NativeProtocolType>
```

**Zona** Túnel dividido

Elemento ProfileXML:

```xml
<RoutingPolicyType>SplitTunnel</RoutingPolicyType>
```

**Resolução de nomes:** Lista de informações de nome de domínio e sufixo DNS

Elementos ProfileXML:

```xml
<DomainNameInformation>
<DomainName>.corp.contoso.com</DomainName>
<DnsServers>10.10.1.10,10.10.1.50</DnsServers>
</DomainNameInformation>

<DnsSuffix>corp.contoso.com</DnsSuffix>
```

**Disparar** Detecção de Always On e de rede confiável

Elementos ProfileXML:

```xml
<AlwaysOn>true</AlwaysOn>
<TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection>
```

**Authentication** PEAP-TLS com certificados de usuário protegidos por TPM

Elementos ProfileXML:

```xml
<Authentication>
<UserMethod>Eap</UserMethod>
<Eap>
<Configuration>...</Configuration>
</Eap>
</Authentication>
```

Você pode usar marcas simples para configurar alguns mecanismos de autenticação de VPN. No entanto, o EAP e o PEAP estão mais envolvidos. A maneira mais fácil de criar a marcação XML é configurar um cliente VPN com suas configurações de EAP e, em seguida, exportar essa configuração para XML.

Para obter mais informações sobre as configurações de EAP, consulte [configuração de EAP](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/eap-configuration).

## <a name="manually-create-a-template-connection-profile"></a>Criar manualmente um perfil de conexão de modelo

Nesta etapa, você usa o protocolo PEAP para proteger a comunicação entre o cliente e o servidor. Ao contrário de um nome de usuário e senha simples, essa conexão requer uma seção EAPConfiguration exclusiva no perfil de VPN para funcionar.

Em vez de descrever como criar a marcação XML do zero, você usa as configurações no Windows para criar um perfil de VPN de modelo. Depois de criar o perfil de VPN de modelo, use o Windows PowerShell para consumir a parte EAPConfiguration do modelo para criar o ProfileXML final que você implanta posteriormente na implantação.

### <a name="record-nps-certificate-settings"></a>Registrar configurações de certificado do NPS

Antes de criar o modelo, tome nota do nome de host ou FQDN (nomes de domínio totalmente qualificado) do servidor NPS do certificado do servidor e o nome da autoridade de certificação que emitiu o certificado.

**Procedure**

1. No servidor NPS, abra servidor de políticas de rede.

2. No console do NPS, em políticas, clique em **políticas de rede**.

3. Clique com o botão direito do mouse em **conexões de VPN (rede virtual privada)** e clique em **Propriedades**.

4. Clique na guia **restrições** e clique em **métodos de autenticação**.

5. Em tipos de EAP, **clique em Microsoft: EAP protegido (PEAP)** e clique em **Editar**.

6. Registre os valores para o **certificado emitido para** o e o **emissor**.

    Você usa esses valores na configuração de modelo de VPN futura. Por exemplo, se o FQDN do servidor for nps01.corp.contoso.com e o hostname for NPS01, o nome do certificado será baseado no FQDN ou no nome DNS do servidor, por exemplo, nps01.corp.contoso.com.

7. Cancele a caixa de diálogo Editar propriedades EAP protegidas.

8. Cancele a caixa de diálogo Propriedades de conexões VPN (rede virtual privada).

9. Feche o servidor de políticas de rede.

>[!NOTE]
>Se você tiver vários servidores NPS, conclua estas etapas em cada um para que o perfil VPN possa verificar se cada um deles deve ser usado.

### <a name="configure-the-template-vpn-profile-on-a-domain-joined-client-computer"></a>Configurar o perfil de VPN de modelo em um computador cliente ingressado no domínio

Agora que você tem as informações necessárias, configure o perfil de VPN de modelo em um computador cliente ingressado no domínio. O tipo de conta de usuário que você usa (ou seja, usuário padrão ou administrador) para essa parte do processo não importa.

No entanto, se você não tiver reiniciado o computador desde a configuração do registro automático de certificado, faça isso antes de configurar a conexão VPN de modelo para garantir que você tenha um certificado utilizável registrado nele.

>[!NOTE]
>Não é possível adicionar manualmente nenhuma propriedade avançada de VPN, como regras de NRPT, Always On, detecção de rede confiável, etc. Na próxima etapa, você cria uma conexão VPN de teste para verificar a configuração do servidor VPN e pode estabelecer uma conexão VPN com o servidor.

**Criar manualmente uma única conexão VPN de teste**

1.  Entre em um computador cliente ingressado no domínio como um membro do grupo de **usuários VPN** .

2.  No menu Iniciar, digite **VPN**e pressione Enter.

3.  No painel de detalhes, clique em **Adicionar uma conexão VPN**.

4.  Na lista provedor de VPN, clique em **Windows (interno)** .

5.  Em nome da conexão, digite **modelo**.

6.  Em nome ou endereço do servidor, digite o FQDN **externo** do servidor VPN (por exemplo, **VPN.contoso.com**).

7.  Clique em **Salvar**.

8.  Em configurações relacionadas, clique em **alterar opções de adaptador**.

9.  Clique com o botão direito do mouse em **modelo**e clique em **Propriedades**.

10. Na guia **segurança** , em **tipo de VPN**, clique em **IKEv2**.

11. Em criptografia de dados, clique em **criptografia de força máxima**.

12. Clique em **usar protocolo EAP (Extensible Authentication Protocol)** ; em seguida, em **usar EAP (Extensible Authentication Protocol)** , **clique em Microsoft: EAP protegido (PEAP) (criptografia habilitada**).

13. Clique em **Propriedades** para abrir a caixa de diálogo Propriedades EAP protegidas e conclua as seguintes etapas:

    a. Na caixa **conectar-se a esses servidores** , digite o nome do servidor NPS que você recuperou das configurações de autenticação do servidor NPS anteriormente nesta seção (por exemplo, NPS01).

    >[!NOTE]
    >O nome do servidor que você digitar deve corresponder ao nome no certificado. Você recuperou esse nome anteriormente nesta seção. Se o nome não corresponder, a conexão falhará, informando que "a conexão foi impedida devido a uma política configurada no servidor RAS/VPN".

    b.  Em autoridades de certificação raiz confiáveis, selecione a AC raiz que emitiu o certificado do servidor NPS (por exemplo, contoso-CA).

    c.  Em notificações antes de se conectar, clique em **não pedir ao usuário para autorizar novos servidores ou autoridades de certificação confiáveis**.

    d.  Em selecionar método de autenticação, clique em **cartão inteligente ou outro certificado**e clique em **Configurar**. A caixa de diálogo Propriedades do cartão inteligente ou outro certificado é aberta.

    e.  Clique em **usar um certificado neste computador**.

    f.  Na caixa conectar-se a esses servidores, insira o nome do servidor NPS que você recuperou das configurações de autenticação do servidor NPS nas etapas anteriores.

    g.  Em autoridades de certificação raiz confiáveis, selecione a AC raiz que emitiu o certificado do servidor NPS.

    h.  Marque a caixa de seleção **não solicitar que o usuário autorize novos servidores ou autoridades de certificação confiáveis** .

    i.  Clique em **OK** para fechar a caixa de diálogo Propriedades do cartão inteligente ou outro certificado.

    j.  Clique em **OK** para fechar a caixa de diálogo Propriedades EAP protegidas.

14. Clique em **OK** para fechar a caixa de diálogo Propriedades do modelo.

15. Feche a janela conexões de rede.

16. Em configurações, teste a VPN clicando em **modelo**e, em seguida, clicando em **conectar**.

>[!IMPORTANT]
>Verifique se a conexão de VPN de modelo com o servidor VPN foi bem-sucedida. Isso garante que as configurações de EAP estejam corretas antes de você usá-las no exemplo a seguir. Você deve se conectar pelo menos uma vez antes de continuar; caso contrário, o perfil não conterá todas as informações necessárias para se conectar à VPN.

## <a name="create-the-profilexml-configuration-files"></a>Criar os arquivos de configuração do ProfileXML

Antes de concluir esta seção, verifique se você criou e testou a conexão de VPN de modelo que a seção [criar manualmente um perfil de conexão de modelo](#manually-create-a-template-connection-profile) descreve. O teste da conexão VPN é necessário para garantir que o perfil contenha todas as informações necessárias para se conectar à VPN.

O script do Windows PowerShell na Listagem 1 cria dois arquivos na área de trabalho, ambos contendo marcas **EAPConfiguration** com base no perfil de conexão de modelo criado anteriormente:

- **VPN_Profile. xml.** Esse arquivo contém a marcação XML necessária para configurar o nó ProfileXML no CSP do VPNv2. Use esse arquivo com serviços de MDM compatíveis com OMA-DM, como o Intune.

- **VPN_Profile. ps1.** Esse arquivo é um script do Windows PowerShell que você pode executar em computadores cliente para configurar o nó ProfileXML no CSP do VPNv2. Você também pode configurar o CSP implantando esse script por meio de System Center Configuration Manager. Você não pode executar esse script em uma sessão de Área de Trabalho Remota, incluindo uma sessão avançada do Hyper-V.

>[!IMPORTANT]
>Os comandos de exemplo abaixo exigem o Windows 10 Build 1607 ou posterior.

**Criar VPN_Profile. xml e VPN_Proflie. ps1**

1. Entre no computador cliente ingressado no domínio que contém o perfil de VPN de modelo com a mesma conta de usuário em que a seção [cria manualmente um perfil de conexão de modelo](#manually-create-a-template-connection-profile) descrito.

2. Cole a listagem 1 no ISE (ambiente de script integrado) do Windows PowerShell e personalize os parâmetros descritos nos comentários. São $Template, $ProfileName, $Servers, $DnsSuffix, $DomainName, $TrustedNetwork e $DNSServers. Uma descrição completa de cada configuração está nos comentários.

3. Execute o script para gerar **VPN_Profile. xml** e **VPN_Profile. ps1** na área de trabalho.

#### <a name="listing-1-understanding-makeprofileps1"></a>Listagem 1. Entendendo MakeProfile. ps1

Esta seção explica o código de exemplo que você pode usar para entender como criar um perfil VPN, especificamente para configurar o ProfileXML no CSP do VPNv2.

Depois de montar um script desse código de exemplo e executar o script, o script gera dois arquivos: VPN_Profile. xml e VPN_Profile. ps1. Use VPN_Profile. xml para configurar o ProfileXML em serviços de MDM compatíveis com OMA-DM, como Microsoft Intune.

Use o script **VPN_Profile. ps1** no Windows PowerShell ou System Center Configuration Manager para configurar o ProfileXML na área de trabalho do Windows 10.

>[!NOTE]
>Para exibir o script de exemplo completo, consulte a seção [MakeProfile. ps1 full script](#makeprofileps1-full-script).

#### <a name="parameters"></a>Parâmetros

Configure os seguintes parâmetros:

**$Template**. O nome do modelo do qual recuperar a configuração de EAP.

**$ProfileName**. Identificador alfanumérico exclusivo para o perfil. O nome do perfil não deve incluir uma barra (/). Se o nome do perfil tiver um espaço ou outro caractere não alfanumérico, ele deverá ter um escape correto de acordo com o padrão de codificação de URL.

**$Servers**. Endereço IP público ou roteável ou nome DNS para o gateway de VPN. Ele pode apontar para o IP externo de um gateway ou um IP virtual para um farm de servidores. Exemplos, 208.147.66.130 ou vpn.contoso.com.

**$DnsSuffix**. Especifica um ou mais sufixos DNS separados por vírgulas. A primeira na lista também é usada como o sufixo DNS específico da conexão primária para a interface VPN. A lista inteira também será adicionada ao SuffixSearchList.

**$DomainName**. Usado para indicar o namespace ao qual a política se aplica. Quando uma consulta de nome é emitida, o cliente DNS compara o nome na consulta a todos os namespaces em DomainNameInformationList para encontrar uma correspondência. Esse parâmetro pode ser um dos seguintes tipos:

- FQDN-nome de domínio totalmente qualificado
- Sufixo-um sufixo de domínio que será anexado à consulta de nome curto para resolução de DNS. Para especificar um sufixo, preceda um ponto (.) para o sufixo DNS.

**$DNSServers**. Lista de endereços IP do servidor DNS separados por vírgula a serem usados para o namespace.

**$TrustedNetwork**. Cadeia de caracteres separada por vírgulas para identificar a rede confiável. A VPN não se conecta automaticamente quando o usuário está na rede sem fio corporativa em que os recursos protegidos estão diretamente acessíveis para o dispositivo.

Veja a seguir os valores de exemplo para parâmetros usados nos comandos abaixo. Certifique-se de alterar esses valores para o seu ambiente.

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

Os comandos de exemplo a seguir obtêm as configurações de EAP do perfil de modelo:

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

### <a name="output-vpnprofilexml-for-intune"></a>Saída de VPN_Profile. xml para o Intune

Você pode usar o seguinte comando de exemplo para salvar o arquivo XML do perfil:

```xml
$ProfileXML | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.xml')
```

### <a name="output-vpnprofileps1-for-the-desktop-and-system-center-configuration-manager"></a>Saída de VPN_Profile. ps1 para desktop e System Center Configuration Manager

O código de exemplo a seguir configura uma conexão VPN IKEv2 do AlwaysOn usando o nó ProfileXML no CSP VPNv2.

Você pode usar esse script na área de trabalho do Windows 10 ou no System Center Configuration Manager.

### <a name="define-key-vpn-profile-parameters"></a>Definir parâmetros chave de perfil VPN

```xml
$Script = '$ProfileName = ''' + $ProfileName + ''''
$ProfileNameEscaped = $ProfileName -replace ' ', '%20'
```

## <a name="define-vpn-profilexml"></a>Definir ProfileXML VPN

```xml
$ProfileXML = ''' + $ProfileXML + '''
```

### <a name="escape-special-characters-in-the-profile"></a>Caracteres especiais de escape no perfil

```xml
$ProfileXML = $ProfileXML -replace '<', '&lt;'
$ProfileXML = $ProfileXML -replace '>', '&gt;'
$ProfileXML = $ProfileXML -replace '"', '&quot;'
```

### <a name="define-wmi-to-csp-bridge-properties"></a>Definir propriedades de ponte WMI para CSP

```xml
$nodeCSPURI = "./Vendor/MSFT/VPNv2"
$namespaceName = "root\cimv2\mdm\dmmap"
$className = "MDM_VPNv2_01"
```

### <a name="determine-user-sid-for-vpn-profile"></a>Determinar o SID de usuário para o perfil VPN:

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

### <a name="define-wmi-session"></a>Definir sessão WMI:

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

### <a name="create-the-vpn-profile"></a>Criar o perfil VPN:

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
Write-Host "$Message"
```

### <a name="save-the-profile-xml-file"></a>Salvar o arquivo XML do perfil

```xml
$Script | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.ps1')

$Message = "Successfully created VPN_Profile.xml and VPN_Profile.ps1 on the desktop."
Write-Host "$Message"
```

## <a name="makeprofileps1-full-script"></a>Script completo MakeProfile. ps1

A maioria dos exemplos usa o cmdlet Set-WmiInstance do Windows PowerShell para inserir ProfileXML em uma nova instância da classe WMI MDM_VPNv2_01. 

No entanto, isso não funciona em System Center Configuration Manager porque você não pode executar o pacote no contexto dos usuários finais. Portanto, esse script usa o modelo CIM para criar uma sessão WMI no contexto do usuário e, em seguida, cria uma nova instância da classe WMI MDM_VPNv2_01 nessa sessão. Essa classe WMI usa a ponte WMI para CSP para configurar o CSP VPNv2. Portanto, ao adicionar a instância de classe, você configura o CSP. 

>[!IMPORTANT]
>A ponte WMI para CSP requer direitos de administrador local, por design. Para implantar perfis VPN por usuário, você deve usar o SCCM ou MDM.

>[!NOTE]
>O script VPN_Profile. ps1 usa o SID do usuário atual para identificar o contexto do usuário. Como nenhum SID está disponível em uma sessão de Área de Trabalho Remota, o script não funciona em uma sessão de Área de Trabalho Remota. Da mesma forma, ele não funciona em uma sessão avançada do Hyper-V. Se você estiver testando um acesso remoto Always On VPN em máquinas virtuais, desabilite a sessão avançada em suas VMs de cliente antes de executar esse script.

O script de exemplo a seguir inclui todos os exemplos de código de seções anteriores. Certifique-se de alterar os valores de exemplo para valores que são apropriados para o seu ambiente.
    
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
    
    $ProfileXML = ''' + $ProfileXML + ''''
    
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
    Write-Host "$Message"
    
    $Script | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.ps1')
    
    $Message = "Successfully created VPN_Profile.xml and VPN_Profile.ps1 on the desktop."
    Write-Host "$Message"
   ```

## <a name="configure-the-vpn-client-by-using-windows-powershell"></a>Configurar o cliente VPN usando o Windows PowerShell

Para configurar o CSP VPNv2 em um computador cliente com Windows 10, execute o script do Windows PowerShell VPN_Profile. ps1 que você criou na seção [criar o perfil XML](#create-the-profile-xml) . Abra o Windows PowerShell como administrador; caso contrário, você receberá um erro dizendo, _acesso negado_.

Depois de executar VPN_Profile. ps1 para configurar o perfil VPN, você pode verificar a qualquer momento que ele foi bem-sucedido executando o seguinte comando no ISE do Windows PowerShell:

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

A configuração ProfileXML deve estar correta em estrutura, ortografia, configuração e, às vezes, letras maiúsculas e minúsculas. Se você vir algo diferente na estrutura para listagem 1, a marcação ProfileXML provavelmente conterá um erro.

Se você precisar solucionar problemas de marcação, é mais fácil colocá-lo em um editor de XML do que solucionar o problema no ISE do Windows PowerShell. Em ambos os casos, comece com a versão mais simples do perfil e adicione os componentes de volta um de cada vez até que o problema ocorra novamente.

## <a name="configure-the-vpn-client-by-using-system-center-configuration-manager"></a>Configurar o cliente VPN usando System Center Configuration Manager

No System Center Configuration Manager, você pode implantar perfis VPN usando o nó CSP ProfileXML, assim como fazia no Windows PowerShell. Aqui, você usa o script do Windows PowerShell VPN_Profile. ps1 que você criou na seção [criar os arquivos de configuração do ProfileXML](#create-the-profilexml-configuration-files).

Para usar System Center Configuration Manager para implantar um perfil VPN de Always On de acesso remoto em computadores cliente com Windows 10, você deve começar criando um grupo de computadores ou usuários para os quais você implanta o perfil. Nesse cenário, crie um grupo de usuários para implantar o script de configuração.

### <a name="create-a-user-group"></a>Criar um grupo de usuários

1.  No console do Configuration Manager, abra ativos e conformidade\\coleções de usuários.

2.  Na faixa de faixas **página inicial** , no grupo **criar** , clique em **criar coleção de usuários**.

3.  Na página geral, conclua as seguintes etapas:

    a. Em **nome**, digite **usuários VPN**.

    b. Clique em **procurar**, clique em **todos os usuários** e clique em **OK**.

    c. Clique em **Avançar**.

4.  Na página regras de associação, conclua as seguintes etapas:

    a.  Em **regras de associação**, clique em **Adicionar regra**e clique em **regra direta**. Neste exemplo, você está adicionando usuários individuais à coleção de usuários. No entanto, você pode usar uma regra de consulta para adicionar usuários a essa coleção dinamicamente para uma implantação de maior escala.

    b.  Na página **Bem-vindo**, clique em **Avançar**.

    c.  Na página pesquisar recursos, em **valor**, digite o nome do usuário que você deseja adicionar. O nome do recurso inclui o domínio do usuário. Para incluir resultados com base em uma correspondência parcial, insira **%** o caractere em qualquer extremidade do critério de pesquisa. Por exemplo, para localizar todos os usuários que contêm a cadeia de caracteres "Lori", digite **% Lori%** . Clique em **Avançar**.

    d.  Na página Selecionar recursos, selecione os usuários que você deseja adicionar ao grupo e clique em **Avançar**.

    e.  Na página Resumo, clique em **Avançar**.

    f.  Na página conclusão, clique em **fechar**.

6.  De volta à página regras de associação do assistente para criar coleção de usuários, clique em **Avançar**.

7.  Na página Resumo, clique em **Avançar**.

8.  Na página conclusão, clique em **fechar**.

Depois de criar o grupo de usuários para receber o perfil VPN, você pode criar um pacote e um programa para implantar o script de configuração do Windows PowerShell que você criou na seção [criar os arquivos de configuração do ProfileXML](#create-the-profilexml-configuration-files).

### <a name="create-a-package-containing-the-profilexml-configuration-script"></a>Criar um pacote que contém o script de configuração do ProfileXML

1.  Hospede o script VPN_Profile. ps1 em um compartilhamento de rede que a conta de computador do servidor do site possa acessar.

2.  No console do Configuration Manager, abra **biblioteca\\de software gerenciamento\\de aplicativos pacotes**.

3.  Na faixa de faixas **página inicial** , no grupo **criar** , clique em **criar pacote** para iniciar o assistente para criar pacote e programa.

4.  Na página pacote, conclua as seguintes etapas:

    a. Em **nome**, digite **Windows 10 Always on perfil VPN**.

    b. Marque a caixa de seleção **Este pacote contém arquivos de origem** e clique em **procurar**.

    c. Na caixa de diálogo Definir pasta de origem, clique em **procurar**, selecione o compartilhamento de arquivos que contém VPN_Profile. ps1 e clique em **OK**.
        Certifique-se de selecionar um caminho de rede, não um caminho local. Em outras palavras, o caminho deve ser algo como  *\\\\* o vpnscript, e não *c:\\vpnscript*.

1.  Clique em **Avançar**.

2.  Na página tipo de programa, clique em **Avançar**.

3.  Na página programa padrão, conclua as seguintes etapas:

    a.  Em **nome**, digite **script de perfil de VPN**.

    b.  Na **linha de comando**, digite **PowerShell. exe-executionpolicy bypass-File "VPN_Profile. ps1"** .

    c.  Em **modo de execução**, clique em **executar com direitos administrativos**.

    d.  Clique em **Avançar**.

4.  Na página requisitos, conclua as seguintes etapas:

    a.  Selecione **este programa pode ser executado somente em plataformas especificadas**.

    b.  Marque as caixas de seleção **todos os Windows 10 (32 bits)** e **todos os windows 10 (64 bits)** .

    c.  Em **espaço em disco estimado**, digite **1**.

    d.  Em **tempo de execução máximo permitido (minutos)** , digite **15**.

    e.  Clique em **Avançar**.

5.  Na página Resumo, clique em **Avançar**.

6.  Na página conclusão, clique em **fechar**.

Com o pacote e o programa criados, você precisa implantá-lo no grupo de **usuários VPN** .

### <a name="deploy-the-profilexml-configuration-script"></a>Implantar o script de configuração do ProfileXML

1.  No console do Configuration Manager, abra biblioteca\\de software gerenciamento\\de aplicativos pacotes.

2.  Em **pacotes**, clique em **perfil de VPN do Windows 10 Always on**.

3.  Na guia **programas** , na parte inferior do painel de detalhes, clique com o botão direito do mouse em **script de perfil VPN**, clique em **Propriedades**e conclua as seguintes etapas:

    a.  Na guia **avançado** , em **quando este programa é atribuído a um computador**, clique **uma vez para cada usuário que fizer logon**.

    b.  Clique em **OK**.

4.  Clique com o botão direito do mouse em **script de perfil VPN** e clique em **implantar** para iniciar o assistente de implantação de software.

5.  Na página geral, conclua as seguintes etapas:

    a.  Ao lado da **coleção**, clique em **procurar**.

    b.  Na lista **tipos de coleção** (superior à esquerda), clique em **coleções de usuários**.

    c.  Clique em **usuários VPN**e em **OK**.

    d.  Clique em **Avançar**.

6.  Na página conteúdo, conclua as seguintes etapas:

    a.  Clique em **Adicionar**e clique em **ponto de distribuição**.

    b.  Em **pontos de distribuição disponíveis**, selecione os pontos de distribuição para os quais você deseja distribuir o script de configuração ProfileXML e clique em **OK**.

    c.  Clique em **Avançar**.

7.  Na página Configurações de implantação, clique em **Avançar**.

8.  Na página agendamento, conclua as seguintes etapas:

    a.  Clique em **novo** para abrir a caixa de diálogo Agendamento de atribuição.

    b.  Clique em **atribuir imediatamente após esse evento**e clique em **OK**.

    c.  Clique em **Avançar**.

9.  Na página experiência do usuário, conclua as seguintes etapas:

    1.  Marque a caixa de seleção **instalação de software** .

    2.  Clique em **Resumo**.

10. Na página Resumo, clique em **Avançar**.

11. Na página conclusão, clique em **fechar**.

Com o script de configuração do ProfileXML implantado, entre em um computador cliente com o Windows 10 com a conta de usuário que você selecionou quando criou a coleção de usuários. Verifique a configuração do cliente VPN.

>[!NOTE]
>O script VPN_Profile. ps1 não funciona em uma sessão de Área de Trabalho Remota. Da mesma forma, ele não funciona em uma sessão avançada do Hyper-V. Se você estiver testando um acesso remoto Always On VPN em máquinas virtuais, desabilite a sessão avançada em suas VMs de cliente antes de continuar.

### <a name="verify-the-configuration-of-the-vpn-client"></a>Verificar a configuração do cliente VPN

1.  No painel de controle, **em\\segurança do sistema**, clique em **Configuration Manager**. 

2.  Na caixa de diálogo Propriedades do Configuration Manager, na guia **ações** , conclua as seguintes etapas:

    a.  Clique em **recuperação de política de computador & ciclo de avaliação**, clique em **executar agora**e clique em **OK**.

    b.  Clique em **ciclo de avaliação de & recuperação de política de usuário**, clique em **executar agora**e clique em **OK**.

    c.  Clique em **OK**.

3.  Feche o painel de controle.

Você deverá ver o novo perfil de VPN em breve.

## <a name="configure-the-vpn-client-by-using-intune"></a>Configurar o cliente VPN usando o Intune

Para usar o Intune para implantar o acesso remoto do Windows 10 Always On perfis VPN, você pode configurar o nó CSP ProfileXML usando o perfil VPN que você criou na seção [criar os arquivos de configuração do ProfileXML](#create-the-profilexml-configuration-files)ou pode usar o exemplo de XML EAP de base fornecido acima.

>[!NOTE]
>O Intune agora usa grupos do Azure AD. Se Azure AD Connect tiver sincronizado o grupo de usuários VPN do local para o Azure AD e os usuários forem atribuídos ao grupo de usuários VPN, você estará pronto para continuar.

Crie a política de configuração de dispositivo VPN para configurar os computadores cliente do Windows 10 para todos os usuários adicionados ao grupo. Como o modelo do Intune fornece parâmetros de VPN, copie \<apenas o \<EapHostConfig >/EapHostConfig > parte do arquivo VPN_ProfileXML.

### <a name="create-the-always-on-vpn-configuration-policy"></a>Criar a política de configuração de VPN Always On

1.  Entre no [portal do Azure](https://portal.azure.com/).

2.  Vá para**perfis**de**configuração** > de dispositivo do **Intune** > .

3.  Clique em **Criar perfil** para iniciar o assistente para criar perfil.

4.  Insira um **nome** para o perfil VPN e (opcionalmente) uma descrição.

1.   Em **plataforma**, selecione **Windows 10 ou posterior**e escolha **VPN** na lista suspensa tipo de perfil.

     >[!TIP]
     >Se você estiver criando um profileXML VPN personalizado, consulte [aplicar profileXML usando o Intune](https://docs.microsoft.com/windows/security/identity-protection/vpn/vpn-profile-options#apply-profilexml-using-intune) para obter as instruções.

2. Na guia **VPN de base** , verifique ou defina as seguintes configurações:

    - **Nome da conexão:** Insira o nome da conexão VPN que aparece no computador cliente na guia **VPN** em **configurações**, por exemplo, _contoso AutoVPN_.  
    
    - **Servidores** Adicione um ou mais servidores VPN clicando em **Adicionar.**
    
    - **Descrição** e **endereço IP ou FQDN:** Insira a descrição e o endereço IP ou o FQDN do servidor VPN. Esses valores devem ser alinhados com o nome da entidade no certificado de autenticação do servidor VPN. 
    
    - **Servidor padrão:** Se esse for o servidor VPN padrão, defina como **true**. Isso habilita esse servidor como o servidor padrão que os dispositivos usam para estabelecer a conexão. 
    
    - **Tipo de conexão:** Defina como **IKEv2**.  
    
    - **Always On:** Defina para **habilitar** o para se conectar à VPN automaticamente na entrada e permanecer conectado até que o usuário se desconecte manualmente.
    
    - **Lembrar credenciais a cada logon**:  Valor booliano (true ou false) para as credenciais de cache. Se definido como true, as credenciais serão armazenadas em cache sempre que possível.

3.  Copie a seguinte cadeia de caracteres XML em um editor de texto:
 
    [!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]
    
    
    ```XML
    <EapHostConfig xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><EapMethod><Type xmlns="https://www.microsoft.com/provisioning/EapCommon">25</Type><VendorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorId><VendorType xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorType><AuthorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</AuthorId></EapMethod><Config xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>25</Type><EapType xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV1"><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><FastReconnect>true</FastReconnect><InnerEapOptional>false</InnerEapOptional><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>13</Type><EapType xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV1"><CredentialsSource><CertificateStore><SimpleCertSelection>true</SimpleCertSelection></CertificateStore></CredentialsSource><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><DifferentUsername>false</DifferentUsername><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</AcceptServerName></EapType></Eap><EnableQuarantineChecks>false</EnableQuarantineChecks><RequireCryptoBinding>false</RequireCryptoBinding><PeapExtensions><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</AcceptServerName></PeapExtensions></EapType></Eap></Config></EapHostConfig>
    ```

4.  Substitua o  **\<TrustedRootCA > 5a 89 FE CB 5b 49 a7 0B 1a 52 63 B7 35 EE D7 1C C2 68 ser 4B <\/TrustedRootCA >** no exemplo com a impressão digital do certificado de sua autoridade de certificação raiz local em ambos os locais.
  
    >[!Important]
    >Não use a impressão digital de exemplo no \<TrustedRootCA >\</TrustedRootCA > seção abaixo.  O TrustedRootCA deve ser a impressão digital do certificado da autoridade de certificação raiz local que emitiu o certificado de autenticação de servidor para servidores RRAS e NPS. **Esse não deve ser o certificado raiz da nuvem nem a impressão digital do certificado de CA emissora intermediária**.

5.  Substitua os  **\<servernames > NPS. contoso. com\</ServerNames >** no XML de exemplo pelo FQDN do NPS ingressado no domínio em que a autenticação ocorre. 

6.  Copie a cadeia de caracteres XML revisada e cole-a na caixa **EAP XML** na guia VPN de base e clique em **OK**.
    Uma política de configuração de dispositivo VPN Always On usando EAP é criada no Intune.


### <a name="sync-the-always-on-vpn-configuration-policy-with-intune"></a>Sincronizar a política de configuração de VPN Always On com o Intune

Para testar a política de configuração, entre em um computador cliente com Windows 10 como o usuário que você adicionou ao grupo **Always on usuários VPN** e, em seguida, sincronize com o Intune.

1.  No menu Iniciar, clique em **configurações**.

2.  Em configurações, clique em **contas**e clique em **acessar trabalho ou escola**.

3.  Clique no perfil MDM e clique em **informações**.

4.  Clique em **sincronizar** para forçar uma avaliação e recuperação de política do Intune.

5.  Feche as configurações. Após a sincronização, você verá o perfil VPN disponível no computador.

## <a name="next-steps"></a>Próximas etapas

Você concluiu a implantação de Always On VPN.  Para outros recursos que você pode configurar, consulte a tabela abaixo:

|Se desejar...  |Em seguida, consulte...  |
|---------|---------|
|Configurar o acesso condicional para VPN    |[Etapa 7. Adicional Configurar o acesso condicional para conectividade VPN usando o](../../ad-ca-vpn-connectivity-windows10.md)Azure AD: Nesta etapa, você pode ajustar como os usuários de VPN autorizados acessam seus recursos usando o [acesso condicional do Azure Active Directory (AD do Azure)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). Com o acesso condicional do Azure AD para conectividade de VPN (rede virtual privada), você pode ajudar a proteger as conexões VPN. O Acesso Condicional é um mecanismo de avaliação com base em política que permite que você crie regras de acesso para qualquer aplicativo conectado ao Azure Active Directory (Azure AD).         |
|Saiba mais sobre os recursos avançados de VPN  |[Recursos avançados de VPN](always-on-vpn-adv-options.md#advanced-vpn-features): Esta página fornece orientação sobre como habilitar filtros de tráfego VPN, como configurar conexões VPN automáticas usando gatilhos de aplicativo e como configurar o NPS para permitir somente conexões VPN de clientes que usam certificados emitidos pelo Azure AD.        |
