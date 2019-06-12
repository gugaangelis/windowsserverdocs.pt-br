---
title: Solucionar problemas de pilha de rede definida do Windows Server Software
description: Este guia do Windows Server examina os erros comuns de Software Defined Networking (SDN) e cenários de falha e descreve um fluxo de trabalho de solução de problemas que aproveita as ferramentas de diagnóstico disponíveis.
manager: ravirao
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 9be83ed2-9e62-49e8-88e7-f52d3449aac5
ms.author: pashort
author: JMesser81
ms.date: 08/14/2018
ms.openlocfilehash: eeb0c335e4afd3c6835a04421a15073aeab6cdc6
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446243"
---
# <a name="troubleshoot-the-windows-server-software-defined-networking-stack"></a>Solucionar problemas de pilha de rede definida do Windows Server Software

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este guia examina os erros comuns de Software Defined Networking (SDN) e cenários de falha e descreve um fluxo de trabalho de solução de problemas que aproveita as ferramentas de diagnóstico disponíveis.  

Para obter mais informações sobre a rede definida da Microsoft Software, consulte [rede definida pelo Software](../../sdn/Software-Defined-Networking--SDN-.md).  

## <a name="error-types"></a>Tipos de erros  
A lista a seguir representa a classe dos problemas mais comuns com a virtualização de rede do Hyper-V (HNVv1) no Windows Server 2012 R2 de implantações de produção no mercado e coincide de muitas maneiras com os mesmos tipos de problemas vistos no Windows Server 2016 HNVv2 com a nova pilha de rede definida pelo Software (SDN).  

A maioria dos erros podem ser classificados em um pequeno conjunto de classes:   
* **Configuração inválida ou sem suporte**  
   Um usuário chama a API NorthBound incorretamente ou com política inválida.   

* **Erro no aplicativo de política**  
     Política do controlador de rede não foi entregue a um Host Hyper-V, significativamente atrasadas e / ou não atualizado em todos os hosts do Hyper-V (por exemplo, após uma migração ao vivo).  
* **Configuração descompasso ou bug do software**  
  Problemas de caminho de dados resultando em pacotes ignorados.  

* **Erro externo relacionado a hardware NIC / drivers ou a base de malha de rede**  
  Com comportamento inadequado descarregamentos de tarefa (por exemplo, a VMQ) ou a malha de rede de base configurada incorretamente (por exemplo, a MTU)   

  Este guia de solução de problemas examina cada uma dessas categorias de erro e recomenda as práticas recomendadas e ferramentas de diagnóstico disponíveis para identificar e corrigir o erro.  

## <a name="diagnostic-tools"></a>Ferramentas de diagnóstico  

Antes de discutir os fluxos de trabalho de solução de problemas para cada um desses tipos de erro, vamos examinar as ferramentas de diagnóstico disponíveis.   

Para usar as ferramentas de diagnóstico do controlador de rede (caminho de controle), você deve primeiro instalar o recurso de RSAT NetworkController e importar o ``NetworkControllerDiagnostics`` módulo:  

```  
Add-WindowsFeature RSAT-NetworkController -IncludeManagementTools  
Import-Module NetworkControllerDiagnostics  
```  

Para usar as ferramentas de diagnóstico de diagnóstico da HNV (caminho de dados), você deve importar o ``HNVDiagnostics`` módulo:

```  
# Assumes RSAT-NetworkController feature has already been installed
Import-Module hnvdiagnostics   
```  

