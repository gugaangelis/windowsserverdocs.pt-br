---
title: Configurar o túnel do dispositivo VPN no Windows 10
description: Saiba como criar um túnel de dispositivo VPN no Windows 10.
ms.prod: windows-server
ms.date: 11/05/2018
ms.technology: networking-ras
ms.topic: article
ms.assetid: 158b7a62-2c52-448b-9467-c00d5018f65b
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: a216c490c92461e07fd5093783ec2c3049e8accb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388030"
---
# <a name="configure-vpn-device-tunnels-in-windows-10"></a>Configurar túneis de dispositivo VPN no Windows 10

>Aplica-se a: Windows 10 versão 1709

Always On VPN oferece a capacidade de criar um perfil VPN dedicado para o dispositivo ou computador. Always On conexões VPN incluem dois tipos de túneis: 

- O _túnel de dispositivo_ se conecta a servidores VPN especificados antes que os usuários façam logon no dispositivo. Os cenários de conectividade de pré-logon e o gerenciamento de dispositivos usam o túnel de dispositivo.

- O _túnel do usuário_ se conecta somente depois que um usuário faz logon no dispositivo. O túnel do usuário permite que os usuários acessem recursos da organização por meio de servidores VPN.

Ao contrário do _encapsulamento do usuário_, que se conecta somente depois que um usuário faz logon no dispositivo ou computador, o _túnel de dispositivo_ permite que a VPN estabeleça conectividade antes que o usuário faça logon. O túnel de _dispositivo_ e o _túnel de usuário_ operam de forma independente com seus perfis de VPN, podem ser conectados ao mesmo tempo e podem usar diferentes métodos de autenticação e outras definições de configuração de VPN, conforme apropriado. O túnel do usuário dá suporte a SSTP e IKEv2, e o túnel de dispositivo dá suporte a IKEv2 somente sem suporte para fallback de SSTP.

O túnel do usuário tem suporte em dispositivos ingressados no domínio, não ingressados no domínio (grupo de trabalho) ou no Azure AD – para permitir cenários empresariais e BYOD. Ele está disponível em todas as edições do Windows, e os recursos de plataforma estão disponíveis para terceiros por meio do suporte de plug-in de VPN UWP.

O túnel de dispositivo só pode ser configurado em dispositivos ingressados no domínio que executam o Windows 10 Enterprise ou o Education versão 1709 ou posterior. Não há suporte para controle de terceiros do túnel do dispositivo.


## <a name="device-tunnel-requirements-and-features"></a>Recursos e requisitos de túnel de dispositivo
Você deve habilitar a autenticação de certificado do computador para conexões VPN e definir uma autoridade de certificação raiz para autenticar conexões VPN de entrada. 

```PowerShell
$VPNRootCertAuthority = “Common Name of trusted root certification authority”
$RootCACert = (Get-ChildItem -Path cert:LocalMachine\root | Where-Object {$_.Subject -Like “*$VPNRootCertAuthority*” })
Set-VpnAuthProtocol -UserAuthProtocolAccepted Certificate, EAP -RootCertificateNameToAccept $RootCACert -PassThru
```

![Recursos e requisitos de túnel de dispositivo](../../media/device-tunnel-feature-and-requirements.png)

## <a name="vpn-device-tunnel-configuration"></a>Configuração de túnel do dispositivo VPN

O XML do perfil de exemplo abaixo fornece uma boa orientação para cenários em que somente os pull iniciados pelo cliente são necessários no túnel do dispositivo.  Os filtros de tráfego são utilizados para restringir o túnel de dispositivo somente ao tráfego de gerenciamento.  Essa configuração funciona bem para cenários de atualização Windows Update, típica Política de Grupo (GP) e System Center Configuration Manager (SCCM), bem como conectividade VPN para o primeiro logon sem credenciais em cache ou cenários de redefinição de senha. 

Para casos de push iniciados pelo servidor, como Gerenciamento Remoto do Windows (WinRM), GPUpdate remotos e cenários remotos de atualização do SCCM – você deve permitir o tráfego de entrada no túnel do dispositivo, portanto, os filtros de tráfego não podem ser usados.  Se no perfil de túnel de dispositivo você ativar os filtros de tráfego, o túnel de dispositivo negará o tráfego de entrada.  Essa limitação será removida em versões futuras.


