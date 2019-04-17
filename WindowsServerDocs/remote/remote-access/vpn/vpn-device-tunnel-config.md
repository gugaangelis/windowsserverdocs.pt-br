---
title: Configurar o encapsulamento do dispositivo VPN no Windows 10
description: Saiba como criar um túnel de dispositivo VPN no Windows 10.
ms.prod: windows-server-threshold
ms.date: 11/05/2018
ms.technology: networking-ras
ms.topic: article
ms.assetid: 158b7a62-2c52-448b-9467-c00d5018f65b
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: 005721873ad3a0df942bc9e23eba13728965ccba
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6066966"
---
# Configurar túneis de dispositivo VPN no Windows 10

>Aplicável a: Windows 10 versão 1709

VPN Always On oferece a capacidade de criar um perfil de VPN dedicado para o dispositivo ou computador. Conexões VPN Always On incluem dois tipos de encapsulamentos: 

- _Encapsulamento do dispositivo_ se conecta aos servidores VPN especificados antes de logon de usuários no dispositivo. Cenários de conectividade de pré-logon e fins de gerenciamento de dispositivo usam o encapsulamento do dispositivo.

- _Túnel de usuário_ se conecta somente após um usuário faz logon no dispositivo. Encapsulamento de usuário permite que os usuários acessem recursos da organização por meio de servidores VPN.

Ao contrário de _encapsulamento do usuário_, que só se conecta após um usuário faz logon no dispositivo ou computador, _encapsulamento do dispositivo_ permite que a VPN estabelecer a conectividade antes do usuário faz logon. _Encapsulamento do dispositivo_ e o _encapsulamento de usuário_ operam de forma independente com seus perfis VPN, podem ser conectados ao mesmo tempo e podem usar diferentes métodos de autenticação e outras configurações de VPN conforme apropriado. Túnel de usuário é compatível com o SSTP e IKEv2 e encapsulamento do dispositivo suporta IKEv2 somente com nenhum suporte para fallback SSTP.

Túnel de usuário é compatível com ingressado no domínio, fora do domínio associado (grupo de trabalho) ou dispositivos Azure – ingressado no AD para permitir cenários BYOD e corporativos. Ele está disponível em todas as edições do Windows e os recursos de plataforma estão disponíveis a terceiros por meio de suporte de plug-in de VPN UWP.

Encapsulamento do dispositivo só pode ser configurado em dispositivos ingressados em domínio executando o Windows 10 Enterprise ou Education versão 1709 ou posterior. Não há nenhum suporte para o controle de terceiros do túnel de dispositivo.


## Recursos e requisitos de encapsulamento do dispositivo
Você deve habilitar a autenticação de certificado de máquina para conexões VPN e define uma autoridade de certificação raiz para autenticar conexões VPN de entrada. 

```PowerShell
$VPNRootCertAuthority = “Common Name of trusted root certification authority”
$RootCACert = (Get-ChildItem -Path cert:LocalMachine\root | Where-Object {$_.Subject -Like “*$VPNRootCertAuthority*” })
Set-VpnAuthProtocol -UserAuthProtocolAccepted Certificate, EAP -RootCertificateNameToAccept $RootCACert -PassThru
```

![Requisitos e recursos de encapsulamento do dispositivo](../../media/device-tunnel-feature-and-requirements.png)

## Configuração de encapsulamento do dispositivo VPN

O perfil de exemplo XML abaixo fornece orientações bom para cenários em que o cliente só iniciada deslizado são necessários no túnel de dispositivo.  Filtros de tráfego são usados para restringir o encapsulamento do dispositivo de apenas o tráfego de gerenciamento.  Essa configuração funciona bem para o Windows Update, política de grupo típico (GP) e cenários de atualização do System Center Configuration Manager (SCCM), bem como conectividade VPN para o primeiro logon sem credenciais armazenadas em cache ou cenários de redefinição de senha. 

Para casos de push iniciadas pelo servidor, como Windows Remote Management (WinRM), GPUpdate remoto e cenários de atualização do SCCM remotos – permitir tráfego de entrada em túnel de dispositivo, portanto, filtros de tráfego não podem ser usados.  Se o perfil de encapsulamento do dispositivo você ativar filtros de tráfego, o encapsulamento do dispositivo nega o tráfego de entrada.  Essa limitação vai ser removidos em versões futuras.


### Exemplo VPN profileXML

A seguir está o profileXML VPN de amostra.

``` xml
<VPNProfile>  
  <NativeProfile>  
<Servers>vpn.contoso.com</Servers>  
<NativeProtocolType>IKEv2</NativeProtocolType>  
<Authentication>  
  <MachineMethod>Certificate</MachineMethod>  
</Authentication>  
<RoutingPolicyType>SplitTunnel</RoutingPolicyType>  
 <!-- disable the addition of a class based route for the assigned IP address on the VPN interface -->
<DisableClassBasedDefaultRoute>true</DisableClassBasedDefaultRoute>  
  </NativeProfile> 
  <!-- use host routes(/32) to prevent routing conflicts -->  
  <Route>  
<Address>10.10.0.2</Address>  
<PrefixSize>32</PrefixSize>  
  </Route>  
  <Route>  
<Address>10.10.0.3</Address>  
<PrefixSize>32</PrefixSize>  
  </Route>  
<!-- traffic filters for the routes specified above so that only this traffic can go over the device tunnel --> 
  <TrafficFilter>  
<RemoteAddressRanges>10.10.0.2, 10.10.0.3</RemoteAddressRanges>  
  </TrafficFilter>
<!-- need to specify always on = true --> 
  <AlwaysOn>true</AlwaysOn> 
<!-- new node to specify that this is a device tunnel -->  
 <DeviceTunnel>true</DeviceTunnel>
<!--new node to register client IP address in DNS to enable manage out -->
<RegisterDNS>true</RegisterDNS>
</VPNProfile>
```

