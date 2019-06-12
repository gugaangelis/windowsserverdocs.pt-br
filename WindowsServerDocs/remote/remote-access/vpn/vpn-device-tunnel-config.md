---
title: Configurar o túnel de dispositivo VPN no Windows 10
description: Saiba como criar um túnel de dispositivo VPN no Windows 10.
ms.prod: windows-server-threshold
ms.date: 11/05/2018
ms.technology: networking-ras
ms.topic: article
ms.assetid: 158b7a62-2c52-448b-9467-c00d5018f65b
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: 989216f90e78689b464240cff957bab1d9c1229b
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749565"
---
# <a name="configure-vpn-device-tunnels-in-windows-10"></a>Configurar túneis de dispositivo VPN no Windows 10

>Aplica-se a: Windows 10 versão 1709

VPN Always On fornece a capacidade de criar um perfil VPN dedicado para o computador ou dispositivo. Conexões de VPN Always On incluem dois tipos de túneis: 

- _Túnel de dispositivo_ conecta-se a servidores VPN especificados antes dos usuários fizerem logon no dispositivo. Cenários de conectividade de pré-logon e fins de gerenciamento de dispositivo usam túnel do dispositivo.

- _Túnel do usuário_ conecta-se somente depois de um usuário faz logon no dispositivo. Túnel do usuário permite que os usuários acessem recursos da organização por meio de servidores VPN.

Diferentemente _túnel do usuário_, que se conecta somente depois de um usuário faz logon no dispositivo ou computador, _túnel do dispositivo_ permite que a VPN estabelecer a conectividade antes do usuário fizer logon. Ambos _túnel do dispositivo_ e _túnel do usuário_ operar de forma independente com seus perfis VPN, podem ser conectados ao mesmo tempo e pode usar diferentes métodos de autenticação e outras definições de configuração de VPN conforme apropriado. Túnel de usuário dá suporte a SSTP e IKEv2 e túnel do dispositivo dá suporte a IKEv2 somente com não há suporte para o fallback SSTP.

Há suporte para o túnel do usuário em ingressado no domínio, ingressado fora do domínio (grupo de trabalho) ou dispositivos ingressados ao AD do Azure para permitir o enterprise e cenários de BYOD. Ele está disponível em todas as edições do Windows e os recursos da plataforma estão disponíveis para terceiros por meio do suporte a plug-in VPN de UWP.

Túnel do dispositivo só pode ser configurada em dispositivos ingressados no domínio executando o Windows 10 Enterprise ou educação versão 1709 ou posterior. Não há nenhum suporte para controle de terceiros do túnel do dispositivo.


## <a name="device-tunnel-requirements-and-features"></a>Recursos e requisitos de túnel do dispositivo
Você deve habilitar a autenticação de certificado de computador para conexões VPN e definir uma autoridade de certificação raiz para autenticar conexões VPN de entrada. 

```PowerShell
$VPNRootCertAuthority = “Common Name of trusted root certification authority”
$RootCACert = (Get-ChildItem -Path cert:LocalMachine\root | Where-Object {$_.Subject -Like “*$VPNRootCertAuthority*” })
Set-VpnAuthProtocol -UserAuthProtocolAccepted Certificate, EAP -RootCertificateNameToAccept $RootCACert -PassThru
```

![Requisitos e recursos de túnel do dispositivo](../../media/device-tunnel-feature-and-requirements.png)

## <a name="vpn-device-tunnel-configuration"></a>Configuração de túnel do dispositivo VPN

O perfil de exemplo XML abaixo fornece boas diretrizes para cenários em que o cliente só iniciada puxa são necessários por meio do túnel do dispositivo.  Filtros de tráfego serão utilizados para restringir o túnel de dispositivo para apenas o tráfego de gerenciamento.  Essa configuração funciona bem para atualizar o Windows, política de grupo típicas (GP) e cenários de atualização do System Center Configuration Manager (SCCM), bem como conectividade VPN para o primeiro logon sem credenciais armazenadas em cache ou cenários de redefinição de senha. 

Para casos de envio por push iniciadas pelo servidor, como o Windows Remote Management (WinRM), GPUpdate remoto e cenários de atualização do SCCM remotos – permitir tráfego de entrada no túnel de dispositivo, portanto, não podem ser usados filtros de tráfego.  Se no perfil do dispositivo de túnel ativar filtros de tráfego, o dispositivo de túnel nega o tráfego de entrada.  Essa limitação será removida em versões futuras.