### <a name="network-controller-diagnostics"></a>Diagnósticos de rede do controlador  
Esses cmdlets estão documentados no TechNet na [tópico de Cmdlet de diagnóstico de controlador de rede](https://docs.microsoft.com/powershell/module/networkcontrollerdiagnostics/). Eles ajudam a identificar problemas com a consistência de diretiva de rede no caminho de controle-entre nós de controlador de rede e entre o controlador de rede e os agentes de Host NC executados nos hosts Hyper-V.

 O _depuração ServiceFabricNodeStatus_ e _Get-NetworkControllerReplica_ cmdlets devem ser executados de uma das máquinas virtuais do nó do controlador de rede. Todos os outros cmdlets de diagnóstico do NC podem ser executados a partir de qualquer host que tenha conectividade com o controlador de rede e estiver em um grupo de segurança de gerenciamento do controlador de rede (Kerberos) ou tem acesso ao certificado X.509 para gerenciar o controlador de rede. 

### <a name="hyper-v-host-diagnostics"></a>Diagnóstico de host Hyper-V  
Esses cmdlets estão documentados no TechNet na [tópico de Cmdlet de diagnóstico de virtualização de rede do Hyper-V (HNV)](https://docs.microsoft.com/powershell/module/hnvdiagnostics/). Ele ajuda a identificar problemas no caminho de dados entre máquinas virtuais de locatário (Leste/Oeste) e o tráfego de entrada por meio de um VIP de SLB (Norte/Sul). 

O _depuração VirtualMachineQueueOperation_, _Get-CustomerRoute_, _Get-PACAMapping_, _Get-ProviderAddress_, _Get-VMNetworkAdapterPortId_, _Get-VMSwitchExternalPortId_, e _EncapOverheadSettings teste_ são todos os testes locais que podem ser executados a partir de qualquer host Hyper-V. Outros cmdlets invocar testes de caminho de dados por meio do controlador de rede e, portanto, precisam acessar o controlador de rede como descried acima.

### <a name="github"></a>GitHub
O [repositório do GitHub Microsoft/SDN](https://github.com/microsoft/sdn) tem um número de fluxos de trabalho que se baseiam nesses cmdlets nativos e scripts de exemplo. Em particular, os scripts de diagnóstico podem ser encontradas na [diagnóstico](https://github.com/Microsoft/sdn/diagnostics) pasta. Ajude-na contribuir com esses scripts, enviando solicitações Pull.

## <a name="troubleshooting-workflows-and-guides"></a>Fluxos de trabalho e guias de solução de problemas  

### <a name="hoster-validate-system-health"></a>[Hoster] Validar a integridade do sistema
Há um recurso inserido denominado _estado de configuração_ em vários dos recursos de controlador de rede. Estado de configuração fornece informações sobre a integridade de sistema, incluindo a consistência entre a configuração do controlador de rede e o estado real (em execução) nos hosts do Hyper-V. 

Para verificar o estado de configuração, execute o seguinte de qualquer host Hyper-V com conectividade para o controlador de rede.

>[!NOTE] 
>O valor para o *NetworkController* parâmetro deve ser o endereço IP ou FQDN, com base no nome da entidade dos x. 509 > certificado criado para o controlador de rede.
>
>O *credencial* parâmetro só deve ser especificado se o controlador de rede está usando a autenticação Kerberos (típica em implantações do VMM). A credencial deve ser para um usuário que está no grupo de segurança de gerenciamento do controlador de rede.

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
> Há um bug no sistema onde os recursos de Interface de rede para a NIC da VM SLB Mux trânsito estão em um estado de falha com erro "Virtual Switch – não conectado ao controlador de Host". Esse erro pode ser ignorado se a configuração de IP no recurso da NIC da VM está definida para um endereço IP do Pool de IP da rede lógica de trânsito. Há um segundo bug no sistema onde os recursos de Interface de rede para as NICs de VM do Gateway da HNV provedor estão em um estado de falha com erro "Virtual Switch – PortBlocked". Esse erro pode também ser ignorado se a configuração de IP no recurso da NIC da VM é definida como nulo (por design).


A tabela a seguir mostra a lista de códigos de erro, mensagens e ações de acompanhamento necessárias com base no estado da configuração observado.


| **Código**| **Mensagem**| **Ação**|  
|--------|-----------|----------|  
| Desconhecido| Erro desconhecido| |  
| HostUnreachable                       | O computador host não está acessível | Verifique a conectividade de rede de gerenciamento entre o Host e o controlador de rede |  
| PAIpAddressExhausted                  | Os endereços Ip do PA esgotada | Aumentar o tamanho do Pool de IP da sub-rede lógica do provedor de HNV |  
| PAMacAddressExhausted                 | Os endereços Mac de PA esgotada | Aumentar o intervalo de Pool de Mac |  
| PAAddressConfigurationFailure         | Falha ao conectar a endereços PA para o host | Verifique a conectividade de rede de gerenciamento entre o Host e o controlador de rede. |  
| CertificateNotTrusted                 | Certificado não é confiável  |Corrija os certificados usados para comunicação com o host. |  
| CertificateNotAuthorized              | Certificado não autorizado | Corrija os certificados usados para comunicação com o host. |  
| PolicyConfigurationFailureOnVfp       | Falha na configuração de políticas VFP | Essa é uma falha de tempo de execução.  Não há alternativas de trabalho definido. Colete logs. |  
| PolicyConfigurationFailure            | Falha no envio por push políticas para os hosts, devido a falhas de comunicação ou outros erros no NetworkController.| Não há ações definitiva.  Isso é devido uma falha no estado de meta os módulos do controlador de rede de processamento. Colete logs. |  
| HostNotConnectedToController          | O Host ainda não está conectado ao controlador de rede | Perfil de porta não é aplicada no host ou o host não é acessível a partir do controlador de rede. Validar que a chave do registro HostID corresponde a ID da instância do recurso servidor |  
| MultipleVfpEnabledSwitches            | Há vários VFp ativado comutadores no host  | Exclua uma das opções, já que o agente de Host do controlador de rede dá suporte apenas a um vSwitch com a extensão VFP habilitada |  
| PolicyConfigurationFailure            | Falha ao enviar por push as políticas de rede virtual para uma VmNic devido a erros de certificado ou erros de conectividade  | Verifique se os certificados apropriados terem sido implantados (nome de assunto do certificado deve corresponder ao FQDN do host). Também verifique se a conectividade de host com o controlador de rede |  
| PolicyConfigurationFailure            | Falha ao enviar por push o vSwitch políticas para uma VmNic devido a erros de certificado ou erros de conectividade  | Verifique se os certificados apropriados terem sido implantados (nome de assunto do certificado deve corresponder ao FQDN do host). Também verifique se a conectividade de host com o controlador de rede|
| PolicyConfigurationFailure            | Falha ao enviar por push políticas de Firewall para uma VmNic devido a erros de certificado ou erros de conectividade | Verifique se os certificados apropriados terem sido implantados (nome de assunto do certificado deve corresponder ao FQDN do host). Também verifique se a conectividade de host com o controlador de rede|
| DistributedRouterConfigurationFailure | Falha ao configurar as configurações do roteador distribuído na vNic do host                          | Erro de pilha de TCP/IP. Pode exigir a limpeza dos vNICs PA e Host de recuperação de desastres no servidor em que esse erro foi relatado |
| DhcpAddressAllocationFailure          | Falha na alocação de endereço DHCP para uma VMNic                                                    | Verifique se o atributo de endereço IP estático está configurado no recurso NIC |  
| CertificateNotTrusted<br>CertificateNotAuthorized | Falha ao conectar-se ao Mux devido a erros de rede ou um certificado | Verifique o código numérico fornecido no código de mensagem de erro: isso corresponde ao código de erro de winsock. Erros de certificado são granulares (por exemplo, o certificado não pode ser verificado, cert não autorizado, etc.) |  
| HostUnreachable                       | MUX não está íntegro (o caso comum é BGPRouter desconectado) | Par de BGP no RRAS (máquina virtual BGP) ou comutador Top-of-Rack (ToR) é inacessível ou não emparelhamento com êxito. Verificar as configurações de protocolo BGP no recurso multiplexador para balanceador de carga do Software e o par de BGP (ToR ou RRAS máquina virtual) |  
| HostNotConnectedToController          | Agente de host do SLB não está conectado.  | Verifique se o serviço SLB Host Agent está em execução; Consulte os logs do agente de host SLB (executando automaticamente) para motivos por que, no caso de SLBM (NC) rejeitou o certificado apresentado pelo agente de host executando o estado mostrará informações sutis  |  
| PortBlocked                           | A porta do VFP está bloqueada, devido à falta de rede virtual / políticas de ACL | Verifique se há quaisquer outros erros, que podem fazer com que as políticas de não ser configurado. |  
| Sobrecarregado                            | LoadBalancer MUX está sobrecarregado  | Problema de desempenho MUX |  
| RoutePublicationFailure               | LoadBalancer MUX não está conectado a um roteador BGP | Verifique se o MUX tem conectividade com os roteadores BGP e esse emparelhamento via protocolo BGP está configurado corretamente |  
| VirtualServerUnreachable              | LoadBalancer MUX não está conectado ao Gerenciador de SLB | Verifique a conectividade entre SLBM e MUX |  
| QosConfigurationFailure               | Falha ao configurar as políticas de QOS | Se a largura de banda suficiente está disponível para todas as VMs se for usada a reserva de QOS |


#### <a name="check-network-connectivity-between-the-network-controller-and-hyper-v-host-nc-host-agent-service"></a>Verifique a conectividade de rede entre o controlador de rede e o Host do Hyper-V (serviço de agente de Host NC)
Execute o *netstat* comando a seguir para validar que há três conexões estabelecido entre o agente de Host NC e os nós de controlador de rede e um soquete de ESCUTA no Host Hyper-V
- ESCUTANDO na porta TCP:6640 no Host do Hyper-V (serviço de agente de Host NC)
- IP na porta 6640 NC nó IP nas portas efêmeras (> 32000) do host de duas conexões estabelecidas do Hyper-V
- Uma conexão estabelecida de IP do host do Hyper-V na porta efêmera para o IP de REST do controlador de rede na porta 6640

>[!NOTE]
>Pode haver apenas duas conexões estabelecidas em um host Hyper-V se não houver nenhuma máquina virtual do locatário implantada nesse host específico.

```none
netstat -anp tcp |findstr 6640

# Successful output
  TCP    0.0.0.0:6640           0.0.0.0:0              LISTENING
  TCP    10.127.132.153:6640    10.127.132.213:50095   ESTABLISHED
  TCP    10.127.132.153:6640    10.127.132.214:62514   ESTABLISHED
  TCP    10.127.132.153:50023   10.127.132.211:6640    ESTABLISHED
```
#### <a name="check-host-agent-services"></a>Verificar serviços do agente de Host
O controlador de rede se comunica com dois serviços de agente de host nos hosts do Hyper-V: Agente de Host do SLB e o agente de Host NC. É possível que um ou ambos os serviços não está em execução. Verifique seu estado e reinicie se elas não estão em execução.

```none
Get-Service SlbHostAgent
Get-Service NcHostAgent

# (Re)start requires -Force flag
Start-Service NcHostAgent -Force
Start-Service SlbHostAgent -Force
```

#### <a name="check-health-of-network-controller"></a>Verifique a integridade do controlador de rede
Se não houver não três conexões estabelecido, ou se o controlador de rede é exibido sem resposta, verifique se todos os nós e os módulos de serviço estão em execução usando os cmdlets a seguir. 

```none
# Prints a DIFF state (status is automatically updated if state is changed) of a particular service module replica 
Debug-ServiceFabricNodeStatus [-ServiceTypeName] <Service Module>
```
Os módulos de serviço do controlador de rede são:
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

Verifique se está o ReplicaStatus **pronto** e é HealthState **Okey**.

Em um de produção a implantação é de um controlador de rede com vários nós, você também pode verificar qual nó de cada serviço é primário e o status da réplica individual.

```none  
Get-NetworkControllerReplica

# Sample Output for the API service module 
Replicas for service: ApiService

ReplicaRole   : Primary
NodeName      : SA18N30NC3.sa18.nttest.microsoft.com
ReplicaStatus : Ready

```
Verifique se o Status da réplica está pronta para cada serviço.

#### <a name="check-for-corresponding-hostids-and-certificates-between-network-controller-and-each-hyper-v-host"></a>Verifique se há IDs de host correspondentes e certificados entre o controlador de rede e cada Host Hyper-V 
Em um Host Hyper-V, execute os seguintes comandos para verificar que o HostID corresponde à Id de instância de um recurso de servidor no controlador de rede

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

*Correção* se usando SDNExpress scripts ou implantação manual, atualize a chave HostId no registro para corresponder à Id de instância do recurso de servidor. Reinicie o agente de Host do controlador de rede no host Hyper-V (servidor físico) se usando o VMM, exclua o servidor Hyper-V do VMM e remover a chave do registro HostId. Em seguida, adicione novamente o servidor por meio do VMM.


Verifique se as impressões digitais dos certificados x. 509 usados pelo host do Hyper-V (o nome do host será um nome de assunto do certificado) para a comunicação entre o Host do Hyper-V (serviço de agente de Host NC) e os nós de controlador de rede (SouthBound) são os mesmos. Também verifique se o certificado REST do controlador de rede tem o nome da entidade de *CN =<FQDN or IP>* .

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

Você também pode verificar os seguintes parâmetros de cada certificado para garantir que o nome da entidade é o que é esperado (nome de host ou FQDN de REST do NC ou IP), o certificado ainda não expirou, e que todas as autoridades de certificação na cadeia de certificados estão incluídas na raiz confiável autoridade.

- Nome do assunto  
- Data de validade  
- Autoridade raiz confiável  

*Correção* se vários certificados têm o mesmo nome de assunto no host Hyper-V, o agente de Host do controlador de rede escolherá aleatoriamente um para apresentar ao controlador de rede. Isso pode não corresponder a impressão digital do recurso servidor conhecido para o controlador de rede. Nesse caso, exclua um dos certificados com o mesmo nome de assunto no host Hyper-V e, em seguida, reinicie o serviço de agente de Host do controlador de rede. Se uma conexão não ainda pode ser feita, exclua o outro certificado com o mesmo nome de assunto no Host Hyper-V e exclua o recurso de servidor correspondente no VMM. Em seguida, crie novamente o recurso de servidor no VMM que vai gerar um novo certificado x. 509 e instalá-lo no host Hyper-V.


#### <a name="check-the-slb-configuration-state"></a>Verificar o estado de configuração de SLB
O estado de configuração do SLB pode ser determinado como parte da saída para o cmdlet Debug-NetworkController. Esse cmdlet também produzirá o conjunto atual de recursos do controlador de rede em arquivos JSON, todas as configurações de IP de cada host do Hyper-V (servidor) e a política de rede local das tabelas de banco de dados do agente de Host. 

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

As informações de estado de configuração do SLB podem ser encontradas na _slbstateResults.Json diagnóstico_ arquivo nesse diretório.

Esse arquivo JSON pode ser dividido nas seguintes seções:
 * Malha
   * SlbmVips - esta seção lista o endereço IP do endereço do Gerenciador de SLB VIP que é usado pelo controlador de rede para a configuração de coodinate e a integridade entre os agentes de Host de SLB e Muxes SLB.
   * MuxState - esta seção listará um valor para cada SLB Mux implantada dando o estado de mux
   * Configuração do roteador - esta seção listará Upstream do roteador (par de BGP) número de sistema autônomo (ASN), endereço de IP de trânsito e ID. Ele também listará o SLB Muxes ASN e IP de trânsito.
   * Conectado a informações de Host – esta seção lista o IP de gerenciamento solucionará todos os hosts do Hyper-V disponíveis para executar cargas de trabalho com balanceamento de carga.
   * Intervalos de VIP - esta seção lista os intervalos de pool de IP VIP públicos e privados. O VIP SLBM será incluído como um IP alocado de um desses intervalos. 
   * Rotas de MUX – esta seção listará um valor para cada SLB Mux implantado que contém todos os anúncios de rota para esse determinado mux.
 * Locatário
   * VipConsolidatedState - esta seção listará o estado de conectividade para cada VIP de locatário, incluindo o prefixo de rota anunciados, o Host Hyper-V e pontos de extremidade DIP.

> [!NOTE]
> SLB estado podem ser determinado diretamente usando o [DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1) script disponível na [repositório GitHub Microsoft SDN](https://github.com/microsoft/sdn). 

#### <a name="gateway-validation"></a>Validação de gateway

**Do controlador de rede:**
```
Get-NetworkControllerLogicalNetwork
Get-NetworkControllerPublicIPAddress
Get-NetworkControllerGatewayPool
Get-NetworkControllerGateway
Get-NetworkControllerVirtualGateway
Get-NetworkControllerNetworkInterface
```

**Na VM do Gateway:**
```
Ipconfig /allcompartments /all
Get-NetRoute -IncludeAllCompartments -AddressFamily
Get-NetBgpRouter
Get-NetBgpRouter | Get-BgpPeer
Get-NetBgpRouter | Get-BgpRouteInformation
```

**Da parte superior do comutador de Rack (ToR):**

`sh ip bgp summary (for 3rd party BGP Routers)`

**Roteador BGP do Windows**
```
Get-BgpRouter
Get-BgpPeer
Get-BgpRouteInformation
```

Além dessas, dos problemas que vimos até agora (especialmente em implantações de SDNExpress com base), o motivo mais comum para o compartimento de locatário não obtendo configurada nas VMs de GW parecem ser o fato de que a capacidade de GW em FabricConfig.psd1 for menor em comparação com o que as pessoas tentar atribuir a conexões de rede (túneis S2S) em TenantConfig.psd1. Isso pode ser verificado com facilidade, comparando as saídas dos comandos a seguir:

```
PS > (Get-NetworkControllerGatewayPool -ConnectionUri $uri).properties.Capacity
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").properties.OutboundKiloBitsPerSecond
PS > (Get-NetworkControllerVirtualgatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId "TenantName").property
```

### <a name="hoster-validate-data-plane"></a>[Hoster] Validar o plano de dados
Depois que o controlador de rede tiver sido implantado, sub-redes e redes virtuais de locatário foi criadas e as VMs foram anexadas a sub-redes virtuais, os testes de nível de malha adicionais podem ser executadas pelo hoster para verificar a conectividade de locatário.

#### <a name="check-hnv-provider-logical-network-connectivity"></a>Verificar a conectividade de rede lógica do provedor HNV
Após o primeiro convidado VM em execução em um host Hyper-V foi conectada a uma rede virtual do locatário, o controlador de rede atribuirá dois endereços de IP de provedor de HNV (endereços IP do PA) para o Host do Hyper-V. Esses IPs virão do Pool de IP da rede lógica do provedor de HNV e serão gerenciadas pelo controlador de rede.  Para descobrir quais são esses dois endereços IP da HNV do

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

Esses endereços de IP do provedor de HNV (PA IPs) são atribuídos a adaptadores Ethernet criado em um compartimento de rede TCP/IP separado e ter um nome de adaptador de _VLANX_ onde X é a VLAN atribuída à rede lógica do provedor de HNV (transporte).

Conectividade entre dois hosts Hyper-V usando o provedor de HNV a rede lógica pode ser feita por um ping com um compartimento adicional (-c Y) onde Y é o compartimento de rede TCP/IP em que o PAhostVNICs são criados de parâmetro. Esse compartimento pode ser determinado executando:

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
> Os adaptadores de vNIC do Host de PA não são usados no caminho de dados e então não tem um IP atribuído ao "adaptador vEthernet (PAhostVNic)".

Por exemplo, suponha que os hosts do Hyper-V 1 e 2 tenham endereços de IP do provedor de HNV (PA) de:

|Host do hyper-V-|-Endereço IP do PA 1|-Endereço IP do PA 2|
|---             |---            |---             |
|Host 1 | 10.10.182.64 | 10.10.182.65 |
|Host 2 | 10.10.182.66 | 10.10.182.67 |

podemos pode executar ping entre os dois usando o seguinte comando para verificar a conectividade de rede lógica do provedor de HNV.

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

*Correção* ping do provedor de HNV se não funcionar, verifique a conectividade de rede física, incluindo a configuração de VLAN. As NICs físicas em cada host Hyper-V devem estar no modo de tronco sem VLAN específico atribuído. A vNIC do Host de gerenciamento deve ser isolada a VLAN da rede lógica de gerenciamento.

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

#### <a name="check-mtu-and-jumbo-frame-support-on-hnv-provider-logical-network"></a>Verifique o suporte MTU e quadros Jumbo na rede lógica do provedor de HNV

Outro problema comum na rede lógica do provedor de HNV é que as portas de rede física e/ou uma placa Ethernet não tenha uma MTU grande o suficiente configurada para lidar com a sobrecarga do encapsulamento VXLAN (ou NVGRE). 
>[!NOTE]
> Alguns cartões de Ethernet e drivers ofereçam suporte a nova * EncapOverhead palavra-chave que será definida automaticamente pelo agente de Host de controlador de rede para um valor de 160. Esse valor, em seguida, será adicionado ao valor da * palavra-chave de JumboPacket cuja soma é usada como o MTU anunciado.
> Por exemplo, * EncapOverhead = 160 e * JumboPacket = 1514 = > MTU = 1674B

```none
# Check whether or not your Ethernet card and driver support *EncapOverhead
PS C:\ > Test-EncapOverheadSettings

Verifying Physical Nic : <NIC> Ethernet Adapter #2
Physical Nic  <NIC> Ethernet Adapter #2 can support SDN traffic. Encapoverhead value set on the nic is  160
Verifying Physical Nic : <NIC> Ethernet Adapter
Physical Nic  <NIC> Ethernet Adapter can support SDN traffic. Encapoverhead value set on the nic is  160
```

Para testar se a rede lógica do provedor de HNV dá suporte a maior MTU tamanho-ponta, use o _LogicalNetworkSupportsJumboPacket teste_ cmdlet:
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
* Ajustar o tamanho MTU nas portas de comutador físico ser pelo menos 1674B (incluindo o cabeçalho de Ethernet 14B e rodapé)
* Se seu cartão NIC não oferece suporte a palavra-chave EncapOverhead, ajuste a palavra-chave de JumboPacket para ser de pelo menos 1674B


#### <a name="check-tenant-vm-nic-connectivity"></a>Verifique a conectividade de NIC da VM de locatário
Cada NIC da VM atribuída a uma VM convidada tem um mapeamento de CA-PA entre o endereço de cliente particular (CA) e o espaço de endereço de provedor de HNV (PA). Esses mapeamentos são mantidos nas tabelas de servidor OVSDB em cada host Hyper-V e podem ser encontrados ao executar o cmdlet a seguir.

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
> Se os mapeamentos CA-PA você espera que não são produzidos para uma determinada VM de locatário, verifique se os recursos de NIC da VM e configuração de IP no controlador de rede usando o _Get-NetworkControllerNetworkInterface_ cmdlet. Além disso, verifique as conexões estabelecidas entre os nós de agente de Host NC e o controlador de rede.

Com essas informações, um ping VM de locatário agora pode ser iniciado pelo Hoster do controlador de rede usando o _VirtualNetworkConnection teste_ cmdlet.

## <a name="specific-troubleshooting-scenarios"></a>Cenários de solução de problemas específicos

As seções a seguir fornecem diretrizes para solução de problemas de cenários específicos.

### <a name="no-network-connectivity-between-two-tenant-virtual-machines"></a>Não há conectividade de rede entre duas máquinas virtuais de locatário

1.  [Locatário] Verifique se o que Firewall do Windows em máquinas virtuais de locatário não está bloqueando o tráfego.  
2.  [Locatário] Verificar que endereços IP foram atribuídos à máquina virtual locatário executando _ipconfig_. 
3.  [Hoster] Execute **VirtualNetworkConnection teste** do host do Hyper-V para validar a conectividade entre as máquinas virtuais de dois locatário em questão. 

>[!NOTE]
>A VSID refere-se para a ID de subrede Virtual. No caso de VXLAN, isso é o identificador de rede VXLAN (VNI). Você pode encontrar esse valor, executando o **Get-PACAMapping** cmdlet.

#### <a name="example"></a>Exemplo

    $password = ConvertTo-SecureString -String "password" -AsPlainText -Force
    $cred = New-Object pscredential -ArgumentList (".\administrator", $password) 

Crie "sa18n30-2.sa18.nttest.microsoft.com" com o endereço IP Mgmt 10.127.132.153 IP ListenerCA de 192.168.1.5 anexados a sub-rede Virtual (VSID) 4114 de ping de autoridade de certificação entre "Verde Web VM 1" com o endereço IP SenderCA 192.168.1.4 no Host.

    Test-VirtualNetworkConnection -OperationId 27 -HostName sa18n30-2.sa18.nttest.microsoft.com -MgmtIp 10.127.132.153 -Creds $cred -VMName "Green Web VM 1" -VMNetworkAdapterName "Green Web VM 1" -SenderCAIP 192.168.1.4 -SenderVSID 4114 -ListenerCAIP 192.168.1.5 -ListenerVSID 4114

    Test-VirtualNetworkConnection at command pipeline position 1

Iniciando teste de ping de espaço do CA iniciar sessão de rastreamento de Ping para 192.168.1.5 bem-sucedida do endereço 192.168.1.4 Rtt = 0 ms


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

Informações de roteamento de PA:

    Local PA IP: 10.10.182.66
    Remote PA IP: 10.10.182.65

 <snip> ...

4. [Locatário] Verifique que não há nenhuma política de firewall distribuído especificada nas interfaces de rede VM que bloqueiam o tráfego ou sub-rede virtual.    

Consulte a API de REST do controlador de rede encontrados no ambiente de demonstração no sa18n30nc no domínio sa18.nttest.microsoft.com.

    $uri = "https://sa18n30nc.sa18.nttest.microsoft.com"
    Get-NetworkControllerAccessControlList -ConnectionUri $uri 

# <a name="look-at-ip-configuration-and-virtual-subnets-which-are-referencing-this-acl"></a>Examinar a configuração de IP e sub-redes virtuais que fazem referência a esta ACL

1. [Hoster] Execute ``Get-ProviderAddress`` no Hyper-V hosts hospedando as duas máquinas virtuais em questão de locatário em, em seguida, execute ``Test-LogicalNetworkConnection`` ou ``ping -c <compartment>`` do host do Hyper-V para validar a conectividade na rede lógica do provedor de HNV
2.  [Hoster] Certifique-se de que as configurações de MTU estão corretos nos hosts do Hyper-V e qualquer camada 2 alternar dispositivos entre os Hosts do Hyper-V. Executar ``Test-EncapOverheadValue`` em todos os hosts do Hyper-V em questão. Verifique também que todos os comutadores de camada 2 entre tem MTU definido como menos bytes 1674 para levar em conta a sobrecarga de máximo de bytes de 160.  
3.  [Hoster] Se não houver endereços de IP do PA e/ou da autoridade de certificação conectividade for interrompida, verifique se a política de rede foi recebida. Executar ``Get-PACAMapping`` para ver se as regras de encapsulamento e os mapeamentos CA-PA necessários para criar redes virtuais de sobreposição são estabelecidos corretamente.  
4.  [Hoster] Verifique se o agente de Host do controlador de rede é conectado ao controlador de rede. Executar ``netstat -anp tcp |findstr 6640`` para ver se a   
5.  [Hoster] Verifique se o Host ID em HKLM / coincide com a ID da instância dos recursos do servidor que hospeda as máquinas virtuais de locatário.  
6. [Hoster] Verifique se a ID de perfil de porta corresponde a ID da instância VM Interfaces de rede de máquinas virtuais do locatário.  

## <a name="logging-tracing-and-advanced-diagnostics"></a>Registro em log, rastreamento e diagnósticos avançados

As seções a seguir fornecem informações sobre diagnóstico avançado, log e rastreamento.

### <a name="network-controller-centralized-logging"></a>Registro em log centralizado de controlador de rede 

O controlador de rede pode coletar logs do depurador e armazená-los em um local centralizado automaticamente. Coleta de log pode ser habilitada quando você implanta o controlador de rede para a primeira vez ou a qualquer momento mais tarde. Os logs são coletados do controlador de rede e gerenciados pelo controlador de rede de elementos de rede: hospedar máquinas, balanceadores de carga de software (SLB) e as máquinas de gateway. 

Esses logs são logs de depuração para o cluster de controlador de rede, o aplicativo do controlador de rede, os logs de gateway, SLB, a rede virtual e o firewall distribuído. Sempre que um novo host/SLB/gateway é adicionado ao controlador de rede, registro em log é iniciado nessas máquinas. Da mesma forma, quando um host/SLB/gateway é removido do controlador de rede, registro em log é interrompido nessas máquinas.

#### <a name="enable-logging"></a>Habilitar registro em log

Registro em log é habilitado automaticamente quando você instala o cluster de controlador de rede usando o **Install-NetworkControllerCluster** cmdlet. Por padrão, os logs são coletados localmente em nós de controlador de rede no *%systemdrive%\SDNDiagnostics*. Vale **altamente RECOMENDÁVEL** que você alterar esse local para ser um compartilhamento de arquivo remoto (não local). 

Os logs de cluster de controlador de rede são armazenados em *%programData%\Windows Fabric\log\Traces*. Você pode especificar um local centralizado para coleta de log com o **DiagnosticLogLocation** parâmetro com a recomendação que isso também é ser um compartilhamento de arquivo remoto. 

Se você quiser restringir o acesso a esse local, você pode fornecer as credenciais de acesso com o **LogLocationCredential** parâmetro. Se você fornecer as credenciais para acessar o local do log, você também deve fornecer o **CredentialEncryptionCertificate** parâmetro, que é usado para criptografar as credenciais armazenadas localmente em nós de controlador de rede.  

Com as configurações padrão, é recomendável que você tem pelo menos de 75 GB de espaço livre no local central e 25 GB em nós de local (se não usar um local central) para um cluster de controlador de rede de 3 nós.

#### <a name="change-logging-settings"></a>Alterar as configurações de log

Você pode alterar as configurações de registro em log a qualquer momento usando o ``Set-NetworkControllerDiagnostic`` cmdlet. As configurações a seguir podem ser alteradas:

- **Local de log centralizado**.  Você pode alterar o local para armazenar os logs, com o ``DiagnosticLogLocation`` parâmetro.  
- **Credenciais para acessar o local do log**.  Você pode alterar as credenciais para acessar o local do log, com o ``LogLocationCredential`` parâmetro.  
- **Mover para o registro em log local**.  Se você tiver fornecido um local centralizado para armazenar os logs, você pode mover de volta ao registro em log localmente em nós de controlador de rede com o ``UseLocalLogLocation`` parâmetro (não é recomendado devido a requisitos de espaço em disco grande).  
- **Escopo de registro em log**.  Por padrão, todos os logs são coletados. Você pode alterar o escopo para coletar somente logs de cluster de controlador de rede.  
- **Nível de log**.  O nível de log padrão é informativo. Você pode alterá-lo para o erro, aviso ou detalhado.  
- **Log de tempo de classificação por vencimento**.  Os logs são armazenados de forma circular. Você terá três dias de dados de registro em log por padrão, se você usar o registro em log local ou logon centralizado. Você pode alterar esse limite de tempo com **LogTimeLimitInDays** parâmetro.  
- **Log de tamanho de classificação por vencimento**.  Por padrão, você terá um máximo 75 GB de dados de log se usando o registro em log centralizado e 25 GB se usando o registro em log local. Você pode alterar esse limite com o **LogSizeLimitInMBs** parâmetro.

#### <a name="collecting-logs-and-traces"></a>Rastreamentos e coleta de Logs

Implantações de VMM usam log centralizado para o controlador de rede por padrão. O local de compartilhamento de arquivos para esses logs é especificado ao implantar o modelo de serviço do controlador de rede.

Se um local de arquivo não foi especificado, o local será usado em cada nó do controlador de rede registro em log com os logs salvos em C:\Windows\tracing\SDNDiagnostics. Esses logs são salvos usando a seguinte hierarquia:

- CrashDumps
- NCApplicationCrashDumps
- NCApplicationLogs
- PerfCounters
- SDNDiagnostics
- Rastreamentos

O controlador de rede usa o Service Fabric (do Azure). Logs do Service Fabric podem ser necessários quando determinados problemas de solução de problemas. Esses logs podem ser encontrados em cada nó do controlador de rede na malha de C:\ProgramData\Microsoft\Service.

Se um usuário não tiver executado o _NetworkController depuração_ cmdlet, logs adicionais estarão disponíveis em cada host do Hyper-V que foi especificada com um recurso de servidor no controlador de rede. Esses logs (e os rastreamentos se habilitado) são mantidos em C:\NCDiagnostics

### <a name="slb-diagnostics"></a>Diagnóstico SLB

#### <a name="slbm-fabric-errors-hosting-service-provider-actions"></a>Erros de malha de SLBM (ações de provedor de serviço de hospedagem)

1.  Verifique se o Gerenciador de Balanceador de carga de Software (SLBM) está funcionando e que as camadas de orquestração podem se comunicar entre si: SLBM -> SLB Mux e SLBM -> agentes de Host do SLB. Execute [DumpSlbRestState](https://github.com/Microsoft/SDN/blob/master/Diagnostics/DumpSlbRestState.ps1) de qualquer nó com acesso ao ponto de extremidade de REST de controlador de rede.  
2.  Validar a *SDNSLBMPerfCounters* no PerfMon em uma das VMs (preferencialmente o controlador de rede nó primário - Get-NetworkControllerReplica) do nó de controlador de rede:
    1.  Mecanismo de Balanceador de carga (LB) está conectado a SLBM? (*SLBM LBEngine configurações Total* > 0)  
    2.  O SLBM pelo menos sabe sobre seus próprios pontos de extremidade? (*Total de pontos de extremidade de VIP* > = 2)  
    3.  Hosts do Hyper-V (DIP) estão conectadas slbm? (*HP clientes conectados* = = num servidores)   
    4.  SLBM está conectado a Muxes? (*Muxes conectados* == *Muxes Íntegro no SLBM* == *Muxes reporting Íntegro* = n º de VMs de Muxes SLB).  
3.  Verifique se o roteador BGP configurado está emparelhamento com êxito com o SLB MUX  
    1.  Se estiver usando o RRAS com o acesso remoto (ou seja, máquina virtual via protocolo BGP):  
        1.  Get-BgpPeer deve aparecer conectado  
        2.  Get-BgpRouteInformation deve mostrar pelo menos uma rota para o SLBM self VIP  
    2.  Se usar físico Top-of-Rack (ToR) alternar como par de BGP, consulte a documentação  
        1.  Por exemplo: # Mostrar a instância de bgp  
4.  Validar a *SlbMuxPerfCounters* e *SLBMUX* contadores no PerfMon na VM SLB Mux
5.  Verificar o estado de configuração e intervalos de VIP no recurso de Gerenciador de Balanceador de carga de Software  
    1.  Get-NetworkControllerLoadBalancerConfiguration - ConnectionUri < https://<FQDN or IP>| convertto-json-profundidade 8 (Verifique os intervalos de VIP em Pools de IPS e certifique-se de self-VIP SLBM (*LoadBalanacerManagerIPAddress*) e qualquer VIPs voltadas para o locatário estão dentro desses intervalos)  
        1. "< Pública/privada VIP lógico rede ID do recurso >" Get-NetworkControllerIpPool - NetworkId - SubnetId "< pública/privada VIP lógico sub-rede ID do recurso >" - ResourceId "<IP Pool Resource Id>" - ConnectionUri $uri | convertto-json-profundidade 8 
    2.  Debug-NetworkControllerConfigurationState -  

Se qualquer uma das verificações acima falhar, o estado do SLB de locatário também será em um modo de falha.  

**Correção**   
Com base nas seguintes informações de diagnóstico apresentadas, corrija o seguinte:  
* Certifique-se de que SLB multiplexadores estão conectados  
  * Corrigir problemas de certificado  
  * Corrigir problemas de conectividade de rede  
* Verifique se as informações sobre o emparelhamento via protocolo BGP está configurado com êxito  
* Certifique-se a ID do Host no registro corresponde à identificação de instância de servidor no servidor de recurso (apêndice para fazer referência a *HostNotConnected* código de erro)  
* Coletar registros  

#### <a name="slbm-tenant-errors-hosting-service-provider--and-tenant-actions"></a>Erros de locatário de SLBM (hospedagem locatário e o provedor de ações de serviço)

1.  [Hoster] Verifique *depuração NetworkControllerConfigurationState* para ver se qualquer recurso de Balanceador de carga está em um estado de erro. Tente reduzir seguindo os itens de ação de tabela no apêndice.   
    1.  Verifique se um ponto de extremidade de VIP está presente e rotas de anúncios  
    2.  Verifique quantos pontos de extremidade DIP foram descobertos para o ponto de extremidade de VIP  
2.  [Locatário] Validar recursos do balanceador de carga estão especificados corretamente  
    1.  Validar o DIP pontos de extremidade que são registrados no SLBM estiver hospedando máquinas virtuais de locatário que correspondem às configurações de IP de pool de endereços de Back-end do LoadBalancer  
3.  [Hoster] Se os pontos de extremidade DIP não são descobertos ou conectados:   
    1.  Verificar *NetworkControllerConfigurationState de depuração*  
        1.  Validar esse NC e agente de Host do SLB com êxito é conectado usando o coordenador de eventos do controlador de rede ``netstat -anp tcp |findstr 6640)``  
    2.  Verifique *HostId* na *nchostagent* regkey de serviço (referência *HostNotConnected* código de erro no apêndice) corresponde à instância do recurso de servidor correspondente (Id ``Get-NCServer |convertto-json -depth 8``)  
    3.  Verifique a id de perfil de porta para a porta da máquina virtual corresponde à Id da instância do recurso NIC de máquina virtual correspondente   
4.  [Hospedagem do provedor] Coletar logs   

#### <a name="slb-mux-tracing"></a>Rastreamento do SLB Mux

Informações de Muxes do balanceador de carga de Software também podem ser determinadas por meio do Visualizador de eventos. 
1. Clique em "Mostrar analítica e Logs de depuração" no menu de exibição do Visualizador de eventos
2. Navegue até "Applications and Services Logs" > Microsoft > Windows > SlbMuxDriver > de rastreamento no Visualizador de eventos 
3. Clique com o botão direito nele e selecione "Habilitar Log"

>[!NOTE]
>É recomendável que você só pode ter esse log habilitado por um curto período enquanto você está tentando reproduzir um problema

### <a name="vfp-and-vswitch-tracing"></a>VFP e rastreamento de vSwitch

De qualquer Hyper-V de host que está hospedando um convidado VM anexada a uma rede virtual do locatário, você pode coletado um rastreamento VFP para determinar onde os problemas podem estar.

```
netsh trace start provider=Microsoft-Windows-Hyper-V-VfpExt overwrite=yes tracefile=vfp.etl report=disable provider=Microsoft-Windows-Hyper-V-VmSwitch 
netsh trace stop
netsh trace convert .\vfp.etl ov=yes 
```
