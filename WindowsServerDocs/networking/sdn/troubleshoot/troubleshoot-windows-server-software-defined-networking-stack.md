---
title: Solucionar problemas de pilha de rede definida do Windows Server Software
description: Este guia do Windows Server examina os erros comuns de SDN (rede definida pelo software) e os cenários de falha e descreve um fluxo de trabalho de solução de problemas que aproveita as ferramentas de diagnóstico disponíveis.
manager: ravirao
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 9be83ed2-9e62-49e8-88e7-f52d3449aac5
ms.author: pashort
author: JMesser81
ms.date: 08/14/2018
ms.openlocfilehash: 2782419f0c3d99e7ec7f4ee3389f174df400bd55
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75949921"
---
# <a name="troubleshoot-the-windows-server-software-defined-networking-stack"></a>Solucionar problemas de pilha de rede definida do Windows Server Software

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Este guia examina os erros comuns de SDN (rede definida pelo software) e os cenários de falha e descreve um fluxo de trabalho de solução de problemas que aproveita as ferramentas de diagnóstico disponíveis.  

Para obter mais informações sobre a rede definida pelo software da Microsoft, consulte [rede definida pelo software](../../sdn/Software-Defined-Networking--SDN-.md).  

## <a name="error-types"></a>Tipos de erros  
A lista a seguir representa a classe de problemas que mais frequentemente vimos com a HNVv1 (virtualização de rede do Hyper-V) no Windows Server 2012 R2 de implantações de produção no mercado e coincide de várias maneiras com os mesmos tipos de problemas vistos no Windows Server 2016 HNVv2 com a nova pilha de SDN (rede definida pelo software).  

A maioria dos erros pode ser classificada em um pequeno conjunto de classes:   
* **Configuração inválida ou sem suporte**  
   Um usuário invoca a API NorthBound incorretamente ou com uma política inválida.   

* **Erro no aplicativo de política**  
     A política do controlador de rede não foi entregue a um host Hyper-V, atrasado e/ou não atualizado em todos os hosts Hyper-V (por exemplo, depois de um Migração ao Vivo).  
* **Descompasso de configuração ou bug de software**  
  Problemas de caminho de dados que resultam em pacotes descartados.  

* **Erro externo relacionado a hardware/drivers NIC ou à malha de rede underlay**  
  Descarregamentos de tarefas com comportamento inadequado (como a VMQ) ou underlay de malha de rede incorretamente (como MTU)   

  Este guia de solução de problemas examina cada uma dessas categorias de erro e recomenda as práticas recomendadas e ferramentas de diagnóstico disponíveis para identificar e corrigir o erro.  

## <a name="diagnostic-tools"></a>Ferramentas de diagnóstico  

Antes de discutir os fluxos de trabalho de solução de problemas para cada um desses tipos de erros, vamos examinar as ferramentas de diagnóstico disponíveis.   

Para usar as ferramentas de diagnóstico do controlador de rede (caminho de controle), você deve primeiro instalar o recurso RSAT-NetworkController e importar o módulo ``NetworkControllerDiagnostics``:  

```  
Add-WindowsFeature RSAT-NetworkController -IncludeManagementTools  
Import-Module NetworkControllerDiagnostics  
```  

Para usar as ferramentas de diagnóstico do HNV Diagnostics (Data-Path), você deve importar o módulo ``HNVDiagnostics``:

```  
# Assumes RSAT-NetworkController feature has already been installed
Import-Module hnvdiagnostics   
```  