### <a name="sample-vpn-profilexml"></a>Exemplo profileXML VPN

A seguir está o exemplo VPN profileXML.

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

Dependendo das necessidades de cada cenário de implantação em particular, é outro recurso VPN que pode ser configurado com o túnel de dispositivo [detecção de rede confiável](https://social.technet.microsoft.com/wiki/contents/articles/38546.new-features-for-vpn-in-windows-10-and-windows-server-2016.aspx#Trusted_Network_Detection).

```
 <!-- inside/outside detection -->
  <TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection>
```

## <a name="deployment-and-testing"></a>Implantação e teste

Você pode configurar túneis de dispositivo usando um script do Windows PowerShell e usando a ponte de Windows Management Instrumentation (WMI). O túnel de dispositivo de VPN Always On deve ser configurado no contexto do **sistema LOCAL** conta. Para fazer isso, será necessário usar [PsExec](https://docs.microsoft.com/sysinternals/downloads/psexec), um da [PsTools](https://docs.microsoft.com/sysinternals/downloads/pstools) incluídos no [Sysinternals](https://docs.microsoft.com/sysinternals/) conjunto de utilitários.

Para obter diretrizes sobre como implantar um por dispositivo `(.\Device)` versus um por usuário `(.\User)` de perfil, consulte [usando o PowerShell scripts com o provedor WMI do Bridge](https://docs.microsoft.com/windows/client-management/mdm/using-powershell-scripting-with-the-wmi-bridge-provider).

Execute o seguinte comando do Windows PowerShell para verificar que você implantou um perfil de dispositivo com êxito:

  ```powershell
  Get-VpnConnection -AllUserConnection
  ```

A saída exibe uma lista de dispositivo\-amplos perfis VPN que são implantados no dispositivo.

### <a name="example-windows-powershell-script"></a>Exemplo Script do Windows PowerShell

Você pode usar o seguinte script do Windows PowerShell para auxiliar na criação de seu próprio script para a criação de perfil.

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

## <a name="additional-resources"></a>Recursos adicionais

A seguir estão os recursos adicionais para ajudá-lo com sua implantação de VPN.

### <a name="vpn-client-configuration-resources"></a>Recursos de configuração de cliente VPN

A seguir estão os recursos de configuração de cliente VPN.

- [Como criar perfis VPN no System Center Configuration Manager](https://docs.microsoft.com/sccm/protect/deploy-use/create-vpn-profiles)
- [Configurar o cliente do Windows 10 sempre em conexões VPN](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)
- [Opções de perfil VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-profile-options)

### <a name="remote-access-server-gateway-resources"></a>Recursos de Gateway de servidor de acesso remotos

Estes são os recursos de Gateway de servidor de acesso remoto (RAS).

- [Configurar o RRAS com um certificado de autenticação de computador](https://technet.microsoft.com/library/dd458982.aspx)
- [Solucionando problemas de conexões de VPN IKEv2](https://technet.microsoft.com/library/dd941612.aspx)
- [Configurar o acesso remoto baseada em IKEv2](https://technet.microsoft.com/library/ff687731.aspx)

>[!IMPORTANT]
>Ao usar o túnel do dispositivo com um gateway de RAS da Microsoft, você precisará configurar o servidor RRAS para dar suporte à autenticação de certificado de computador IKEv2, permitindo que o **permitir autenticação de certificado de computador para IKEv2** método de autenticação, conforme descrito [aqui](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee922682%28v=ws.10%29). Quando essa configuração estiver habilitada, é altamente recomendável que o **Set-VpnAuthProtocol** cmdlet do PowerShell, juntamente com o **RootCertificateNameToAccept** parâmetro opcional, é usado para garantir que As conexões IKEv2 de RRAS são permitidas somente para certificados de cliente VPN que se encadeiam com uma definido explicitamente interna/privada autoridade de certificação raiz. Como alternativa, o **autoridades de certificação raiz confiáveis** repositório no servidor RRAS deve ser corrigido para garantir que ele não contenha autoridades de certificação pública, conforme discutido [aqui](https://blogs.technet.microsoft.com/rrasblog/2009/06/10/what-type-of-certificate-to-install-on-the-vpn-server/). Métodos semelhantes podem também precisam ser considerados para outros gateways de VPN.