### <a name="sample-vpn-profilexml"></a>Exemplo de profileXML de VPN

A seguir está o exemplo de profileXML de VPN.

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

Dependendo das necessidades de cada cenário de implantação específico, outro recurso de VPN que pode ser configurado com o túnel de dispositivo é [detecção de rede confiável](https://social.technet.microsoft.com/wiki/contents/articles/38546.new-features-for-vpn-in-windows-10-and-windows-server-2016.aspx#Trusted_Network_Detection).

```
 <!-- inside/outside detection -->
  <TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection>
```

## <a name="deployment-and-testing"></a>Implantação e teste

Você pode configurar os túneis de dispositivo usando um script do Windows PowerShell e usando a ponte Instrumentação de Gerenciamento do Windows (WMI). O túnel de dispositivo VPN Always On deve ser configurado no contexto da conta do **sistema local** . Para fazer isso, será necessário usar o [PsExec](https://docs.microsoft.com/sysinternals/downloads/psexec), um dos [PsTools](https://docs.microsoft.com/sysinternals/downloads/pstools) incluídos no pacote de utilitários do [Sysinternals](https://docs.microsoft.com/sysinternals/) .

Para obter diretrizes sobre como implantar um `(.\Device)` por dispositivo versus um perfil por usuário `(.\User)`, consulte [usando o script do PowerShell com o provedor de ponte WMI](https://docs.microsoft.com/windows/client-management/mdm/using-powershell-scripting-with-the-wmi-bridge-provider).

Execute o seguinte comando do Windows PowerShell para verificar se você implantou com êxito um perfil de dispositivo:

  ```powershell
  Get-VpnConnection -AllUserConnection
  ```

A saída exibe uma lista do dispositivo\-perfis de VPN largos que são implantados no dispositivo.

### <a name="example-windows-powershell-script"></a>Exemplo de script do Windows PowerShell

Você pode usar o script do Windows PowerShell a seguir para auxiliar na criação de seu próprio script para a criação de perfil.

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

Veja a seguir os recursos adicionais para ajudá-lo em sua implantação de VPN.

### <a name="vpn-client-configuration-resources"></a>Recursos de configuração de cliente VPN

A seguir estão os recursos de configuração de cliente VPN.

- [Como criar perfis VPN no System Center Configuration Manager](https://docs.microsoft.com/sccm/protect/deploy-use/create-vpn-profiles)
- [Configurar conexões VPN Always On cliente do Windows 10](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)
- [Opções de perfil de VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-profile-options)

### <a name="remote-access-server-gateway-resources"></a>Recursos de gateway do servidor de acesso remoto

A seguir estão os recursos de gateway de RAS (servidor de acesso remoto).

- [Configurar o RRAS com um certificado de autenticação de computador](https://technet.microsoft.com/library/dd458982.aspx)
- [Solucionando problemas de conexões VPN IKEv2](https://technet.microsoft.com/library/dd941612.aspx)
- [Configurar o acesso remoto baseado em IKEv2](https://technet.microsoft.com/library/ff687731.aspx)

>[!IMPORTANT]
>Ao usar o túnel de dispositivo com um gateway de RAS da Microsoft, você precisará configurar o servidor RRAS para dar suporte à autenticação de certificado de computador IKEv2 habilitando o método **Permitir autenticação de certificado de computador para autenticação IKEv2** , conforme descrito [aqui](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee922682%28v=ws.10%29). Quando essa configuração é habilitada, é altamente recomendável que o cmdlet **set-VpnAuthProtocol** do PowerShell, juntamente com o parâmetro opcional **RootCertificateNameToAccept** , seja usado para garantir que as conexões RRAS IKEv2 sejam permitidas somente para certificados de cliente VPN que se encadeadom a uma autoridade de certificação raiz interna/privada definida explicitamente. Como alternativa, o armazenamento de **autoridades de certificação raiz confiáveis** no servidor RRAS deve ser alterado para garantir que ele não contenha autoridades de certificação públicas, conforme discutido [aqui](https://blogs.technet.microsoft.com/rrasblog/2009/06/10/what-type-of-certificate-to-install-on-the-vpn-server/). Métodos semelhantes também podem precisar ser considerados para outros gateways de VPN.

