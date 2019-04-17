---
title: A pilha de rede definidos do Windows Server Software de solução de problemas
description: Este guia do Windows Server examina a cenários de falhas e erros comuns de rede definidos Software (SDN) e descreve um fluxo de trabalho de solução de problemas que aproveita as ferramentas de diagnóstico disponíveis.
manager: ravirao
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 9be83ed2-9e62-49e8-88e7-f52d3449aac5
ms.author: pashort
author: JMesser81
ms.openlocfilehash: af59ae6746467f9aecf384d1b3cf9af1e8baeb9a
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="troubleshoot-the-windows-server-software-defined-networking-stack"></a>A pilha de rede definidos do Windows Server Software de solução de problemas

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este guia examina a cenários de falhas e erros comuns de rede definidos Software (SDN) e descreve um fluxo de trabalho de solução de problemas que aproveita as ferramentas de diagnóstico disponíveis.  

Para saber mais sobre Software definido de rede da Microsoft, consulte [Software de rede definidos](../../sdn/Software-Defined-Networking--SDN-.md).  

## <a name="error-types"></a>Tipos de erros  
A lista a seguir representa a classe dos problemas mais comuns com a virtualização de rede do Hyper-V (HNVv1) no Windows Server 2012 R2 de implantações de produção existentes no mercado e coincida de muitas formas com os mesmos tipos de problemas vistos no Windows Server 2016 HNVv2 com a nova pilha de rede de definido de Software (SDN).  

A maioria dos erros podem ser classificados em um pequeno conjunto de classes:   
* **Configuração inválida ou sem suporte**  
   Um usuário invoca a API em sentido norte incorretamente ou com a política inválida.   

* **Erro na aplicação de políticas**  
     Política de controlador de rede não foi entregue a um Host do Hyper-V, significativamente atrasado e / ou não atualizados em todos os hosts Hyper-V (por exemplo, após a migração Live).  
* **Bug de desvio ou software de configuração**  
 Problemas de caminho de dados resultando em pacotes removidos.  

* **Erro externo relacionados ao hardware NIC / drivers ou a base de malha de rede**  
 Descarrega tarefa com comportamento inadequado (como VMQ) ou base malha de rede definidas incorretamente (como MTU)   

 Este guia de solução de problemas examina cada uma dessas categorias de erro e recomenda as práticas recomendadas e ferramentas de diagnóstico disponíveis para identificar e corrigir o erro.  

## <a name="diagnostic-tools"></a>Ferramentas de diagnóstico  

Antes de abordar os fluxos de trabalho de solução de problemas para cada um desses tipos de erros, vamos examinar as ferramentas de diagnóstico disponíveis.   
  
Para usar as ferramentas de diagnóstico de controlador de rede (caminho do controle), primeiro você deve instalar o recurso RSAT-NetworkController e importar o ``NetworkControllerDiagnostics`` módulo:  

```  
Add-WindowsFeature RSAT-NetworkController -IncludeManagementTools  
Import-Module NetworkControllerDiagnostics  
```  

Para usar as ferramentas de diagnóstico de diagnóstico HNV (caminho de dados), você deve importar o ``HNVDiagnostics`` módulo:
  
```  
# Assumes RSAT-NetworkController feature has already been installed
Import-Module hnvdiagnostics   
```  