Dependendo das necessidades de cada cenário de implantação em particular, outro recurso VPN que pode ser configurado com o encapsulamento do dispositivo é [Detecção de rede confiável](https://social.technet.microsoft.com/wiki/contents/articles/38546.new-features-for-vpn-in-windows-10-and-windows-server-2016.aspx#Trusted_Network_Detection).

```
 <!-- inside/outside detection --> 
  <TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection> 
```

## Implantação e o teste

Você pode configurar túneis de dispositivo usando um script do Windows PowerShell e usando a ponte do Windows Management Instrumentation \(WMI\). O encapsulamento do dispositivo VPN Always On deve ser configurado no contexto da conta do **Sistema LOCAL** . Para fazer isso, será necessário usar [PsExec](https://docs.microsoft.com/sysinternals/downloads/psexec), um dos [PsTools](https://docs.microsoft.com/sysinternals/downloads/pstools) incluídos no conjunto de utilitários [Sysinternals](https://docs.microsoft.com/sysinternals/) .

Para obter diretrizes sobre como implantar um por dispositivo `(.\Device)` versus um por usuário `(.\User)` de perfil, consulte [scripts do PowerShell usando com o provedor de ponte WMI](https://docs.microsoft.com/windows/client-management/mdm/using-powershell-scripting-with-the-wmi-bridge-provider). 

Execute o seguinte comando do Windows PowerShell para verificar se você tiver implantado com êxito um perfil de dispositivo:

    `Get-VpnConnection -AllUserConnection`

A saída exibe uma lista dos perfis de VPN em todo o device\ que são implantados no dispositivo.


### Exemplo Script do Windows PowerShell

Você pode usar o seguinte script do Windows PowerShell para ajudar a criar seus próprios scripts para a criação de perfil.

```PowerShell
Param(
[string]$xmlFilePath,
[string]$ProfileName
)

$a = Test-Path $xmlFilePath
echo $a

$ProfileXML = Get-Content $xmlFilePath

echo $XML

$ProfileNameEscaped = $ProfileName -replace ' ', '%20'

$Version = 201606090004

$ProfileXML = $ProfileXML -replace '<', '&lt;'
$ProfileXML = $ProfileXML -replace '>', '&gt;'
$ProfileXML = $ProfileXML -replace '"', '&quot;'

$nodeCSPURI = './Vendor/MSFT/VPNv2'
$namespaceName = "root\cimv2\mdm\dmmap"
$className = "MDM_VPNv2_01"

$session = New-CimSession

try
{
$newInstance = New-Object Microsoft.Management.Infrastructure.CimInstance $className, $namespaceName
$property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ParentID", "$nodeCSPURI", 'String', 'Key')
$newInstance.CimInstanceProperties.Add($property)
$property = [Microsoft.Management.Infrastructure.CimProperty]::Create("InstanceID", "$ProfileNameEscaped", 'String', 'Key')
$newInstance.CimInstanceProperties.Add($property)
$property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ProfileXML", "$ProfileXML", 'String', 'Property')
$newInstance.CimInstanceProperties.Add($property)

$session.CreateInstance($namespaceName, $newInstance)
$Message = "Created $ProfileName profile."
Write-Host "$Message"
}
catch [Exception]
{
$Message = "Unable to create $ProfileName profile: $_"
Write-Host "$Message"
exit
}
$Message = "Complete."
Write-Host "$Message"
```

## Recursos adicionais

Estes são recursos adicionais para ajudá-lo com a implantação de VPN.

### Recursos de configuração de cliente VPN

Estes são recursos de configuração do cliente VPN.

- [Como perfis de VPN criar no System Center Configuration Manager](https://docs.microsoft.com/sccm/protect/deploy-use/create-vpn-profiles)
- [Configurar conexões de VPN Always On em cliente do Windows 10](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)
- [Opções de perfil de VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-profile-options)

### Remoto \(RAS\) servidor de acesso a recursos de Gateway

Estes são os recursos de Gateway de RAS.

- [Configurar o RRAS com um certificado de autenticação do computador](https://technet.microsoft.com/library/dd458982.aspx)
- [Solucionando problemas de conexões de VPN IKEv2](https://technet.microsoft.com/library/dd941612.aspx)
- [Configurar o acesso remoto baseadas em IKEv2](https://technet.microsoft.com/library/ff687731.aspx)

>[!IMPORTANT]
>Ao usar o encapsulamento do dispositivo com um gateway de RAS Microsoft, você precisará configurar o servidor RRAS para dar suporte à autenticação de certificado de máquina IKEv2, permitindo que o método de autenticação **Permitir autenticação de certificado de máquina para IKEv2** , conforme descrito [aqui](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee922682%28v=ws.10%29). Depois que essa configuração é habilitada, é altamente recomendável que o cmdlet do PowerShell **Conjunto VpnAuthProtocol** , juntamente com o parâmetro opcional **RootCertificateNameToAccept** , é usado para garantir que as conexões RRAS IKEv2 só são permitidas para Cliente VPN certificados que são encadeados para um definidas explicitamente interno/privada autoridade de certificação raiz. Como alternativa, o armazenamento de **Autoridades de certificação raiz confiáveis** no servidor RRAS deve ser corrigido para garantir que ele não contém autoridades de certificação pública como discutidas [aqui](https://blogs.technet.microsoft.com/rrasblog/2009/06/10/what-type-of-certificate-to-install-on-the-vpn-server/). Métodos semelhantes também talvez precise ser considerado para outros gateways VPN.

---
