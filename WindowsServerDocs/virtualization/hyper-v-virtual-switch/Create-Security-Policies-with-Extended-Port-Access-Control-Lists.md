---
title: Criar Políticas de Segurança com Listas de Controle de Acesso de Porta Estendida
description: Este tópico fornece informações sobre ACLs (listas de controle de acesso) de porta estendidas no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-hv-switch
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a92e61c3-f7d4-4e42-8575-79d75d05a218
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f76a3146c1cb38dab26019be655fadbd15d924c5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365596"
---
# <a name="create-security-policies-with-extended-port-access-control-lists"></a>Criar Políticas de Segurança com Listas de Controle de Acesso de Porta Estendida

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico fornece informações sobre ACLs (listas de controle de acesso) de porta estendidas no Windows Server 2016. É possível configurar ACLs estendidas no comutador virtual do Hyper-V para permitir e bloquear o tráfego da rede de/para as VMs (Máquinas Virtuais) conectadas ao comutador através de adaptadores de rede virtuais.  
  
Este tópico contém as seguintes seções.  
  
-   [Regras de ACL detalhadas](#bkmk_detailed)  
  
-   [Regras de ACL com estado](#bkmk_stateful)  
  
## <a name="bkmk_detailed"></a>Regras de ACL detalhadas  
ACLs estendidas do comutador virtual do Hyper-V permitem criar regras detalhadas que você pode aplicar a adaptadores de rede VM individuais que estão conectados ao comutador virtual Hyper-V. A capacidade de criar regras detalhadas permite que empresas e CSPs (provedores de serviços de nuvem) resolvam ameaças de segurança baseadas em rede em um ambiente de servidor compartilhado multilocatário.  
  
Com as ACLs estendidas, em vez de ter que criar regras abrangentes que bloqueiem ou permitam todo o tráfego de todos os protocolos de ou para uma máquina virtual, é possível bloquear ou permitir o tráfego de rede de protocolos individuais que estão sendo executando nas VMs. Você pode criar regras de ACL estendidas no Windows Server 2016 que incluem o seguinte conjunto de parâmetros de 5 tuplas: endereço IP de origem, endereço IP de destino, protocolo, porta de origem e porta de destino. Além disso, cada regra pode especificar a direção específica do tráfego da rede (entrada ou saída), bem como a ação que a regra apoia (bloquear ou permitir tráfego).  
  
Por exemplo, é possível configurar ACLs de porta para uma máquina virtual para permitir todo o tráfego que entra e sai de HTTP e HTTPS na porta 80, ao mesmo tempo que bloqueia o trafego de rede de todos os demais protocolos em todas as portas.  
  
Essa capacidade para designar o tráfego do protocolo que pode ou não ser recebido pelas VMs do locatário proporciona flexibilidade durante a configuração de políticas de segurança.  
  
### <a name="configuring-acl-rules-with-windows-powershell"></a>Configurando regras de ACL com o Windows PowerShell  
Para configurar uma ACL estendida, é necessário usar o comando do Windows PowerShell **Add-VMNetworkAdapterExtendedAcl**. Esse comando possui quatro sintaxes diferentes, cada uma com uso distinto:  
  
1.  Adicione uma ACL estendida a todos os adaptadores de rede de uma VM nomeada, que é especificada pelo primeiro parâmetro,-VMName. Sintaxe:  
  
    > [!NOTE]  
    > Se você quiser adicionar uma ACL estendida a um adaptador de rede em vez de todos, poderá especificar o adaptador de rede com o parâmetro-VMNetworkAdapterName.  
  
    ```  
    Add-VMNetworkAdapterExtendedAcl [-VMName] <string[]> [-Action] <VMNetworkAdapterExtendedAclAction> {Allow | Deny}  
        [-Direction] <VMNetworkAdapterExtendedAclDirection> {Inbound | Outbound} [[-LocalIPAddress] <string>]  
        [[-RemoteIPAddress] <string>] [[-LocalPort] <string>] [[-RemotePort] <string>] [[-Protocol] <string>] [-Weight]  
        <int> [-Stateful <bool>] [-IdleSessionTimeout <int>] [-IsolationID <int>] [-Passthru] [-VMNetworkAdapterName  
        <string>] [-ComputerName <string[]>] [-WhatIf] [-Confirm]  [<CommonParameters>]  
    ```  
  
2.  Adicione uma ACL estendida a um adaptador de rede virtual específico em uma VM específica. Sintaxe:  
  
    ```  
    Add-VMNetworkAdapterExtendedAcl [-VMNetworkAdapter] <VMNetworkAdapterBase[]> [-Action]  
        <VMNetworkAdapterExtendedAclAction> {Allow | Deny} [-Direction] <VMNetworkAdapterExtendedAclDirection> {Inbound |  
        Outbound} [[-LocalIPAddress] <string>] [[-RemoteIPAddress] <string>] [[-LocalPort] <string>] [[-RemotePort]  
        <string>] [[-Protocol] <string>] [-Weight] <int> [-Stateful <bool>] [-IdleSessionTimeout <int>] [-IsolationID  
        <int>] [-Passthru] [-WhatIf] [-Confirm]  [<CommonParameters>]  
    ```  
  
3.  Adicione uma ACL estendida a todos os adaptadores da rede virtual, reservados para utilização pelo sistema operacional de gerenciamento do host do Hyper-V.  
  
    > [!NOTE]  
    > Se você quiser adicionar uma ACL estendida a um adaptador de rede em vez de todos, poderá especificar o adaptador de rede com o parâmetro-VMNetworkAdapterName.  
  
    ```  
    Add-VMNetworkAdapterExtendedAcl [-Action] <VMNetworkAdapterExtendedAclAction> {Allow | Deny} [-Direction]  
        <VMNetworkAdapterExtendedAclDirection> {Inbound | Outbound} [[-LocalIPAddress] <string>] [[-RemoteIPAddress]  
        <string>] [[-LocalPort] <string>] [[-RemotePort] <string>] [[-Protocol] <string>] [-Weight] <int> -ManagementOS  
        [-Stateful <bool>] [-IdleSessionTimeout <int>] [-IsolationID <int>] [-Passthru] [-VMNetworkAdapterName <string>]  
        [-ComputerName <string[]>] [-WhatIf] [-Confirm]  [<CommonParameters>]  
    ```  
  
4.  Adicione uma ACL estendida a um objeto de VM que você criou no Windows PowerShell, como **$VM = Get-VM "my_vm"** . Na linha seguinte do código, é possível executar este comando para criar uma ACL estendida com a sintaxe a seguir:  
  
    ```  
    Add-VMNetworkAdapterExtendedAcl [-VM] <VirtualMachine[]> [-Action] <VMNetworkAdapterExtendedAclAction> {Allow |  
        Deny} [-Direction] <VMNetworkAdapterExtendedAclDirection> {Inbound | Outbound} [[-LocalIPAddress] <string>]  
        [[-RemoteIPAddress] <string>] [[-LocalPort] <string>] [[-RemotePort] <string>] [[-Protocol] <string>] [-Weight]  
        <int> [-Stateful <bool>] [-IdleSessionTimeout <int>] [-IsolationID <int>] [-Passthru] [-VMNetworkAdapterName  
        <string>] [-WhatIf] [-Confirm]  [<CommonParameters>]  
    ```  
  
### <a name="detailed-acl-rule-examples"></a>Exemplos de regras de ACL detalhadas  
A seguir, apresentamos alguns exemplos de como usar o comando **Add-VMNetworkAdapterExtendedAcl** para configurar ACLs de porta estendida e para criar políticas de segurança para VMs.  
  
-   [Impor segurança em nível de aplicativo](#bkmk_enforce)  
  
-   [Impor a segurança em nível de usuário e de aplicativo](#bkmk_both)  
  
-   [Fornecer suporte de segurança para um aplicativo não TCP/UDP](#bkmk_tcp)  
  
> [!NOTE]  
> Os valores para o parâmetro da regra de **Direção** nas tabelas abaixo baseiam-se no fluxo do tráfego de ou para a VM para a qual a regra está sendo criada. Se a VM estiver recebendo tráfego, o tráfego é de entrada; se a VM estiver enviando tráfego, o tráfego é de saída. Por exemplo, se você aplicar uma regra a uma VM que bloqueia o tráfego de entrada, a direção do tráfego de entrada será dos recursos externos para a VM. Se você aplicar uma regra que bloqueia o tráfego de saída, a direção do tráfego de saída será da VM local para recursos externos.  
  
### <a name="bkmk_enforce"></a>Impor segurança em nível de aplicativo  
Tendo em vista que muitos servidores de aplicativos utilizam portas TCP/UDP padronizadas para comunicação com computadores clientes, é fácil criar regras que bloqueiem ou permitam o acesso a um servidor de aplicativos, filtrando-se o tráfego que vai ou vem da porta designada para o aplicativo.  
  
Por exemplo, se você quisesse permitir a um usuário fazer login em um servidor de aplicativos no datacenter usando a RDP (Conexão com Área de Trabalho Remota). Uma vez que a RDP usa a porta TCP 3389, é possível configurar rapidamente a regra a seguir:  
  
|IP de Origem|IP de destino|Protocol|Porta de Origem|Porta de destino|Direction|Ação|  
|-------------|------------------|------------|---------------|--------------------|-------------|----------|  
|*|*|TCP|*|3389|Dentro|Allow|  
  
Apresentamos a seguir dois exemplos de como criar regras com os comandos do Windows PowerShell. A primeira regra de exemplo bloqueia todo o tráfego para a VM chamada "ApplicationServer". A segunda regra de exemplo, que é aplicada ao adaptador de rede da VM denominada "ApplicationServer", permite apenas o tráfego RDP de entrada para a VM.  
  
> [!NOTE]  
> Ao criar regras, você pode usar o parâmetro **-Weight** para determinar a ordem na qual o comutador virtual do Hyper-V processa as regras. Valores de **peso** são expressos como inteiros; as regras com um inteiro maior são processadas antes das regras com inteiros menores. Por exemplo, se você tiver aplicado duas regras a um adaptador de rede da VM, uma com um peso de 1 e outra com o peso de 10, a com peso de 10 será aplicada primeiro.  
  
```  
Add-VMNetworkAdapterExtendedAcl -VMName "ApplicationServer" -Action "Deny" -Direction "Inbound" -Weight 1  
Add-VMNetworkAdapterExtendedAcl -VMName "ApplicationServer" -Action "Allow" -Direction "Inbound" -LocalPort 3389 -Protocol "TCP" -Weight 10  
```  
  
### <a name="bkmk_both"></a>Impor a segurança em nível de usuário e de aplicativo  
Uma vez que a regra pode corresponder a um pacote IP com 5 tuplas (IP de Origem, IP de Destino, Protocolo, Porta de Origem e Porta de Destino), a regra pode impor uma política de segurança mais detalhada do que uma ACL de porta.  
  
Por exemplo, se você quiser fornecer um serviço DHCP para um número limitado de computadores cliente usando um conjunto específico de servidores DHCP, poderá configurar as seguintes regras no computador com Windows Server 2016 que está executando o Hyper-V, onde as VMs do usuário estão hospedadas:  
  
|IP de Origem|IP de destino|Protocol|Porta de Origem|Porta de destino|Direction|Ação|  
|-------------|------------------|------------|---------------|--------------------|-------------|----------|  
|*|255.255.255.255|UDP|*|67|Fora|Allow|  
|*|10.175.124.0/25|UDP|*|67|Fora|Allow|  
|10.175.124.0/25|*|UDP|*|68|Dentro|Allow|  
  
Apresentamos a seguir dois exemplos de como criar regras com os comandos do Windows PowerShell.  
  
```  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Deny" -Direction "Outbound" -Weight 1  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Allow" -Direction "Outbound" -RemoteIPAddress 255.255.255.255 -RemotePort 67 -Protocol "UDP"-Weight 10  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Allow" -Direction "Outbound" -RemoteIPAddress 10.175.124.0/25 -RemotePort 67 -Protocol "UDP"-Weight 20  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Allow" -Direction "Inbound" -RemoteIPAddress 10.175.124.0/25 -RemotePort 68 -Protocol "UDP"-Weight 20  
```  
  
### <a name="bkmk_tcp"></a>Fornecer suporte de segurança para um aplicativo não TCP/UDP  
Enquanto a maior parte do tráfego da rede em um datacenter é TCP e UDP, ainda existe tráfego que utiliza outros protocolos. Por exemplo, se desejar permitir que um grupo de servidores execute um aplicativo com multicast do IP que depende do protocolo IGMP, é possível criar a regra a seguir.  
  
> [!NOTE]  
> O IGMP tem um número de protocolo IP designado de 0x02.  
  
|IP de Origem|IP de destino|Protocol|Porta de Origem|Porta de destino|Direction|Ação|  
|-------------|------------------|------------|---------------|--------------------|-------------|----------|  
|*|*|0x02|*|*|Dentro|Allow|  
|*|*|0x02|*|*|Fora|Allow|  
  
Apresentamos a seguir um exemplo de como criar essas regras com os comandos do Windows PowerShell.  
  
```  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Allow" -Direction "Inbound" -Protocol 2 -Weight 20  
Add-VMNetworkAdapterExtendedAcl -VMName "ServerName" -Action "Allow" -Direction "Outbound" -Protocol 2 -Weight 20  
```  
  
## <a name="bkmk_stateful"></a>Regras de ACL com estado  
Outro recurso novo das ACLs estendidas permite a configuração de regras de monitoração de estado. Uma regra com estado filtra pacotes com base em cinco atributos em um IP de origem de pacote, IP de destino, protocolo, porta de origem e porta de destino.  
  
As regras de monitoração de estado têm os seguintes recursos:  
  
-   Elas sempre permitem o tráfego e não são usadas para bloquear o tráfego.  
  
-   Se especificar que o valor do parâmetro de **Direção** é de entrada e o tráfego corresponde à regra, o comutador virtual do Hyper-V cria uma regra correspondente dinamicamente, a qual permite que a VM envie o tráfego de saída em resposta ao recurso externo.  
  
-   Se especificar que o valor do parâmetro de **Direção** é de saída e o tráfego corresponde à regra, o comutador virtual do Hyper-V cria uma regra correspondente dinamicamente, a qual permite que a VM receba o tráfego de entrada do recurso externo.  
  
-   Elas incluem um atributo de tempo limite, medido em segundos. Quando um pacote da rede chega ao comutador e ele corresponde à regra de monitoração de estado, o comutador virtual do Hyper-V cria um estado, de modo que todos os pacotes subsequentes – em ambas as direções do mesmo fluxo – sejam permitidos. O estado espirará se não houver tráfego em nenhuma das direções no período de tempo especificado pelo valor do tempo limite.  
  
A seguir, apresentamos um exemplo de como usar as regras de monitoração de estado.  
  
### <a name="allow-inbound-remote-server-traffic-only-after-it-is-contacted-by-the-local-server"></a>Permitir o tráfego de entrada do servidor remoto somente depois de ele ter sido contactado pelo servidor local  
Em alguns casos, uma regra de monitoração de estado deve ser empregada tendo em vista que somente uma regra desse tipo pode monitorar uma conexão estabelecida conhecida e diferenciar uma conexão da outra.  
  
Por exemplo, se desejar permitir que um servidor de aplicativos da VM inicie conexões na porta 80 com os serviços Web na Internet e desejar que os servidores remotos da Web consigam responder ao tráfego da VM, será possível configurar uma regra de monitoração de estado que permita o tráfego de saída inicial da VM para os serviços Web; uma vez que a regra é de monitoração de estado, o tráfego de retorno para a VM dos servidores Web será igualmente permitido. Por motivos de segurança, é possível bloquear todo o tráfego de entrada da rede para a VM.  
  
Para configurar essa regra, será possível utilizar as definições da tabela abaixo.  
  
> [!NOTE]  
> Devido às restrições de formatação e à quantidade de informações na tabela abaixo, as informações estão exibidas de forma diferente das tabelas anteriores neste documento.  
  
|Parâmetro|Regra 1|Regra 2|Regra 3|  
|-------------|----------|----------|----------|  
|IP de Origem|*|*|*|  
|IP de destino|*|*|*|  
|Protocol|*|*|TCP|  
|Porta de Origem|*|*|*|  
|Porta de destino|*|*|80|  
|Direction|Dentro|Fora|Fora|  
|Ação|Negar|Negar|Allow|  
|Com monitoração de estado|Não|Não|Sim|  
|Tempo limite (em segundos)|N/D|N/D|3\.600|  
  
A regra de monitoração de estado permite que o servidor de aplicativos da VM conecte-se a um servidor remoto da Web. Quando o primeiro pacote é enviado, o comutador virtual do Hyper-V cria dinamicamente dois estados de fluxo, a fim de permitir que todos os pacotes sejam enviados para e todos os pacotes de retorno sejam enviados do servidor remoto da Web. Quando o fluxo de pacotes entre os servidores para, o fluxo estabelece o tempo limite no valor de tempo limite de 3.600 segundos ou uma hora.  
  
Apresentamos a seguir um exemplo de como criar essas regras com os comandos do Windows PowerShell.  
  
```  
Add-VMNetworkAdapterExtendedAcl -VMName "ApplicationServer" -Action "Deny" -Direction "Inbound" -Weight 1   
Add-VMNetworkAdapterExtendedAcl -VMName "ApplicationServer" -Action "Deny" -Direction "Outbound" -Weight 1  
Add-VMNetworkAdapterExtendedAcl -VMName "ApplicationServer" -Action "Allow" -Direction "Outbound" 80 "TCP" -Weight 100 -Stateful -Timeout 3600  
```  
  