### <a name="network-controller-diagnostics"></a>Diagnóstico do controlador de rede  
Esses cmdlets estão documentados no TechNet no [tópico do cmdlet de diagnóstico do controlador de rede](https://docs.microsoft.com/powershell/module/networkcontrollerdiagnostics/). Eles ajudam a identificar problemas com a consistência da diretiva de rede no caminho de controle entre os nós do controlador de rede e entre o controlador de rede e os agentes do host NC em execução nos hosts do Hyper-V.

 Os cmdlets _debug-ServiceFabricNodeStatus_ e _Get-NetworkControllerReplica_ devem ser executados de uma das máquinas virtuais do nó do controlador de rede. Todos os outros cmdlets de diagnóstico NC podem ser executados em qualquer host que tenha conectividade com o controlador de rede e esteja no grupo de segurança de gerenciamento do controlador de rede (Kerberos) ou tenha acesso ao certificado X. 509 para gerenciar o controlador de rede. 

### <a name="hyper-v-host-diagnostics"></a>Diagnóstico de host do Hyper-V  
Esses cmdlets estão documentados no TechNet no [tópico do cmdlet de diagnóstico de HNV (virtualização de rede) do Hyper-V](https://docs.microsoft.com/powershell/module/hnvdiagnostics/). Eles ajudam a identificar problemas no caminho de dados entre as máquinas virtuais de locatário (leste/oeste) e o tráfego de entrada por meio de um VIP SLB (Norte/Sul). 

O _debug-VirtualMachineQueueOperation_, o _Get-CustomerRoute_, o _Get-PACAMapping_, o _Get-ProviderAddress_, o _Get-VMNetworkAdapterPortId_, o _Get-VMSwitchExternalPortId_e o _Test-EncapOverheadSettings_ são todos os testes locais que podem ser executados em qualquer host Hyper-V. Os outros cmdlets invocam testes de caminho de dados por meio do controlador de rede e, portanto, precisam de acesso ao controlador de rede como Descried acima.

### <a name="github"></a>GitHub
O [repositório GitHub Microsoft/Sdn](https://github.com/microsoft/sdn) tem vários scripts de exemplo e fluxos de trabalho que se baseiam nesses cmdlets na caixa. Em particular, os scripts de diagnóstico podem ser encontrados na pasta de [diagnóstico](https://github.com/Microsoft/sdn/diagnostics) . Ajude-nos a contribuir com esses scripts enviando solicitações pull.

## <a name="troubleshooting-workflows-and-guides"></a>Solucionando problemas de fluxos de trabalho e guias  

### <a name="hoster-validate-system-health"></a>Hoster Validar a integridade do sistema
Há um recurso inserido chamado _estado de configuração_ em vários dos recursos do controlador de rede. O estado de configuração fornece informações sobre a integridade do sistema, incluindo a consistência entre a configuração do controlador de rede e o estado real (em execução) nos hosts do Hyper-V. 

Para verificar o estado da configuração, execute o seguinte em qualquer host Hyper-V com conectividade com o controlador de rede.

>[!NOTE] 
>O valor do parâmetro *NetworkController* deve ser o FQDN ou o endereço IP com base no nome da entidade do certificado X. 509 > criado para o controlador de rede.
>
>O parâmetro *Credential* só precisa ser especificado se o controlador de rede estiver usando a autenticação Kerberos (típica em implantações do VMM). A credencial deve ser para um usuário que está no grupo de segurança de gerenciamento do controlador de rede.

```none
Debug-NetworkControllerConfigurationState -NetworkController <FQDN or NC IP> [-Credential <PS Credential>]

# Healthy State Example - no status reported
$cred = Get-Credential
Debug-NetworkControllerConfigurationState -NetworkController 10.127.132.211 -Credential $cred

Fetching ResourceType:     accessControlLists
Fetching ResourceType:     servers
Fetching ResourceType:     virtualNetworks
Fetching ResourceType:     networkInterfaces
Fetching ResourceType:     virtualGateways
Fetching ResourceType:     loadbalancerMuxes
Fetching ResourceType:     Gateways
```

Uma mensagem de estado de configuração de exemplo é mostrada abaixo:

```none
Fetching ResourceType:     servers
---------------------------------------------------------------------------------------------------------
ResourcePath:     https://10.127.132.211/Networking/v1/servers/4c4c4544-0056-4b10-8058-b8c04f395931
Status:           Warning

Source:           SoftwareLoadBalancerManager
Code:             HostNotConnectedToController
Message:          Host is not Connected.
----------------------------------------------------------------------------------------------------------
```

>[!NOTE]
> Há um bug no sistema em que os recursos de interface de rede para a NIC de VM de trânsito do SLB MUX estão em um estado de falha com o erro "comutador virtual-host não conectado ao controlador". Esse erro pode ser ignorado com segurança se a configuração de IP no recurso NIC da VM estiver definida como um endereço IP do pool de IPS da rede lógica de trânsito. Há um segundo bug no sistema em que os recursos de interface de rede para as NICs da VM do provedor HNV do gateway estão em um estado de falha com o erro "comutador virtual-PortBlocked". Esse erro também pode ser ignorado com segurança se a configuração de IP no recurso NIC da VM estiver definida como NULL (por Design).


A tabela a seguir mostra a lista de códigos de erro, mensagens e ações de acompanhamento a serem executadas com base no estado de configuração observado.


| **Código**| **Message**| **Ação**|  
|--------|-----------|----------|  
| Unknown| Erro desconhecido| |  
| HostUnreachable                       | O computador host não está acessível | Verificar a conectividade de rede de gerenciamento entre o controlador de rede e o host |  
| PAIpAddressExhausted                  | Os endereços IP PA foram exauridos | Aumentar o tamanho do pool de IPS da sub-rede lógica do provedor HNV |  
| PAMacAddressExhausted                 | Os endereços MAC do PA esgotados | Aumentar o intervalo do pool de Mac |  
| PAAddressConfigurationFailure         | Falha ao encanar endereços PA para o host | Verifique a conectividade de rede de gerenciamento entre o controlador de rede e o host. |  
| CertificateNotTrusted                 | O certificado não é confiável  |Corrija os certificados usados para comunicação com o host. |  
| CertificateNotAuthorized              | Certificado não autorizado | Corrija os certificados usados para comunicação com o host. |  
| PolicyConfigurationFailureOnVfp       | Falha ao configurar políticas VFP | Essa é uma falha de tempo de execução.  Não há nenhum trabalho definitivo. Coletar logs. |  
| PolicyConfigurationFailure            | Falha ao enviar políticas para os hosts devido a falhas de comunicação ou outros erros no NetworkController.| Nenhuma ação definitiva.  Isso ocorre devido a uma falha no processamento do estado de meta nos módulos do controlador de rede. Coletar logs. |  
| HostNotConnectedToController          | O host ainda não está conectado ao controlador de rede | Perfil de porta não aplicado no host ou o host não pode ser acessado do controlador de rede. Validar que a chave do registro hostid corresponde à ID da instância do recurso do servidor |  
| MultipleVfpEnabledSwitches            | Há vários comutadores habilitados para VFp no host  | Excluir uma das opções, já que o agente de host do controlador de rede dá suporte apenas a um vSwitch com a extensão VFP habilitada |  
| PolicyConfigurationFailure            | Falha ao enviar políticas de VNet para um VmNic devido a erros de certificado ou erros de conectividade  | Verifique se os certificados adequados foram implantados (o nome da entidade do certificado deve corresponder ao FQDN do host). Verifique também a conectividade do host com o controlador de rede |  
| PolicyConfigurationFailure            | Falha ao enviar políticas vSwitch para um VmNic devido a erros de certificado ou erros de conectividade  | Verifique se os certificados adequados foram implantados (o nome da entidade do certificado deve corresponder ao FQDN do host). Verifique também a conectividade do host com o controlador de rede|
| PolicyConfigurationFailure            | Falha ao enviar por push as políticas de firewall para um VmNic devido a erros de certificado ou erros de conectividade | Verifique se os certificados adequados foram implantados (o nome da entidade do certificado deve corresponder ao FQDN do host). Verifique também a conectividade do host com o controlador de rede|
| DistributedRouterConfigurationFailure | Falha ao definir as configurações do roteador distribuído no host vNic                          | Erro de pilha TCPIP. Pode exigir a limpeza do vNICs de host PA e DR no servidor no qual esse erro foi relatado |
| DhcpAddressAllocationFailure          | Falha na alocação de endereço DHCP para um VMNic                                                    | Verificar se o atributo de endereço IP estático está configurado no recurso NIC |  
| CertificateNotTrusted<br>CertificateNotAuthorized | Falha ao conectar-se ao MUX devido a erros de rede ou de certificado | Verifique o código numérico fornecido no código da mensagem de erro: isso corresponde ao código de erro do Winsock. Os erros de certificado são granulares (por exemplo, o certificado não pode ser verificado, certificado não autorizado, etc.) |  
| HostUnreachable                       | MUX não íntegro (o caso comum é BGPRouter desconectado) | O par de BGP na opção RRAS (BGP Virtual Machine) ou Top-of-rack (ToR) está inacessível ou não está emparelhando com êxito. Verificar as configurações de BGP no recurso Multiplexador de software Load Balancer e no par de BGP (ToR ou máquina virtual RRAS) |  
| HostNotConnectedToController          | O agente de host SLB não está conectado  | Verifique se o serviço do agente de host SLB está em execução; Consulte os logs do agente de host SLB (execução automática) por motivos pelos quais, no caso de SLBM (NC) rejeitou que o certificado apresentado pelo estado de execução do agente de host mostrará informações nuances  |  
| PortBlocked                           | A porta VFP está bloqueada devido à falta de políticas de VNET/ACL | Verifique se há outros erros, o que pode fazer com que as políticas não sejam configuradas. |  
| Sobrecarregado                            | Objectbalancer MUX sobrecarregado  | Problema de desempenho com o MUX |  
| RoutePublicationFailure               | O MUX do Balancer não está conectado a um roteador BGP | Verifique se a MUX tem conectividade com os Roteadores BGP e se o emparelhamento via protocolo BGP está configurado corretamente |  
| VirtualServerUnreachable              | O multiplexador do Balancer não está conectado ao Gerenciador SLB | Verificar a conectividade entre o SLBM e o MUX |  
| QosConfigurationFailure               | Falha ao configurar políticas de QOS | Veja se há largura de banda suficiente disponível para todas as VMs se a reserva de QOS for usada |


#### <a name="check-network-connectivity-between-the-network-controller-and-hyper-v-host-nc-host-agent-service"></a>Verificar a conectividade de rede entre o controlador de rede e o host Hyper-V (serviço do agente de host NC)
Execute o comando *netstat* abaixo para validar que há três conexões estabelecidas entre o agente do host NC e os nós do controlador de rede e um soquete de escuta no host do Hyper-V
- ESCUTAndo na porta TCP: 6640 no host Hyper-V (serviço de agente de host NC)
- Duas conexões estabelecidas do IP do host Hyper-V na porta 6640 para o IP do nó NC em portas efêmeras (> 32000)
- Uma conexão estabelecida do IP do host Hyper-V na porta efêmera para o IP REST do controlador de rede na porta 6640

>[!NOTE]
>Pode haver apenas duas conexões estabelecidas em um host Hyper-V se não houver máquinas virtuais de locatário implantadas nesse host específico.

```none
netstat -anp tcp |findstr 6640

# Successful output
  TCP    0.0.0.0:6640           0.0.0.0:0              LISTENING
  TCP    10.127.132.153:6640    10.127.132.213:50095   ESTABLISHED
  TCP    10.127.132.153:6640    10.127.132.214:62514   ESTABLISHED
  TCP    10.127.132.153:50023   10.127.132.211:6640    ESTABLISHED
```
#### <a name="check-host-agent-services"></a>Verificar serviços do Host Agent
O controlador de rede se comunica com dois serviços de agente de host nos hosts Hyper-V: agente de host SLB e agente de host NC. É possível que um ou ambos os serviços não estejam em execução. Verifique seu estado e reinicie se eles não estiverem em execução.

```none
Get-Service SlbHostAgent
Get-Service NcHostAgent

# (Re)start requires -Force flag
Start-Service NcHostAgent -Force
Start-Service SlbHostAgent -Force
```

#### <a name="check-health-of-network-controller"></a>Verificar a integridade do controlador de rede
Se não houver três conexões estabelecidas ou se o controlador de rede não estiver respondendo, verifique se todos os nós e módulos de serviço estão ativos e em execução usando os cmdlets a seguir. 

```none
# Prints a DIFF state (status is automatically updated if state is changed) of a particular service module replica 
Debug-ServiceFabricNodeStatus [-ServiceTypeName] <Service Module>
```
Os módulos de serviço do controlador de rede são:
- ControllerService
- ApiService
- SlbManagerService
- Ininserção
- FirewallService
- VSwitchService
- Gatewaymanager
- FnmService
- HelperService
- UpdateService

Verifique se ReplicaStatus está **pronto** e HealthState está **OK**.

Em uma implantação de produção é com um controlador de rede de vários nós, você também pode verificar em qual nó cada serviço é primário e seu status de réplica individual.

```none  
Get-NetworkControllerReplica

# Sample Output for the API service module 
Replicas for service: ApiService

ReplicaRole   : Primary
NodeName      : SA18N30NC3.sa18.nttest.microsoft.com
ReplicaStatus : Ready

```
Verifique se o status da réplica está pronto para cada serviço.

#### <a name="check-for-corresponding-hostids-and-certificates-between-network-controller-and-each-hyper-v-host"></a>Verificar HostIDs e certificados correspondentes entre o controlador de rede e cada host do Hyper-V 
Em um host Hyper-V, execute os seguintes comandos para verificar se o hostid corresponde à ID de instância de um recurso de servidor no controlador de rede

```none
Get-ItemProperty "hklm:\system\currentcontrolset\services\nchostagent\parameters" -Name HostId |fl HostId

HostId : **162cd2c8-08d4-4298-8cb4-10c2977e3cfe**

Get-NetworkControllerServer -ConnectionUri $uri |where { $_.InstanceId -eq "162cd2c8-08d4-4298-8cb4-10c2977e3cfe"}

Tags             :
ResourceRef      : /servers/4c4c4544-0056-4a10-8059-b8c04f395931
InstanceId       : **162cd2c8-08d4-4298-8cb4-10c2977e3cfe**
Etag             : W/"50f89b08-215c-495d-8505-0776baab9cb3"
ResourceMetadata : Microsoft.Windows.NetworkController.ResourceMetadata
ResourceId       : 4c4c4544-0056-4a10-8059-b8c04f395931
Properties       : Microsoft.Windows.NetworkController.ServerProperties
```

*Correção* Se estiver usando scripts SDNExpress ou implantação manual, atualize a chave de hostid no registro para corresponder à ID de instância do recurso de servidor. Reinicie o agente de host do controlador de rede no host do Hyper-V (servidor físico) se estiver usando o VMM, exclua o servidor Hyper-V do VMM e remova a chave do registro hostid. Em seguida, adicione novamente o servidor por meio do VMM.


Verifique se as impressões digitais dos certificados X. 509 usados pelo host do Hyper-V (o nome do host será a entidade) para a comunicação (SouthBound) entre o host Hyper-V (serviço do agente de host do NC) e os nós do controlador de rede são os mesmos. Verifique também se o certificado REST do controlador de rede tem o nome de assunto *CN =<FQDN or IP>* .

```  
# On Hyper-V Host
dir cert:\\localmachine\my  

Thumbprint                                Subject
----------                                -------
2A3A674D07D8D7AE11EBDAC25B86441D68D774F9  CN=SA18n30-4.sa18.nttest.microsoft.com
...

dir cert:\\localmachine\root

Thumbprint                                Subject
----------                                -------
30674C020268AA4E40FD6817BA6966531FB9ADA4  CN=10.127.132.211 **# NC REST IP ADDRESS**

# On Network Controller Node VM
dir cert:\\localmachine\root  

Thumbprint                                Subject
----------                                -------
2A3A674D07D8D7AE11EBDAC25B86441D68D774F9  CN=SA18n30-4.sa18.nttest.microsoft.com
30674C020268AA4E40FD6817BA6966531FB9ADA4  CN=10.127.132.211 **# NC REST IP ADDRESS**
...
```  

Você também pode verificar os seguintes parâmetros de cada certificado para certificar-se de que o nome da entidade é esperado (FQDN ou IP REST de nome de host ou NC), o certificado ainda não expirou e que todas as autoridades de certificação na cadeia de certificados estão incluídas na raiz confiável autoridades.

- Nome do assunto  
- Data de validade  
- Confiável por autoridade raiz  

*Correção* Se vários certificados tiverem o mesmo nome de entidade no host Hyper-V, o agente de host do controlador de rede escolherá aleatoriamente um para apresentar ao controlador de rede. Isso pode não corresponder à impressão digital do recurso de servidor conhecido pelo controlador de rede. Nesse caso, exclua um dos certificados com o mesmo nome de entidade no host Hyper-V e reinicie o serviço de agente de host do controlador de rede. Se uma conexão ainda não puder ser feita, exclua o outro certificado com o mesmo nome de assunto no host Hyper-V e exclua o recurso de servidor correspondente no VMM. Em seguida, recrie o recurso de servidor no VMM, que irá gerar um novo certificado X. 509 e instalá-lo no host Hyper-V.


#### <a name="check-the-slb-configuration-state"></a>Verificar o estado de configuração de SLB
O estado de configuração de SLB pode ser determinado como parte da saída para o cmdlet Debug-NetworkController. Esse cmdlet também produzirá o conjunto atual de recursos do controlador de rede em arquivos JSON, todas as configurações de IP de cada host do Hyper-V (servidor) e política de rede local de tabelas de banco de dados do agente do host. 

Os rastreamentos adicionais serão coletados por padrão. Para não coletar rastreamentos, adicione o parâmetro-IncludeTraces: $false.

```none
Debug-NetworkController -NetworkController <FQDN or IP> [-Credential <PS Credential>] [-IncludeTraces:$false]

# Don't collect traces
$cred = Get-Credential 
Debug-NetworkController -NetworkController 10.127.132.211 -Credential $cred -IncludeTraces:$false

Transcript started, output file is C:\\NCDiagnostics.log
Collecting Diagnostics data from NC Nodes
```

>[!NOTE]
>O local de saída padrão será o < working_directory > diretório \NCDiagnostics\. O diretório de saída padrão pode ser alterado usando o parâmetro `-OutputDirectory`. 

As informações de estado de configuração de SLB podem ser encontradas no arquivo _Diagnostics-slbstateResults. JSON_ nesse diretório.

Esse arquivo JSON pode ser dividido nas seguintes seções:
 * Malha
   * SlbmVips-esta seção lista o endereço IP do endereço VIP do Gerenciador SLB que é usado pelo controlador de rede para coodinate a configuração e a integridade entre os agentes SLB Muxes e SLB host.
   * MuxState-esta seção listará um valor para cada SLB MUX implantado, fornecendo o estado do MUX
   * Configuração do roteador-esta seção listará o número do sistema autônomo (peer do BGP) do roteador upstream (ASN par), o endereço IP de trânsito e a ID. Ele também listará o IP do SLB Muxes ASN e de trânsito.
   * Informações do host conectado-esta seção listará o endereço IP de gerenciamento todos os hosts do Hyper-V disponíveis para executar cargas de trabalho com balanceamento de carga.
   * Intervalos VIP-esta seção listará os intervalos de pool de IPS VIP público e privado. O VIP do SLBM será incluído como um IP alocado de um desses intervalos. 
   * Rotas do MUX – esta seção listará um valor para cada SLB MUX implantado contendo todos os anúncios de rota para esse MUX específico.
 * Locatário
   * VipConsolidatedState-esta seção listará o estado de conectividade para cada VIP de locatário, incluindo prefixo de rota anunciado, host Hyper-V e pontos de extremidade DIP.

> [!NOTE]
> O estado de SLB pode ser determinado diretamente usando o script [DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1) disponível no [repositório GITHUB do Microsoft Sdn](https://github.com/microsoft/sdn). 

#### <a name="gateway-validation"></a>Validação do gateway

**Do controlador de rede:**
```
Get-NetworkControllerLogicalNetwork
Get-NetworkControllerPublicIPAddress
Get-NetworkControllerGatewayPool
Get-NetworkControllerGateway
Get-NetworkControllerVirtualGateway
Get-NetworkControllerNetworkInterface
```

**Da VM de gateway:**
```
Ipconfig /allcompartments /all
Get-NetRoute -IncludeAllCompartments -AddressFamily
Get-NetBgpRouter
Get-NetBgpRouter | Get-BgpPeer
Get-NetBgpRouter | Get-BgpRouteInformation
```

**Na opção superior do rack (ToR):**

`sh ip bgp summary (for 3rd party BGP Routers)`

**Roteador BGP do Windows**
```
Get-BgpRouter
Get-BgpPeer
Get-BgpRouteInformation
```

Além disso, dos problemas que vimos até agora (especialmente em implantações baseadas em SDNExpress), o motivo mais comum para o compartimento de locatário não ser configurado em VMs GW parece ser o fato de que a capacidade GW em FabricConfig. psd1 é menos comparada ao que as pessoas tentam atribuir às conexões de rede (Túneis S2S) em TenantConfig. psd1. Isso pode ser verificado facilmente por meio da comparação de saídas dos seguintes comandos:

```
PS > (Get-NetworkControllerGatewayPool -ConnectionUri $uri).properties.Capacity
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").properties.OutboundKiloBitsPerSecond
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").property
```

### <a name="hoster-validate-data-plane"></a>Hoster Validar plano de dados
Depois que o controlador de rede foi implantado, as redes virtuais de locatário e as sub-redes foram criadas e as VMs foram anexadas às sub-redes virtuais, testes de nível de malha adicionais podem ser executados pelo hoster para verificar a conectividade do locatário.

#### <a name="check-hnv-provider-logical-network-connectivity"></a>Verificar conectividade de rede lógica do provedor de HNV
Depois que a primeira VM convidada em execução em um host Hyper-V tiver sido conectada a uma rede virtual de locatário, o controlador de rede atribuirá dois endereços IP do provedor de HNV (endereços IP PA) ao host Hyper-V. Esses IPs serão provenientes do pool de IPS da rede lógica do provedor de HNV e serão gerenciados pelo controlador de rede.  Para descobrir quais são esses dois endereços IP de HNV

```none
PS > Get-ProviderAddress

# Sample Output
ProviderAddress : 10.10.182.66
MAC Address     : 40-1D-D8-B7-1C-04
Subnet Mask     : 255.255.255.128
Default Gateway : 10.10.182.1
VLAN            : VLAN11

ProviderAddress : 10.10.182.67
MAC Address     : 40-1D-D8-B7-1C-05
Subnet Mask     : 255.255.255.128
Default Gateway : 10.10.182.1
VLAN            : VLAN11
```

Esses endereços IP do provedor de HNV (PA IPs) são atribuídos a adaptadores de Ethernet criados em um compartimento de rede TCPIP separado e têm um nome de adaptador de _VLANX_ em que X é a VLAN atribuída à rede lógica do provedor de HNV (transporte).

A conectividade entre dois hosts Hyper-V usando a rede lógica do provedor HNV pode ser feita por um ping com um parâmetro de compartimento adicional (-c Y) em que Y é o compartimento de rede TCPIP no qual o PAhostVNICs é criado. Esse compartimento pode ser determinado executando:

```none
C:\> ipconfig /allcompartments /all

<snip> ...
==============================================================================
Network Information for *Compartment 3*
==============================================================================
   Host Name . . . . . . . . . . . . : SA18n30-2
<snip> ...

Ethernet adapter VLAN11:

   Connection-specific DNS Suffix  . :
   Description . . . . . . . . . . . : Microsoft Hyper-V Network Adapter
   Physical Address. . . . . . . . . : 40-1D-D8-B7-1C-04
   DHCP Enabled. . . . . . . . . . . : No
   Autoconfiguration Enabled . . . . : Yes
   Link-local IPv6 Address . . . . . : fe80::5937:a365:d135:2899%39(Preferred)
   IPv4 Address. . . . . . . . . . . : 10.10.182.66(Preferred)
   Subnet Mask . . . . . . . . . . . : 255.255.255.128
   Default Gateway . . . . . . . . . : 10.10.182.1
   NetBIOS over Tcpip. . . . . . . . : Disabled

Ethernet adapter VLAN11:

   Connection-specific DNS Suffix  . :
   Description . . . . . . . . . . . : Microsoft Hyper-V Network Adapter
   Physical Address. . . . . . . . . : 40-1D-D8-B7-1C-05
   DHCP Enabled. . . . . . . . . . . : No
   Autoconfiguration Enabled . . . . : Yes
   Link-local IPv6 Address . . . . . : fe80::28b3:1ab1:d9d9:19ec%44(Preferred)
   IPv4 Address. . . . . . . . . . . : 10.10.182.67(Preferred)
   Subnet Mask . . . . . . . . . . . : 255.255.255.128
   Default Gateway . . . . . . . . . : 10.10.182.1
   NetBIOS over Tcpip. . . . . . . . : Disabled

*Ethernet adapter vEthernet (PAhostVNic):*
<snip> ...
```

>[!NOTE]
> Os adaptadores de host vNIC do PA não são usados no caminho de dados e, portanto, não têm um IP atribuído ao adaptador "vEthernet (PAhostVNic)".

Por exemplo, suponha que os hosts Hyper-V 1 e 2 tenham endereços IP de provedor de HNV (PA) de:

|-Host Hyper-V-|-PA endereço IP 1|-PA endereço IP 2|
|---             |---            |---             |
|Host 1 | 10.10.182.64 | 10.10.182.65 |
|Host 2 | 10.10.182.66 | 10.10.182.67 |

Podemos executar ping entre os dois usando o comando a seguir para verificar a conectividade de rede lógica do provedor HNV.

```none
# Ping the first PA IP Address on Hyper-V Host 2 from the first PA IP address on Hyper-V Host 1 in compartment (-c) 3
C:\> ping -c 3 10.10.182.66 -S 10.10.182.64

# Ping the second PA IP Address on Hyper-V Host 2 from the first PA IP address on Hyper-V Host 1 in compartment (-c) 3
C:\> ping -c 3 10.10.182.67 -S 10.10.182.64

# Ping the first PA IP Address on Hyper-V Host 2 from the second PA IP address on Hyper-V Host 1 in compartment (-c) 3
C:\> ping -c 3 10.10.182.66 -S 10.10.182.65

# Ping the second PA IP Address on Hyper-V Host 2 from the second PA IP address on Hyper-V Host 1 in compartment (-c) 3
C:\> ping -c 3 10.10.182.67 -S 10.10.182.65
```

*Correção* Se o ping do provedor HNV não funcionar, verifique a conectividade da rede física, incluindo a configuração de VLAN. As NICs físicas em cada host Hyper-V devem estar no modo de tronco sem uma VLAN específica atribuída. O host de gerenciamento vNIC deve ser isolado para a VLAN da rede lógica de gerenciamento.

```none
PS C:\> Get-NetAdapter "Ethernet 4" |fl

Name                       : Ethernet 4
InterfaceDescription       : <NIC> Ethernet Adapter
InterfaceIndex             : 2
MacAddress                 : F4-52-14-55-BC-21
MediaType                  : 802.3
PhysicalMediaType          : 802.3
InterfaceOperationalStatus : Up
AdminStatus                : Up
LinkSpeed(Gbps)            : 10
MediaConnectionState       : Connected
ConnectorPresent           : True
*VlanID                     : 0*
DriverInformation          : Driver Date 2016-08-28 Version 5.25.12665.0 NDIS 6.60

# VMM uses the older PowerShell cmdlet <Verb>-VMNetworkAdapterVlan to set VLAN isolation
PS C:\> Get-VMNetworkAdapterVlan -ManagementOS -VMNetworkAdapterName <Mgmt>

VMName VMNetworkAdapterName Mode     VlanList
------ -------------------- ----     --------
<snip> ...        
       Mgmt                 Access   7
<snip> ...

# SDNExpress deployments use the newer PowerShell cmdlet <Verb>-VMNetworkAdapterIsolation to set VLAN isolation
PS C:\> Get-VMNetworkAdapterIsolation -ManagementOS

<snip> ...

IsolationMode        : Vlan
AllowUntaggedTraffic : False
DefaultIsolationID   : 7
MultiTenantStack     : Off
ParentAdapter        : VMInternalNetworkAdapter, Name = 'Mgmt'
IsTemplate           : True
CimSession           : CimSession: .
ComputerName         : SA18N30-2
IsDeleted            : False

<snip> ...
```

#### <a name="check-mtu-and-jumbo-frame-support-on-hnv-provider-logical-network"></a>Verificar o MTU e o suporte a quadros Jumbo na rede lógica do provedor de HNV

Outro problema comum na rede lógica do provedor de HNV é que as portas de rede física e/ou a placa Ethernet não têm uma MTU grande o suficiente configurada para lidar com a sobrecarga do encapsulamento VXLAN (ou NVGRE). 
>[!NOTE]
> Alguns drivers e placas Ethernet dão suporte à nova palavra-chave * EncapOverhead, que será automaticamente definida pelo agente de host do controlador de rede para um valor de 160. Esse valor será então adicionado ao valor da palavra-chave * JumboPacket cujo somatório é usado como a MTU anunciada.
> por exemplo, * EncapOverhead = 160 e * JumboPacket = 1514 = > MTU = 1674B

```none
# Check whether or not your Ethernet card and driver support *EncapOverhead
PS C:\ > Test-EncapOverheadSettings

Verifying Physical Nic : <NIC> Ethernet Adapter #2
Physical Nic  <NIC> Ethernet Adapter #2 can support SDN traffic. Encapoverhead value set on the nic is  160
Verifying Physical Nic : <NIC> Ethernet Adapter
Physical Nic  <NIC> Ethernet Adapter can support SDN traffic. Encapoverhead value set on the nic is  160
```

Para testar se a rede lógica do provedor de HNV dá suporte ao tamanho de MTU maior de ponta a ponta, use o cmdlet _Test-LogicalNetworkSupportsJumboPacket_ :
```none
# Get credentials for both source host and destination host (or use the same credential if in the same domain)
$sourcehostcred = Get-Credential
$desthostcred = Get-Credential

# Use the Management IP Address or FQDN of the Source and Destination Hyper-V hosts
Test-LogicalNetworkSupportsJumboPacket -SourceHost sa18n30-2 -DestinationHost sa18n30-3 -SourceHostCreds $sourcehostcred -DestinationHostCreds $desthostcred

# Failure Results
SourceCompartment : 3
pinging Source PA: 10.10.182.66 to Destination PA: 10.10.182.64 with Payload: 1632
pinging Source PA: 10.10.182.66 to Destination PA: 10.10.182.64 with Payload: 1472
Checking if physical nics support jumbo packets on host
Physical Nic  <NIC> Ethernet Adapter #2 can support SDN traffic. Encapoverhead value set on the nic is  160
Cannot send jumbo packets to the destination. Physical switch ports may not be configured to support jumbo packets.
Checking if physical nics support jumbo packets on host
Physical Nic  <NIC> Ethernet Adapter #2 can support SDN traffic. Encapoverhead value set on the nic is  160
Cannot send jumbo packets to the destination. Physical switch ports may not be configured to support jumbo packets.

# TODO: Success Results aftering updating MTU on physical switch ports
```

*Remediação*
* Ajuste o tamanho de MTU nas portas do comutador físico para que seja pelo menos 1674B (incluindo o cabeçalho e o trailer de Ethernet 14B)
* Se a placa NIC não oferecer suporte à palavra-chave EncapOverhead, ajuste a palavra-chave JumboPacket para ter pelo menos 1674B


#### <a name="check-tenant-vm-nic-connectivity"></a>Verificar conectividade NIC de VM de locatário
Cada NIC de VM atribuída a uma VM convidada tem um mapeamento de CA-PA entre o endereço do cliente privado (CA) e o espaço de endereço do provedor HNV (PA). Esses mapeamentos são mantidos nas tabelas do servidor OVSDB em cada host do Hyper-V e podem ser encontrados executando o cmdlet a seguir.

```none
# Get all known PA-CA Mappings from this particular Hyper-V Host
PS > Get-PACAMapping

CA IP Address CA MAC Address    Virtual Subnet ID PA IP Address
------------- --------------    ----------------- -------------
10.254.254.2  00-1D-D8-B7-1C-43              4115 10.10.182.67
10.254.254.3  00-1D-D8-B7-1C-43              4115 10.10.182.67
192.168.1.5   00-1D-D8-B7-1C-07              4114 10.10.182.65
10.254.254.1  40-1D-D8-B7-1C-06              4115 10.10.182.66
192.168.1.1   40-1D-D8-B7-1C-06              4114 10.10.182.66
192.168.1.4   00-1D-D8-B7-1C-05              4114 10.10.182.66
```
>[!NOTE]
> Se os mapeamentos de CA-PA esperados não forem impressos para uma determinada VM de locatário, verifique os recursos de configuração de IP e NIC de VM no controlador de rede usando o cmdlet _Get-NetworkControllerNetworkInterface_ . Além disso, verifique as conexões estabelecidas entre o agente do host NC e os nós do controlador de rede.

Com essas informações, um ping de VM de locatário agora pode ser iniciado pelo hoster do controlador de rede usando o cmdlet _Test-VirtualNetworkConnection_ .

## <a name="specific-troubleshooting-scenarios"></a>Cenários de solução de problemas específicos

As seções a seguir fornecem orientações para solucionar problemas de cenários específicos.

### <a name="no-network-connectivity-between-two-tenant-virtual-machines"></a>Nenhuma conectividade de rede entre duas máquinas virtuais de locatário

1.  Vários Verifique se o Firewall do Windows em máquinas virtuais de locatário não está bloqueando o tráfego.  
2.  Vários Verifique se os endereços IP foram atribuídos à máquina virtual de locatário executando _ipconfig_. 
3.  Hoster Execute **Test-VirtualNetworkConnection** do host Hyper-V para validar a conectividade entre as duas máquinas virtuais de locatário em questão. 

>[!NOTE]
>O VSID refere-se à ID de sub-rede virtual. No caso de VXLAN, esse é o VNI (identificador de rede VXLAN). Você pode encontrar esse valor executando o cmdlet **Get-PACAMapping** .

#### <a name="example"></a>Exemplo

    $password = ConvertTo-SecureString -String "password" -AsPlainText -Force
    $cred = New-Object pscredential -ArgumentList (".\administrator", $password) 

Criar CA-ping entre "VM verde da Web 1" com SenderCA IP de 192.168.1.4 no host "sa18n30-2.sa18.nttest.microsoft.com" com IP de gerenciamento de 10.127.132.153 para ListenerCA IP de 192.168.1.5 ambos anexados à sub-rede virtual (VSID) 4114.

    Test-VirtualNetworkConnection -OperationId 27 -HostName sa18n30-2.sa18.nttest.microsoft.com -MgmtIp 10.127.132.153 -Creds $cred -VMName "Green Web VM 1" -VMNetworkAdapterName "Green Web VM 1" -SenderCAIP 192.168.1.4 -SenderVSID 4114 -ListenerCAIP 192.168.1.5 -ListenerVSID 4114

    Test-VirtualNetworkConnection at command pipeline position 1

Iniciando a AC-espaço de ping teste iniciando sessão de rastreamento ping para 192.168.1.5 bem-sucedido do endereço 192.168.1.4 RTT = 0 ms


Informações de roteamento de CA:

    Local IP: 192.168.1.4
    Local VSID: 4114
    Remote IP: 192.168.1.5
    Remote VSID: 4114
    Distributed Router Local IP: 192.168.1.1
    Distributed Router Local MAC: 40-1D-D8-B7-1C-06
    Local CA MAC: 00-1D-D8-B7-1C-05
    Remote CA MAC: 00-1D-D8-B7-1C-07
    Next Hop CA MAC Address: 00-1D-D8-B7-1C-07

Informações de roteamento do PA:

    Local PA IP: 10.10.182.66
    Remote PA IP: 10.10.182.65

 <snip> ...

4. Vários Verifique se não há políticas de firewall distribuídas especificadas na sub-rede virtual ou nas interfaces de rede de VM que bloqueiam o tráfego.    

Consulte a API REST do controlador de rede encontrada no ambiente de demonstração em sa18n30nc no domínio sa18.nttest.microsoft.com.

    $uri = "https://sa18n30nc.sa18.nttest.microsoft.com"
    Get-NetworkControllerAccessControlList -ConnectionUri $uri 

## <a name="look-at-ip-configuration-and-virtual-subnets-which-are-referencing-this-acl"></a>Examine a configuração de IP e as sub-redes virtuais que fazem referência a essa ACL

1. Hoster Execute ``Get-ProviderAddress`` em ambos os hosts Hyper-V que hospedam as duas máquinas virtuais de locatário em questão e, em seguida, execute ``Test-LogicalNetworkConnection`` ou ``ping -c <compartment>`` do host Hyper-V para validar a conectividade na rede lógica do provedor de HNV
2.  Hoster Verifique se as configurações de MTU estão corretas nos hosts Hyper-V e em qualquer dispositivo de comutação de camada 2 entre os hosts Hyper-V. Execute ``Test-EncapOverheadValue`` em todos os hosts Hyper-V em questão. Verifique também se todos os comutadores de camada 2 no entre têm MTU definido como menos 1674 bytes para considerar a sobrecarga máxima de 160 bytes.  
3.  Hoster Se os endereços IP PA não estiverem presentes e/ou se a conectividade de CA for interrompida, verifique se a política de rede foi recebida. Execute ``Get-PACAMapping`` para ver se as regras de encapsulamento e os mapeamentos de CA-PA necessários para a criação de redes virtuais de sobreposição são estabelecidos corretamente.  
4.  Hoster Verifique se o agente de host do controlador de rede está conectado ao controlador de rede. Execute ``netstat -anp tcp |findstr 6640`` para ver se o   
5.  Hoster Verifique se a ID do host em HKLM/corresponde à ID da instância dos recursos do servidor que hospeda as máquinas virtuais de locatário.  
6. Hoster Verifique se a ID do perfil de porta corresponde à ID de instância das interfaces de rede de VM das máquinas virtuais de locatário.  

## <a name="logging-tracing-and-advanced-diagnostics"></a>Registro em log, rastreamento e diagnóstico avançado

As seções a seguir fornecem informações sobre diagnóstico, registro em log e rastreamento avançados.

### <a name="network-controller-centralized-logging"></a>Log centralizado do controlador de rede 

O controlador de rede pode coletar automaticamente os logs do depurador e armazená-los em um local centralizado. A coleta de log pode ser habilitada quando você implanta o controlador de rede pela primeira vez ou a qualquer hora depois. Os logs são coletados do controlador de rede e elementos de rede gerenciados pelo controlador de rede: máquinas host, SLB (balanceadores de carga de software) e máquinas de gateway. 

Esses logs incluem logs de depuração para o cluster do controlador de rede, o aplicativo do controlador de rede, os logs do gateway, o SLB, a rede virtual e o firewall distribuído. Sempre que um novo host/SLB/gateway é adicionado ao controlador de rede, o registro em log é iniciado nesses computadores. Da mesma forma, quando um host/SLB/gateway é removido do controlador de rede, o registro em log é interrompido nessas máquinas.

#### <a name="enable-logging"></a>Habilitar registro em log

O registro em log é habilitado automaticamente quando você instala o cluster do controlador de rede usando o cmdlet **install-NetworkControllerCluster** . Por padrão, os logs são coletados localmente nos nós do controlador de rede em *%systemdrive%\SDNDiagnostics*. É **altamente recomendável** que você altere esse local para que seja um compartilhamento de arquivos remoto (não local). 

Os logs de cluster do controlador de rede são armazenados em *%ProgramData%\Windows Fabric\log\Traces*. Você pode especificar um local centralizado para a coleta de log com o parâmetro **DiagnosticLogLocation** com a recomendação de que esse também seja um compartilhamento de arquivo remoto. 

Se você quiser restringir o acesso a esse local, você pode fornecer as credenciais de acesso com o parâmetro **LogLocationCredential** . Se você fornecer as credenciais para acessar o local do log, você também deverá fornecer o parâmetro **CredentialEncryptionCertificate** , que é usado para criptografar as credenciais armazenadas localmente nos nós do controlador de rede.  

Com as configurações padrão, é recomendável que você tenha pelo menos 75 GB de espaço livre no local central e 25 GB nos nós locais (se não estiver usando um local central) para um cluster de controlador de rede de 3 nós.

#### <a name="change-logging-settings"></a>Alterar configurações de log

Você pode alterar as configurações de log a qualquer momento usando o cmdlet ``Set-NetworkControllerDiagnostic``. As seguintes configurações podem ser alteradas:

- **Local de log centralizado**.  Você pode alterar o local para armazenar todos os logs, com o parâmetro ``DiagnosticLogLocation``.  
- **Credenciais para acessar o local do log**.  Você pode alterar as credenciais para acessar o local do log, com o parâmetro ``LogLocationCredential``.  
- **Mover para o log local**.  Se você tiver fornecido local centralizado para armazenar logs, poderá voltar para o log localmente nos nós do controlador de rede com o parâmetro ``UseLocalLogLocation`` (não recomendado devido a grandes requisitos de espaço em disco).  
- **Escopo de log**.  Por padrão, todos os logs são coletados. Você pode alterar o escopo para coletar somente os logs de cluster do controlador de rede.  
- **Nível de log**.  O nível de log padrão é informativo. Você pode alterá-lo para erro, aviso ou detalhado.  
- **Tempo de envelhecimento do log**.  Os logs são armazenados de maneira circular. Você terá 3 dias de dados de registro em log por padrão, independentemente de usar o log local ou o registro em log centralizado. Você pode alterar esse limite de tempo com o parâmetro **LogTimeLimitInDays** .  
- **Tamanho de envelhecimento do log**.  Por padrão, você terá um máximo de 75 GB de dados de log se estiver usando o registro em log centralizado e 25 GB se estiver usando o registro em log local. Você pode alterar esse limite com o parâmetro **LogSizeLimitInMBs** .

#### <a name="collecting-logs-and-traces"></a>Coletando logs e rastreamentos

As implantações do VMM usam o registro em log centralizado para o controlador de rede por padrão. O local de compartilhamento de arquivos para esses logs é especificado ao implantar o modelo de serviço do controlador de rede.

Se um local de arquivo não tiver sido especificado, o log local será usado em cada nó do controlador de rede com logs salvos em C:\Windows\tracing\SDNDiagnostics. Esses logs são salvos usando a seguinte hierarquia:

- CrashDumps
- NCApplicationCrashDumps
- NCApplicationLogs
- PerfCounters
- SDNDiagnostics
- Rastreamentos

O controlador de rede usa o Service Fabric do Azure. Service Fabric logs podem ser necessários ao solucionar determinados problemas. Esses logs podem ser encontrados em cada nó do controlador de rede na malha do C:\ProgramData\Microsoft\Service.

Se um usuário tiver executado o cmdlet _debug-NetworkController_ , logs adicionais estarão disponíveis em cada host Hyper-V que foi especificado com um recurso de servidor no controlador de rede. Esses logs (e rastreamentos, se habilitados) são mantidos em C:\NCDiagnostics

### <a name="slb-diagnostics"></a>Diagnóstico de SLB

#### <a name="slbm-fabric-errors-hosting-service-provider-actions"></a>Erros de malha do SLBM (ações do provedor de serviço de hospedagem)

1.  Verifique se o SLBM (Gerenciador de Load Balancer de software) está funcionando e se as camadas de orquestração podem se comunicar entre si: SLBM-> SLB MUX e SLBM-> agentes de host SLB. Execute [DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1) de qualquer nó com acesso ao ponto de extremidade REST do controlador de rede.  
2.  Valide o *SDNSLBMPerfCounters* no perfmon em uma das VMs do nó do controlador de rede (preferivelmente o nó do controlador de rede primário-get-NetworkControllerReplica):
    1.  O mecanismo de Load Balancer (LB) está conectado ao SLBM? (*Total de configurações de LBEngine do SLBM* > 0)  
    2.  O SLBM não conhece pelo menos seus pontos de extremidade? (*Total de pontos de extremidade VIP* > = 2)  
    3.  Os hosts do Hyper-V (DIP) estão conectados ao SLBM? (*Clientes HP conectados* = = número de servidores)   
    4.  O SLBM está conectado ao Muxes? (*Muxes conectado* == *MUXES íntegro no SLBM* == *Muxes Reporting íntegro* = # SLB Muxes VMS).  
3.  Verifique se o roteador BGP configurado está emparelhando com êxito com o SLB MUX  
    1.  Se estiver usando o RRAS com acesso remoto (ou seja, máquina virtual BGP):  
        1.  Get-BgpPeer deve mostrar conectado  
        2.  Get-BgpRouteInformation deve mostrar pelo menos uma rota para o próprio VIP de SLBM  
    2.  Se estiver usando o comutador ToR (Top-of-rack físico) como par de BGP, consulte a documentação  
        1.  Por exemplo: # show BGP Instance  
4.  Validar os contadores *SlbMuxPerfCounters* e *SLBMUX* no perfmon na VM SLB MUX
5.  Verificar o estado de configuração e os intervalos VIP no recurso do Gerenciador de Load Balancer de software  
    1.  Get-NetworkControllerLoadBalancerConfiguration-Conexãouri < https://<FQDN or IP>| ConvertTo-JSON-Depth 8 (verifique os intervalos VIP em pools de IPS e garanta o*LoadBalanacerManagerIPAddress*(autovip) de SLBM e quaisquer VIPs voltados para o locatário estão dentro desses intervalos)  
        1. Get-NetworkControllerIpPool-networkid "< ID de recurso de rede lógica VIP pública/privada >"-Subnetid "< ID de recurso de sub-rede lógica VIP/privada >"-ResourceId "<IP Pool Resource Id>"-Conexãouri $uri | ConvertTo-JSON-Depth 8 
    2.  Debug-NetworkControllerConfigurationState-  

Se qualquer uma das verificações acima falhar, o estado SLB do locatário também estará em um modo de falha.  

  de **correção**  
Com base nas seguintes informações de diagnóstico apresentadas, corrija o seguinte:  
* Garantir que os multiplexadores SLB estejam conectados  
  * Corrigir problemas de certificado  
  * Corrigir problemas de conectividade de rede  
* Verifique se as informações de emparelhamento BGP foram configuradas com êxito  
* Verifique se a ID do host no registro corresponde à ID da instância do servidor no recurso do servidor (apêndice de referência para o código de erro *HostNotConnected* )  
* Coletar registros  

#### <a name="slbm-tenant-errors-hosting-service-provider--and-tenant-actions"></a>Erros de locatário do SLBM (provedor de serviços de hospedagem e ações de locatário)

1.  Hoster Marque *debug-NetworkControllerConfigurationState* para ver se há recursos de balanceador de carga em estado de erro. Tente mitigar seguindo a tabela itens de ação no apêndice.   
    1.  Verificar se um ponto de extremidade VIP está presente e anunciar rotas  
    2.  Verifique quantos pontos de extremidade DIP foram descobertos para o ponto de extremidades VIP  
2.  Vários Validar Load Balancer recursos especificados corretamente  
    1.  Validar pontos de extremidade DIP que são registrados no SLBM que hospedam máquinas virtuais de locatário que correspondem às configurações de IP do pool de endereços de back-end do balanceador de carga  
3.  Hoster Se os pontos de extremidade DIP não forem descobertos ou conectados:   
    1.  Verificar *debug-NetworkControllerConfigurationState*  
        1.  Valide se o NC e o agente de host SLB estão conectados com êxito ao coordenador de eventos do controlador de rede usando ``netstat -anp tcp |findstr 6640)``  
    2.  Verifique o *hostid* na RegKey do serviço *nchostagent* (o código de erro *HostNotConnected* de referência no apêndice) corresponde à ID de instância do recurso de servidor correspondente (``Get-NCServer |convertto-json -depth 8``)  
    3.  Verificar a ID do perfil da porta para a porta da máquina virtual corresponde à ID da instância do recurso NIC da máquina virtual correspondente   
4.  [Provedor de hospedagem] Coletar logs   

#### <a name="slb-mux-tracing"></a>Rastreamento de MUX SLB

As informações do software Load Balancer Muxes também podem ser determinadas por meio de Visualizador de Eventos. 
1. Clique em "Mostrar logs analíticos e de depuração" no menu Visualizador de Eventos exibição
2. Navegue até "logs de aplicativos e serviços" > Microsoft > Windows > SlbMuxDriver > rastreamento no Visualizador de Eventos 
3. Clique com o botão direito do mouse nele e selecione "habilitar log"

>[!NOTE]
>É recomendável que você só tenha esse log habilitado por um curto período enquanto estiver tentando reproduzir um problema

### <a name="vfp-and-vswitch-tracing"></a>Rastreamento de VFP e vSwitch

A partir de qualquer host Hyper-V que esteja hospedando uma VM convidada anexada a uma rede virtual de locatário, você pode coletar um rastreamento VFP para determinar onde os problemas podem estar.

```
netsh trace start provider=Microsoft-Windows-Hyper-V-VfpExt overwrite=yes tracefile=vfp.etl report=disable provider=Microsoft-Windows-Hyper-V-VmSwitch 
netsh trace stop
netsh trace convert .\vfp.etl ov=yes 
```