### <a name="network-controller-diagnostics"></a>Diagnóstico de controlador de rede  
Esses cmdlets estão documentados no TechNet no [rede controlador diagnóstico Cmdlet tópico](https://docs.microsoft.com/en-us/powershell/module/networkcontrollerdiagnostics/). Ele ajuda a identificar problemas com a consistência de política de rede no controle-caminho entre nós de controlador de rede e entre o controlador de rede e os agentes do Host NC em execução nos hosts Hyper-V.

 O _depurar ServiceFabricNodeStatus_ e _Get-NetworkControllerReplica_ cmdlets deve ser executados em um das máquinas de virtuais de nó do controlador de rede. Todos os outros cmdlets NC diagnóstico pode ser executados em qualquer host que tenha conectividade com o controlador de rede e está no grupo de segurança do gerenciamento de controlador de rede (Kerberos) ou tem acesso ao certificado x. 509 para gerenciar o controlador de rede. 
   
### <a name="hyper-v-host-diagnostics"></a>Diagnóstico de host do Hyper-V  
Esses cmdlets estão documentados no TechNet no [virtualização de rede do Hyper-V (HNV) diagnóstico Cmdlet tópico](https://docs.microsoft.com/en-us/powershell/module/hnvdiagnostics/). Ele ajuda a identificar problemas no caminho de dados entre máquinas virtuais de locatário (Leste/Oeste) e tráfego de entrada por meio de um VIP SLB (Norte/do Sul). 

O _depurar VirtualMachineQueueOperation_, _Get-CustomerRoute_, _Get-PACAMapping_, _Get-ProviderAddress_, _Get-VMNetworkAdapterPortId_, _Get-VMSwitchExternalPortId_, e _teste EncapOverheadSettings_ são todos os testes locais que podem ser executados em qualquer host do Hyper-V. Os outros cmdlets invocar testes de caminho de dados por meio do controlador de rede e, portanto, precisam de acesso ao controlador de rede como descried acima.
 
### <a name="github"></a>GitHub
O [Microsoft/SDN GitHub Repo](https://github.com/microsoft/sdn) possui um número de fluxos de trabalho que compilar sobre esses cmdlets nativos e scripts de exemplo. Em particular, os scripts de diagnósticos podem ser encontrados no [diagnóstico](https://github.com/Microsoft/sdn/diagnostics) pasta. Ajude-na contribuir para esses scripts ao enviar solicitações de puxar.

## <a name="troubleshooting-workflows-and-guides"></a>Solução de problemas de fluxos de trabalho e guias  

### <a name="hoster-validate-system-health"></a>[Hoster] Validar a integridade do sistema
Há um recurso interno chamado _configuração estado_ em vários dos recursos de controlador de rede. Estado de configuração fornece informações sobre a integridade do sistema incluindo a consistência entre configuration do controlador de rede e o estado real (em execução) nos hosts Hyper-V. 

Para verificar o estado de configuração, execute o seguinte em qualquer host do Hyper-V conectividade com o controlador de rede.

>[!NOTE] 
>O valor para o *NetworkController* parâmetro deve ser o FQDN ou o endereço IP com base no nome do requerente dos x. 509 > certificado criado para o controlador de rede.
>
>O *credenciais* parâmetro só precisa ser especificado se o controlador de rede está usando autenticação Kerberos (típica em implantações VMM). As credenciais devem ser para um usuário que esteja no grupo de segurança de rede controlador de gerenciamento.

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

Uma mensagem de estado da configuração de exemplo é mostrada abaixo:

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
> Há um bug no sistema onde os recursos de Interface de rede para o SLB multiplexador trânsito VM NIC estão em um estado de falha com erro "Virtual Switch – não conectado ao controlador de Host". Esse erro pode ser ignorado se a configuração de IP no recurso VM NIC for definida como um endereço IP do Pool de IP da rede lógico trânsito. Há um bug segundo no sistema em que os recursos de Interface de rede para o Gateway HNV provedor VM NICs estejam em um estado de falha com erro "Virtual Switch – PortBlocked". Esse erro pode também ser ignorado se a configuração de IP no recurso VM NIC for definida como nulo (por design).


A tabela a seguir mostra a lista de códigos de erro, mensagens e ações de acompanhamento a tomar com base no estado configuração observado.

  
| **Código**| **Mensagem**| **Ação**|  
|:--------:|:-----------:|----------:|  
| Desconhecido| Erro desconhecido| |  
| HostUnreachable                       | O computador host não está acessível | Verifique a conectividade de rede de gerenciamento entre o controlador de rede e Host |  
| PAIpAddressExhausted                  | Os endereços Ip PA esgotado | Aumente o tamanho de Pool de IP da sub-rede lógica HNV provedor |  
| PAMacAddressExhausted                 | Os endereços Mac PA esgotado | Aumentar o intervalo de Pool Mac |  
| PAAddressConfigurationFailure         | Falha ao inserir endereços PA para o host | Verifique a conectividade de rede de gerenciamento entre o controlador de rede e Host. |  
| CertificateNotTrusted                 | Certificado não é confiável  |Corrigi os certificados usados para comunicação com o host. |  
| CertificateNotAuthorized              | Certificado não autorizado | Corrigi os certificados usados para comunicação com o host. |  
| PolicyConfigurationFailureOnVfp       | Falha ao configurar as políticas VFP | Isso é uma falha de tempo de execução.  Não definido para contornar. Colete logs. |  
| PolicyConfigurationFailure            | Falha no empurrando políticas de hosts, devido a falhas de comunicação ou outros erros na NetworkController.| Nenhuma ação definitiva.  Isso é devido a falha em estado de meta de processamento em módulos o controlador de rede. Colete logs. |  
| HostNotConnectedToController          | O Host ainda não estiver conectado ao controlador de rede | Perfil de porta não aplicada no host ou o host não é acessível a partir do controlador de rede. Validar a chave do registro Identificação_do_host corresponde a ID da instância do recurso de servidor |  
| MultipleVfpEnabledSwitches            | Há vários VFp ativado comutadores no host  | Exclua uma das opções, desde que o agente de Host do controlador de rede só dá suporte a um vSwitch com a extensão VFP habilitada |  
| PolicyConfigurationFailure            | Falha ao push VNet políticas para um VmNic devido a erros de conectividade ou erros de certificado  | Verifique se os certificados adequados tiverem sido implantados (nome de entidade do certificado deve coincidir com o FQDN do host). Também verifique se a conectividade de host com o controlador de rede |  
| PolicyConfigurationFailure            | Falha ao push vSwitch políticas para um VmNic devido a erros de conectividade ou erros de certificado  | Verifique se os certificados adequados tiverem sido implantados (nome de entidade do certificado deve coincidir com o FQDN do host). Também verifique se a conectividade de host com o controlador de rede|
| PolicyConfigurationFailure            | Falha ao push políticas de Firewall para um VmNic devido a erros de conectividade ou erros de certificado | Verifique se os certificados adequados tiverem sido implantados (nome de entidade do certificado deve coincidir com o FQDN do host). Também verifique se a conectividade de host com o controlador de rede|
| DistributedRouterConfigurationFailure | Falha ao configurar as configurações do roteador distribuído em vNic o host                          | Erro de pilha TCPIP. Pode exigir limpando a vNICs PA e recuperação de desastres Host no servidor em que esse erro foi relatado |
| DhcpAddressAllocationFailure          | Falha de alocação de endereços DHCP para um VMNic                                                    | Verifique se o atributo de endereço IP estático está configurado no recurso NIC |  
| CertificateNotTrusted<br>CertificateNotAuthorized | Falha ao se conectar a multiplexador devido a erros de rede ou um certificado | Verifique o código numérico fornecido no código de mensagem de erro: isso corresponde ao código de erro winsock. Erros de certificado são granulares (por exemplo, o certificado não pode ser verificado, cert não autorizado, etc.) |  
| HostUnreachable                       | Multiplexador está com problemas (caso comum é BGPRouter desconectado) | Par BGP no RRAS (máquina virtual BGP) ou superior de Rack (ToR) switch está inacessível ou não com êxito. Verifique as configurações de BGP no recurso Software Load balanceador multiplexador e correspondente BGP (ToR ou RRAS máquina virtual) |  
| HostNotConnectedToController          | Agente de host SLB não está conectado  | Verifique se o serviço de agente de Host SLB está em execução; Consulte logs de agente de host SLB (auto executando) por motivos por que, no caso de SLBM (NC) rejeitadas o certificado apresentado pelo agente do host que executam o estado mostrará informações sutis  |  
| PortBlocked                           | A porta VFP estiver bloqueada, devido à falta de VNET / políticas ACL | Verifique se há quaisquer outros erros, que podem fazer com que as políticas que não será configurada. |  
| Sobrecarregado                            | LoadBalancer MUX é sobrecarregado  | Problema de desempenho com Multiplexador |  
| RoutePublicationFailure               | LoadBalancer MUX não estiver conectado a um roteador BGP | Verifique se Multiplexador tem conectividade com as roteadores BGP e estranhas que BGP está configurado corretamente |  
| VirtualServerUnreachable              | LoadBalancer MUX não está conectado ao Gerenciador de SLB | Verifique a conectividade entre SLBM e Multiplexador |  
| QosConfigurationFailure               | Falha ao configurar as políticas QOS | Veja se a largura de banda suficiente está disponível para todos os VM se reserva QOS é usada |


#### <a name="check-network-connectivity-between-the-network-controller-and-hyper-v-host-nc-host-agent-service"></a>Verifique a conectividade de rede entre o controlador de rede e o Host do Hyper-V (serviço de agente de Host NC)
Execute o *netstat* comando a seguir para validar que existem três conexões estabelecido entre o agente de Host NC e os nós de controlador de rede e um soquete de ESCUTA no Host do Hyper-V
- OUVINDO na porta TCP:6640 no Host do Hyper-V (serviço de agente de Host NC)
- Duas conexões estabelecidas do Hyper-V hospedam IP na porta 6640 para NC nó IP em portas efêmeras (> 32000)
- Uma conexão estabelecida de IP do host do Hyper-V na porta efêmera IP de REST de controlador de rede na porta 6640

>[!NOTE]
>Pode haver apenas duas conexões estabelecidas em um host do Hyper-V se não houver nenhum máquinas virtuais de locatário implantadas no host específico.

```none
netstat -anp tcp |findstr 6640

# Successful output
  TCP    0.0.0.0:6640           0.0.0.0:0              LISTENING
  TCP    10.127.132.153:6640    10.127.132.213:50095   ESTABLISHED
  TCP    10.127.132.153:6640    10.127.132.214:62514   ESTABLISHED
  TCP    10.127.132.153:50023   10.127.132.211:6640    ESTABLISHED
```
#### <a name="check-host-agent-services"></a>Verifique os serviços de agente de Host
O controlador de rede se comunica com dois serviços de agente de host nos hosts Hyper-V: SLB Host Agent e o agente de Host NC. É possível que um ou dois desses serviços não estiver em execução. Verifique seu estado e reinicie se eles não estiverem em execução.

```none
Get-Service SlbHostAgent
Get-Service NcHostAgent

# (Re)start requires -Force flag
Start-Service NcHostAgent -Force
Start-Service SlbHostAgent -Force
```

#### <a name="check-health-of-network-controller"></a>Verificar a integridade do controlador de rede
Se houver não três conexões estabelecido ou se o controlador de rede parece não responder, verifique se todos os nós e módulos de serviço estão em execução, usando cmdlets a seguir. 

```none
# Prints a DIFF state (status is automatically updated if state is changed) of a particular service module replica 
Debug-ServiceFabricNodeStatus [-ServiceTypeName] <Service Module>
```
Os módulos de serviço de controlador de rede são:
- ControllerService
- ApiService
- SlbManagerService
- ServiceInsertion
- FirewallService
- VSwitchService
- GatewayManager
- FnmService
- HelperService
- UpdateService

Verifique se está ReplicaStatus **pronto** e HealthState é **Okey**.

Em uma produção implantação é com um controlador de rede de vários nós, você também pode verificar quais nó cada serviço é primário e o status da réplica individual.

```none  
Get-NetworkControllerReplica

# Sample Output for the API service module
Replicas for service: ApiService

ReplicaRole   : Primary
NodeName      : SA18N30NC3.sa18.nttest.microsoft.com
ReplicaStatus : Ready

```
Verifique se o Status da réplica está pronto para cada serviço.
 
#### <a name="check-for-corresponding-hostids-and-certificates-between-network-controller-and-each-hyper-v-host"></a>Verifique se há IDs de host correspondente e certificados entre cada Host do Hyper-V e o controlador de rede 
Em um Host do Hyper-V, execute os seguintes comandos para verificar que a Identificação_do_host corresponde à identificação de instância de um recurso de servidor no controlador de rede

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

*Correção* se usando SDNExpress scripts ou implantação manual, atualize a chave Identificação_do_host no registro para corresponder a Id da instância do recurso de servidor. Reinicie o agente de Host do controlador de rede no host do Hyper-V (servidor físico) se usando VMM, excluir o Hyper-V Server do VMM e remova a chave de registro Identificação_do_host. Em seguida, adicione novamente o servidor por meio do VMM.


Verifique se as impressões digitais de certificados x. 509 usados pelo host do Hyper-V (o nome do host será nome do requerente do certificado) para a comunicação entre o Host do Hyper-V (serviço de agente de Host NC) e nós de controlador de rede (SouthBound) são os mesmos. Verifique também se o certificado do restante do controlador de rede tem nome do requerente do *CN =<FQDN or IP>*.

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

Você também pode verificar os seguintes parâmetros de cada certificado para garantir que o nome do requerente é o que é esperado (nome de host ou NC REST FQDN ou IP), o certificado ainda não expirou e que todas as autoridades de certificação na cadeia de certificados estão incluídas na autoridade raiz confiável.

- Nome do requerente  
- Data de vencimento  
- Autoridade raiz confiável  

*Correção* se vários certificados têm o mesmo nome de entidade no host do Hyper-V, o agente de Host do controlador de rede escolherá aleatoriamente um para apresentar ao controlador de rede. Isso pode não corresponder a impressão digital do recurso de servidor conhecido no controlador de rede. Nesse caso, excluir um dos certificados com o mesmo nome de entidade no host do Hyper-V e, em seguida, reiniciar o serviço de agente de Host do controlador de rede. Se uma conexão não ainda pode ser feita, excluir o outro certificado com o mesmo nome de entidade no Host do Hyper-V e exclua o recurso de servidor correspondente no VMM. Em seguida, recrie o recurso de servidor no VMM que irá gerar um novo certificado x. 509 e instalá-lo no host do Hyper-V.
  

#### <a name="check-the-slb-configuration-state"></a>Verificar o estado de configuração SLB
O estado da configuração SLB pode ser determinado como parte da saída para o cmdlet NetworkController de depuração. Este cmdlet também resultará no conjunto de recursos de rede controlador em arquivos JSON, todas as configurações de IP de cada host do Hyper-V (servidor) e a política de rede local de tabelas do banco de dados de agente de Host atual. 

Rastreamentos adicionais serão coletados por padrão. Para não coletar rastreamentos, adicione o IncludeTraces-: $false parâmetro.

```none
Debug-NetworkController -NetworkController <FQDN or IP> [-Credential <PS Credential>] [-IncludeTraces:$false]

# Don't collect traces
$cred = Get-Credential 
Debug-NetworkController -NetworkController 10.127.132.211 -Credential $cred -IncludeTraces:$false

Transcript started, output file is C:\\NCDiagnostics.log
Collecting Diagnostics data from NC Nodes
```

>[!NOTE]
>O local de saída padrão será o diretório de \NCDiagnostics\ < working_directory >. O diretório de saída padrão pode ser alterado usando o `-OutputDirectory` parâmetro. 

As informações de estado de configuração SLB podem ser encontradas no _diagnóstico slbstateResults.Json_ arquivo nesse diretório.

Este arquivo JSON pode ser dividido em seções a seguir:
 * Fabric
   * SlbmVips - esta seção lista o endereço IP do endereço SLB Manager VIP que é usado pelo controlador de rede para configuração coodinate e integridade entre o SLB Muxes e agentes de Host SLB.
   * MuxState - nesta seção irá listar um valor para cada multiplexador SLB implantado oferecendo o estado do multiplexador
   * Configuração de roteador - nesta seção listará Upstream do roteador (correspondente BGP) número de sistema autônomo (ASN), endereço de IP de trânsito e ID. Ele também listará o SLB Muxes ASN e IP de trânsito.
   * Conectado a informações de Host - esta seção lista IP gerenciamento tratarão todos os hosts Hyper-V disponíveis para ser executado balanceamento de carga cargas de trabalho.
   * Intervalos de VIP - esta seção lista os intervalos de pool de IP VIP públicos e privados. O VIP SLBM serão incluído como um IP alocado de um desses intervalos. 
   * Rotas multiplexador - nesta seção listará um valor para cada multiplexador SLB implantado que contém todos os anúncios de rotas para esse multiplexador específico.
 * Locatário
   * VipConsolidatedState - esta seção listará o estado de conectividade para cada VIP de locatário, incluindo o prefixo de rota anunciado, Host do Hyper-V e pontos de extremidade DIP.
    
> [!NOTE]
> Estado SLB pode ser determinado diretamente usando o [DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1) disponível no script o [repositório GitHub de SDN Microsoft](https://github.com/microsoft/sdn). 

#### <a name="gateway-validation"></a>Validação de gateway

**De controlador de rede:**
```
Get-NetworkControllerLogicalNetwork
Get-NetworkControllerPublicIPAddress
Get-NetworkControllerGatewayPool
Get-NetworkControllerGateway
Get-NetworkControllerVirtualGateway
Get-NetworkControllerNetworkInterface
```

**Do Gateway VM:**
```
Ipconfig /allcompartments /all
Get-NetRoute -IncludeAllCompartments -AddressFamily
Get-NetBgpRouter
Get-NetBgpRouter | Get-BgpPeer
Get-NetBgpRouter | Get-BgpRouteInformation
```

**Da parte superior do Switch Rack (ToR):**

`sh ip bgp summary (for 3rd party BGP Routers)`

**Windows BGP roteador**
```
Get-BgpRouter
Get-BgpPeer
Get-BgpRouteInformation
```

Além dessas, dos problemas que vimos até agora (especialmente em SDNExpress com base em implantações), a razão mais comum para compartimento de locatário não estão recebendo configurados GW VMs parece ser o fato de que a capacidade de GW em FabricConfig.psd1 é menor em comparação com o que as pessoas tentam atribuir às conexões de rede (S2S túneis) em TenantConfig.psd1. Isso pode ser verificado facilmente comparando saídas dos seguintes comandos:

```
PS > (Get-NetworkControllerGatewayPool -ConnectionUri $uri).properties.Capacity
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").properties.OutboundKiloBitsPerSecond
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").property
```

### <a name="hoster-validate-data-plane"></a>[Hoster] Validar o plano de dados
Depois que o controlador de rede tenha sido implantado, redes virtuais locatário e subredes foram criados e VMs foram conectadas para as sub-redes virtuais, testes de nível de malha adicionais podem ser executados pelo hoster para verificar a conectividade de locatário.

#### <a name="check-hnv-provider-logical-network-connectivity"></a>Verifique a conectividade de rede lógica de provedor HNV
Depois do primeiro convidado da VM em execução em um host do Hyper-V tenha sido conectado a uma rede virtual de locatário, o controlador de rede atribuirá dois endereços de IP de provedor HNV (endereços IP do PA) para o Host do Hyper-V. Esses IPs virão da rede lógica HNV provedor Pool de IP e ser gerenciados com o controlador de rede.  Para descobrir o que são esses dois endereços IP HNV do

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

Esses endereços de IP de provedor de HNV (PA IPs) são atribuídos a adaptadores Ethernet criado em um compartimento de rede TCPIP separado e ter um nome de adaptador de _VLANX_ onde X é a VLAN atribuída à rede lógica HNV provedor (transporte).

Conectividade entre dois hosts Hyper-V usando o provedor de HNV rede lógica pode ser feito por um ping com um compartimento adicional (-c Y) parâmetro onde Y é compartimento da rede TCPIP no qual os PAhostVNICs são criados. Este compartimento pode ser determinado, executando:

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
> Os adaptadores de vNIC PA Host não são usados no caminho de dados e então não têm um IP atribuído ao "adaptador vEthernet (PAhostVNic)".

Por exemplo, suponha que hosts Hyper-V 1 e 2 tenham endereços IP HNV provedor (PA):

|-Host hyper-V-|-Endereço IP PA 1|-Endereço IP PA 2|
|---             |---            |---             |
|Host 1 | 10.10.182.64 | 10.10.182.65 |
|Host 2 | 10.10.182.66 | 10.10.182.67 |

nós pode executar ping entre os dois usando o seguinte comando para verificar a conectividade de rede lógica HNV provedor.

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

*Correção* ping provedor HNV se não funcionar, verifique sua conectividade de rede física incluindo configuração VLAN. As placas de NIC físicas em cada host do Hyper-V devem estar no modo de tronco sem VLAN específico atribuído. O Host de gerenciamento vNIC deve ser isolado obtenham da rede lógico gerenciamento.

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
 
#### <a name="check-mtu-and-jumbo-frame-support-on-hnv-provider-logical-network"></a>Verificar o suporte MTU e Jumbo Frame na rede de lógica de provedor de HNV

Outro problema comum na rede lógico HNV provedor é que as portas de rede física e/ou a placa Ethernet não têm uma grande o suficiente MTU configurado para manipular a sobrecarga de encapsulamento VXLAN (ou NVGRE). 
>[!NOTE]
> Alguns cartões de Ethernet e drivers dão suporte a nova * EncapOverhead palavra-chave que será definida automaticamente pelo agente de Host do controlador de rede como um valor de 160. Esse valor, em seguida, será adicionado ao valor do * JumboPacket palavra-chave cuja soma é usada como o MTU anunciado.
> Por exemplo, * EncapOverhead = 160 e * JumboPacket = 1514 = > MTU = 1674B

```none
# Check whether or not your Ethernet card and driver support *EncapOverhead
PS C:\ > Test-EncapOverheadSettings

Verifying Physical Nic : <NIC> Ethernet Adapter #2
Physical Nic  <NIC> Ethernet Adapter #2 can support SDN traffic. Encapoverhead value set on the nic is  160
Verifying Physical Nic : <NIC> Ethernet Adapter
Physical Nic  <NIC> Ethernet Adapter can support SDN traffic. Encapoverhead value set on the nic is  160
```

Para testar se a rede lógica do provedor HNV compatível com a maior MTU tamanho ponta a ponta ou não, use o _teste LogicalNetworkSupportsJumboPacket_ cmdlet:
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

*Correção*
* Ajustar o tamanho MTU nas portas comutador físico para ser no mínimo 1674B (incluindo o cabeçalho de Ethernet 14B e trailer)
* Se seu cartão NIC não dá suporte a palavra-chave EncapOverhead, ajustar a palavra-chave JumboPacket para ser no mínimo 1674B


#### <a name="check-tenant-vm-nic-connectivity"></a>Verifique a conectividade de locatário VM NIC
Cada NIC VM atribuído a um convidado da VM tem um mapeamento de CA-PA entre o endereço do cliente particular (CA) e o espaço de endereço de provedor de HNV (PA). Esses mapeamentos são mantidos nas tabelas de servidor OVSDB em cada host do Hyper-V e podem ser encontrados, executando o seguinte cmdlet.

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
> Se os mapeamentos de CA-PA você espera que não são de saída para um determinado locatário VM, verifique os recursos de VM NIC e configuração de IP do controlador de rede usando o _Get-NetworkControllerNetworkInterface_ cmdlet. Além disso, verifique as conexões estabelecidas entre os nós NC Host agente e controlador de rede.

Com essas informações, um ping VM locatário agora pode ser iniciado pelo Hoster o controlador de rede que usa o _teste VirtualNetworkConnection_ cmdlet.

## <a name="specific-troubleshooting-scenarios"></a>Cenários de solução de problemas específicos

As seções a seguir fornecem orientação para cenários específicos de solução de problemas.

### <a name="no-network-connectivity-between-two-tenant-virtual-machines"></a>Sem conectividade de rede entre duas máquinas virtuais de locatário

1.  [Locatário] Certifique-se de que o Firewall do Windows em máquinas virtuais de locatário não está bloqueando o tráfego.  
2.  [Locatário] Verificar que endereços IP atribuídos à máquina virtual locatário executando _ipconfig_. 
3.  [Hoster] Executar **teste VirtualNetworkConnection** do host Hyper-V para validar a conectividade entre as máquinas virtuais de dois locatário em questão. 

>[!NOTE]
>O VSID refere-se para a ID de sub-rede Virtual. No caso de VXLAN, esse é o identificador de rede de VXLAN (VNI). Você pode encontrar esse valor executando o **Get-PACAMapping** cmdlet.

#### <a name="example"></a>Exemplo

    $password = ConvertTo-SecureString -String "password" -AsPlainText -Force
    $cred = New-Object pscredential -ArgumentList (".\administrator", $password) 

Crie CA-ping entre "Verde Web VM 1" com SenderCA IP de 192.168.1.4 no Host "sa18n30-2.sa18.nttest.microsoft.com" com Mgmt IP de 10.127.132.153 para ListenerCA IP de 192.168.1.5 anexados a sub-rede Virtual (VSID) 4114.

    Test-VirtualNetworkConnection -OperationId 27 -HostName sa18n30-2.sa18.nttest.microsoft.com -MgmtIp 10.127.132.153 -Creds $cred -VMName "Green Web VM 1" -VMNetworkAdapterName "Green Web VM 1" -SenderCAIP 192.168.1.4 -SenderVSID 4114 -ListenerCAIP 192.168.1.5 -ListenerVSID 4114

    Test-VirtualNetworkConnection at command pipeline position 1

Teste de ping CA espaço inicial a partir de sessão de rastreamento Ping para 192.168.1.5 foi bem-sucedida do endereço 192.168.1.4 Rtt = 0 ms


Informações de roteamento de autoridade de certificação:

    Local IP: 192.168.1.4
    Local VSID: 4114
    Remote IP: 192.168.1.5
    Remote VSID: 4114
    Distributed Router Local IP: 192.168.1.1
    Distributed Router Local MAC: 40-1D-D8-B7-1C-06
    Local CA MAC: 00-1D-D8-B7-1C-05
    Remote CA MAC: 00-1D-D8-B7-1C-07
    Next Hop CA MAC Address: 00-1D-D8-B7-1C-07

Informações de roteamento PA:

    Local PA IP: 10.10.182.66
    Remote PA IP: 10.10.182.65
 
 <snip> ...

4.  [Locatário] Verifique se há não há políticas de firewall distribuído especificadas na sub-rede virtual ou interfaces de rede VM que podem bloquear o tráfego.    

Consulte a API de REST do controlador de rede encontrados no ambiente de demonstração em sa18n30nc no domínio sa18.nttest.microsoft.com.

    $uri = "https://sa18n30nc.sa18.nttest.microsoft.com"
    Get-NetworkControllerAccessControlList -ConnectionUri $uri 

# <a name="look-at-ip-configuration-and-virtual-subnets-which-are-referencing-this-acl"></a>Examinar a configuração de IP e subredes virtuais que estiver fazendo referência a esse ACL

1. [Hoster] Executar ``Get-ProviderAddress`` em ambos os Hyper-V hosts hospeda as duas máquinas virtuais em questão do locatário em execute ``Test-LogicalNetworkConnection`` ou ``ping -c <compartment>`` do host Hyper-V para validar a conectividade na rede lógica HNV provedor
2.  [Hoster] Certifique-se de que as configurações de MTU estão corretas nos hosts Hyper-V e qualquer camada 2 alternar dispositivos entre os Hosts Hyper-V. Executar ``Test-EncapOverheadValue`` em todos os hosts Hyper-V em questão. Também verifique se todas as opções de camada 2 entre tem MTU definido como menos bytes 1674 para levar em conta máxima sobrecarga de 160 bytes.  
3.  [Hoster] Se os endereços IP de PA não estão presentes e/ou conectividade com a CA é quebrada, verifique se a política de rede foi recebida. Executar ``Get-PACAMapping`` para ver se as regras de encapsulamento e mapeamentos de CA-PA exigidos para criar redes virtuais de sobreposição são estabelecidos corretamente.  
4.  [Hoster] Verifique se o agente de Host do controlador de rede está conectado ao controlador de rede. Executar ``netstat -anp tcp |findstr 6640`` para ver se o   
5.  [Hoster] Verifique se o Host ID de HKLM / coincide com a ID da instância dos recursos do servidor que hospeda as máquinas virtuais de locatário.  
6. [Hoster] Verifique se a ID do perfil de porta coincide com a ID da instância VM Interfaces de rede das máquinas virtuais locatário.  

## <a name="logging-tracing-and-advanced-diagnostics"></a>Registro em log, rastreamento e diagnóstico avançado

As seções a seguir fornecem informações sobre o diagnóstico avançado, rastreamento e registro em log.

### <a name="network-controller-centralized-logging"></a>Registro em log controlador centralizado de rede 
 
O controlador de rede pode coletar logs de depurador e armazená-los em um local centralizado automaticamente. Coleção de log pode ser habilitada quando quando você implanta o controlador de rede para a primeira vez ou a qualquer momento posterior. Os logs são coletados do controlador de rede e gerenciados pelo controlador de rede de elementos de rede: hospedar máquinas, balanceadores de carga de software (SLB) e máquinas de gateway. 

Esses logs incluem logs de depuração para o cluster de controlador de rede, o aplicativo controlador de rede, logs de gateway, SLB, rede virtual e o firewall distribuído. Sempre que um novo host/SLB/gateway é adicionado ao controlador de rede, o registro em log é iniciado nesses computadores. Da mesma forma, quando um host/SLB/gateway é removido do controlador de rede, o registro em log é interrompido nesses computadores.

#### <a name="enable-logging"></a>Habilitar log

Registro em log é habilitado automaticamente quando você instala o cluster de controlador de rede usando o **instalar NetworkControllerCluster** cmdlet. Por padrão, os logs são coletados localmente em todos os nós de controlador de rede em *%systemdrive%\SDNDiagnostics*. É **altamente RECOMENDÁVEL** que você alterar esse local para ser um compartilhamento de arquivos remotos (não local). 

Os logs de cluster de controlador de rede são armazenados no *%programData%\Windows Fabric\log\Traces*. Você pode especificar um local centralizado para a coleção de log com o **DiagnosticLogLocation** parâmetro com a recomendação esse também é ser um compartilhamento de arquivos remoto. 

Se você quiser restringir o acesso a esse local, você pode fornecer as credenciais de acesso com o **LogLocationCredential** parâmetro. Se você fornecer as credenciais para acessar o local do log, você também deve fornecer o **CredentialEncryptionCertificate** parâmetro, que é usado para criptografar as credenciais armazenadas localmente em todos os nós de controlador de rede.  

Com as configurações padrão, é recomendável que você tenha pelo menos 75 GB de espaço livre no local central e 25 GB em nós locais (se não usando um local central) para um cluster de controlador de rede do nó de 3.

#### <a name="change-logging-settings"></a>Alterar as configurações de log

Você pode alterar as configurações de registro em log em qualquer momento usando o ``Set-NetworkControllerDiagnostic`` cmdlet. As seguintes configurações podem ser alteradas:

- **Local do log centralizado**.  Você pode alterar o local para armazenar todos os logs, com o ``DiagnosticLogLocation`` parâmetro.  
- **As credenciais para acessar o local do log**.  Você pode alterar as credenciais para acessar o local do log, com o ``LogLocationCredential`` parâmetro.  
- **Mover para o registro em log local**.  Se você tiver fornecido uma localização centralizada para armazenar os logs, você pode voltar para fazer logon localmente em todos os nós de controlador de rede com o ``UseLocalLogLocation`` parâmetro (não recomendado devido a requisitos de espaço em disco grande).  
- **Escopo de registro em log**.  Por padrão, todos os logs são coletados. Você pode alterar o escopo para coletar somente logs de cluster de controlador de rede.  
- **Nível de log**.  O nível de log padrão é informativo. Você pode alterá-lo para o erro, aviso ou detalhado.  
- **Duração de tempo de log**.  Os logs são armazenados em uma forma circular. Você terá 3 dias de registro em log dados por padrão, se você usar o registro em log local ou o log centralizado. Você pode alterar esse limite de tempo com **LogTimeLimitInDays** parâmetro.  
- **Duração do tamanho do log**.  Por padrão, você terá um máximo 75 GB de dados de registro em log se usando o log centralizado e 25 GB se usando o registro local. Você pode alterar esse limite com o **LogSizeLimitInMBs** parâmetro.

#### <a name="collecting-logs-and-traces"></a>Rastreamentos e coletar Logs

Implantações de VMM usam o log centralizado para o controlador de rede por padrão. O local do compartilhamento de arquivo para esses logs é especificado ao implantar o modelo de serviço do controlador de rede.

Se um local de arquivo não foi especificado, local log será usado em cada nó de controlador de rede com os logs salvos em C:\Windows\tracing\SDNDiagnostics. Esses logs são salvos usando a hierarquia a seguir:

- Despejos de memória
- NCApplicationCrashDumps
- NCApplicationLogs
- PerfCounters
- SDNDiagnostics
- Rastreamentos

O controlador de rede usa (Azure) Service Fabric. Service Fabric logs podem ser necessárias quando determinados problemas. Esses logs podem ser encontrados em cada nó de controlador de rede em C:\ProgramData\Microsoft\Service malha.

Se um usuário executou o _depurar NetworkController_ cmdlet, logs adicionais estarão disponíveis em cada host do Hyper-V que foi especificado com um recurso de servidor no controlador de rede. Esses logs (rastreamentos e se habilitado) são mantidos em C:\NCDiagnostics

### <a name="slb-diagnostics"></a>Diagnóstico SLB

#### <a name="slbm-fabric-errors-hosting-service-provider-actions"></a>Erros de malha SLBM (ações de provedor de serviço de hospedagem)

1.  Verifique se o Gerenciador de Balanceador de carga de Software (SLBM) está funcionando e que as camadas de coordenação podem falar uns aos outros: SLBM -> SLB multiplexador e SLBM -> SLB agentes do Host. Executar [DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1) de qualquer nó com acesso à rede ponto de extremidade de REST de controlador.  
2.  Validar o *SDNSLBMPerfCounters* em PerfMon em um nó de controlador de rede VMs (preferencialmente o controlador de rede nó primário - Get-NetworkControllerReplica):
    1.  Mecanismo de carregamento balanceador (LB) está conectado à SLBM? (*SLBM LBEngine configurações Total* > 0)  
    2.  SLBM pelo menos conhece seus próprios pontos de extremidade? (*Total de pontos de extremidade VIP* > = 2)  
    3.  Os hosts Hyper-V (DIP) estão conectados à SLBM? (*HP clientes conectados* = = num servidores)   
    4.  SLBM está conectado à Muxes? (*Muxes conectado* == *Muxes saudável em SLBM* == *Muxes relatório saudável* = # SLB Muxes VMs).  
3.  Certifique-se de que o roteador BGP configurado com êxito é estranhas com SLB MUX  
    1.  Se estiver usando RRAS com acesso remoto (isto é, máquina virtual BGP):  
        1.  Get-BgpPeer deve mostrar conectado  
        2.  Get-BgpRouteInformation deve mostrar pelo menos uma rota para o SLBM self VIP  
    2.  Se usar físico superior de Rack (ToR) alternar como correspondente BGP, consulte a documentação  
        1.  Por exemplo: # Mostrar bgp instância  
4.  Validar o *SlbMuxPerfCounters* e *SLBMUX* contadores em PerfMon na VM multiplexador SLB
5.  Verificar o estado da configuração e intervalos de VIP no recurso de Gerenciador de Balanceador de carga de Software  
    1.  Get-NetworkControllerLoadBalancerConfiguration - ConnectionUri < https://<FQDN or IP>| json ConvertTo-profundidade 8 (Verifique VIP intervalos em Pools de IP e certifique-se de SLBM self-VIP (*LoadBalanacerManagerIPAddress*) e qualquer VIPs locatário voltados estão contidos esses intervalos)  
        1. Get-NetworkControllerIpPool - NetworkId "< pública/privada VIP lógico rede Resource ID >" - SubnetId "< pública/privada VIP lógico sub-rede Resource ID >" - ResourceId "<IP Pool Resource Id>" - ConnectionUri $uri | json convertto-profundidade 8 
    2.  Depurar-NetworkControllerConfigurationState-  

Se qualquer uma das verificações acima falhar, o estado SLB locatário também estará no modo de falha.  

**Correção**   
Com base nos seguintes informações de diagnóstico apresentadas, corrigi o seguinte:  
* Certifique-se de que estão conectados SLB Multiplexers  
  * Corrigir problemas de certificado  
  * Corrigir problemas de conectividade de rede  
* Certifique-se de informações de correspondência BGP for configuradas com êxito  
* Certifique-se a ID do Host no registro corresponde a ID da instância de servidor no recurso de servidor (consultar o Apêndice para *HostNotConnected* código de erro)  
* Coletar registros  

#### <a name="slbm-tenant-errors-hosting-service-provider--and-tenant-actions"></a>Erros de locatário SLBM (hospedagem provedor e locatário ações de serviço)

1.  [Hoster] Verificar *depurar NetworkControllerConfigurationState* para ver se todos os recursos LoadBalancer em um estado de erro. Tente atenuar seguindo os itens de ação tabela no apêndice.   
    1.  Verifique se um ponto de extremidade VIP está presente e rotas de publicidade  
    2.  Verifique a descoberta de quantos pontos de extremidade DIP para o ponto de extremidade VIP  
2.  [Locatário] Validar recursos de Balanceador de carga são especificados corretamente  
    1.  Validar DIP pontos de extremidade que são registrados no SLBM são hospedando máquinas de virtuais de locatário que correspondem às configurações de IP do pool de endereço LoadBalancer Back-end  
3.  [Hoster] Se não, pontos de extremidade DIP são descobertos ou conectados:   
    1.  Verificar *NetworkControllerConfigurationState de depuração*  
        1.  Validar esse NC e agente de Host SLB com êxito é conectado à rede coordenador de evento de controlador usando ``netstat -anp tcp |findstr 6640)``  
    2.  Verificar *Identificação_do_host* em *nchostagent* regkey de serviço (referência *HostNotConnected* código de erro no apêndice) corresponde a Id da instância do recurso do servidor correspondente (``Get-NCServer |convertto-json -depth 8``)  
    3.  Verifique a id do perfil de porta para porta de máquina virtual corresponde a Id da instância do recurso NIC de máquina virtual correspondente   
4.  [Provedor de hospedagem] Coletar registros   

#### <a name="slb-mux-tracing"></a>Rastreamento de multiplexador SLB

Informações de Muxes de Balanceador de carga do Software também podem ser determinadas por meio de Visualizador de eventos. 
1. Clique em "Mostrar e depurar Logs analíticos" no menu Exibir do Visualizador de eventos
2. Navegue até "Logs de aplicativos e serviços" > Microsoft > Windows > SlbMuxDriver > rastrear no Visualizador de eventos 
3. Clique com o botão direito nele e selecione "Habilitar Log"

>[!NOTE]
>É recomendável que você só tem esse registro em log habilitado por um curto período enquanto você está tentando reproduzir um problema

### <a name="vfp-and-vswitch-tracing"></a>VFP vSwitch rastreamento e

De qualquer Hyper-V host que está hospedando um convidado da VM conectado a uma rede virtual de locatário, você pode coletadas um rastreamento VFP para determinar onde problemas podem estar.

```
netsh trace start provider=Microsoft-Windows-Hyper-V-VfpExt overwrite=yes tracefile=vfp.etl report=disable provider=Microsoft-Windows-Hyper-V-VmSwitch 
netsh trace stop
netsh trace convert .\vfp.etl ov=yes 
```
