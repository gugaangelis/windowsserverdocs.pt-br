---
title: Implantar DHCP usando o Windows PowerShell
description: You can use this topic to deploy a Windows Server 2016 Internet Protocol (IP) version 4 DHCP server that provides automatic IP addresses and DHCP options to IPv4 DHCP clients connected to one or more subnets on your network.
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: article
ms.assetid: 7110ad21-a33e-48d5-bb3c-129982913bc8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 2be3c02f32229c9b9ee411f5e97305776f9825d8
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="deploy-dhcp-using-windows-powershell"></a>Implantar DHCP usando o Windows PowerShell

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

This guide provides instructions on how to use Windows PowerShell to deploy an Internet Protocol (IP) version 4 Dynamic Host Configuration Protocol \(DHCP\)  server that automatically assigns IP addresses and DHCP options to IPv4 DHCP clients that are connected to one or more subnets on your network.

>[!NOTE]
>To download this document in Word format from TechNet Gallery, see [Deploy DHCP Using Windows PowerShell in Windows Server 2016](https://gallery.technet.microsoft.com/Deploy-DHCP-Using-Windows-246dd293).

Using DHCP servers to assign IP addresses saves in administrative overhead because you do not need to manually configure the TCP/IP v4 settings for every network adapter in every computer on your network. With DHCP, TCP/IP v4 configuration is performed automatically when a computer or other DHCP client is connected to your network.

You can deploy your DHCP server in a workgroup as a standalone server, or as part of an Active Directory domain.

Este guia contém as seguintes seções.

- [DHCP Deployment Overview](#bkmk_overview)
- [Visões gerais de tecnologia](#bkmk_technologies)
- [Plan DHCP Deployment](#bkmk_plan)
- [Using This Guide in a Test Lab](#bkmk_lab)
- [Deploy DHCP](#bkmk_deploy)
- [Verify Server Functionality](#bkmk_verify)
- [Windows PowerShell Commands for DHCP](#bkmk_dhcpwps)
- [List of Windows PowerShell Commands in this guide](#bkmk_list)

## <a name="bkmk_overview"></a>DHCP Deployment Overview

The following illustration depicts the scenario that you can deploy by using this guide. The scenario includes one DHCP server in an Active Directory domain. The server is configured to provide IP addresses to DHCP clients on two different subnets. The subnets are separated by a router that has DHCP Forwarding enabled.

![DHCP Network Topology Overview](../../media/Core-Network-Guide/cng16_overview.jpg)

## <a name="bkmk_technologies"></a>Technology Overviews

The following sections provide brief overviews of DHCP and TCP/IP.

### <a name="dhcp-overview"></a>DHCP overview

DHCP é um padrão para simplificar o gerenciamento de configuração de IP do host IP. O padrão DHCP fornece o uso de servidores DHCP como uma maneira de gerenciar a alocação dinâmica de endereços IP e outros detalhes de configuração relacionados para clientes DHCP em sua rede.

DHCP allows you to use a DHCP server to dynamically assign an IP address to a computer or other device, such as a printer, on your local network, rather than manually configuring every device with a static IP address.

Todos os computadores em uma rede TCP/IP devem ter um endereço IP exclusivo, porque o endereço IP e máscara de sub-rede relacionada é identificam o computador host e a sub-rede ao qual o computador está conectado. Usando o DHCP, você pode garantir que todos os computadores que estão configurados como clientes DHCP recebem um endereço IP que é adequado para seu local de rede e a sub-rede e usando as opções de DHCP, como gateway padrão e servidores DNS, você pode fornecer automaticamente clientes DHCP com as informações que eles precisam funcionar corretamente em sua rede.

For TCP/IP-based networks, DHCP reduces the complexity and amount of administrative work involved in configuring computers.

### <a name="tcpip-overview"></a>TCP/IP overview

By default, all versions of Windows Server and Windows Client operating systems have TCP/IP settings for IP version 4 network connections configured to automatically obtain an IP address and other information, called DHCP options, from a DHCP server. Because of this, you do not need to configure TCP/IP settings manually unless the computer is a server computer or other device that requires a manually configured, static IP address. 

For example, it is recommended that you manually configure the IP address of the DHCP server, and the IP addresses of DNS servers and domain controllers that are running Active Directory Domain Services \(AD DS\).

TCP/IP no Windows Server 2016 é o seguinte:

-   Software de rede com base em protocolos de rede padrão da indústria.

-   Um protocolo de rede corporativa pode ser roteado que dá suporte a conexão do computador baseado no Windows para a rede local (LAN) e ambientes de rede (WAN) de longa distância.

-   Principais tecnologias e utilitários para conectar seu computador baseado em Windows sistemas diferentes para fins de compartilhamento de informações.

-   A foundation for gaining access to global Internet services, such as Web and File Transfer Protocol (FTP) servers.

-   Uma estrutura de cliente/servidor robusta, escalonável e de plataforma cruzada.

TCP/IP oferece utilitários TCP/IP básicos que permitem aos computadores baseados no Windows para se conectar e compartilhar informações com outros sistemas Microsoft e não são da Microsoft, incluindo:

- Windows Server 2016

- Windows 10

- Windows Server 2012 R2

- Windows 8.1

- Windows Server 2012

- Windows 8

- Windows Server 2008 R2

- Windows 7

- Windows Server 2008

- Windows Vista

- Hosts de Internet

- Sistemas Apple Macintosh

- Mainframes IBM

- Sistemas UNIX e Linux

- Sistemas VMS abertos

- Impressoras prontas para a rede

- Tablets e telefones celulares com Ethernet com fio ou tecnologia sem fio 802.11 habilitada

## <a name="bkmk_plan"></a>Plan DHCP Deployment

Following are key planning steps before installing the DHCP server role.

### <a name="planning-dhcp-servers-and-dhcp-forwarding"></a>Planejamento servidores DHCP e o encaminhamento de DHCP

Como mensagens DHCP são mensagens de transmissão, eles não são encaminhados entre sub-redes por roteadores. Se você tiver várias sub-redes e fornecer o serviço DHCP para cada sub-rede, faça um destes procedimentos:

-   Instalar um servidor DHCP em cada sub-rede

-   Configure roteadores para encaminhar mensagens de transmissão de DHCP em sub-redes e configurar vários escopos no servidor DHCP, um escopo por sub-rede.

Na maioria dos casos, é mais econômico que a implantação de um servidor DHCP em cada segmento físico da rede configurar roteadores para encaminhar mensagens de transmissão de DHCP.

### <a name="planning-ip-address-ranges"></a>Planejamento de intervalos de endereço IP

Cada sub-rede deve ter seu próprio intervalo de endereços IP exclusivo. Esses intervalos são representados em um servidor DHCP com escopos.

Um escopo é um agrupamento administrativo de endereços IP para computadores em uma sub-rede que usam o serviço DHCP. O administrador primeiro cria um escopo para cada sub-rede física e, em seguida, usa o escopo para definir os parâmetros usados pelos clientes.

Um escopo tem as seguintes propriedades:

-   Um intervalo de endereços IP dos quais incluir ou excluir endereços usados para ofertas de concessão de serviço DHCP.

-   Uma máscara de sub-rede, que determina o prefixo da sub-rede para um determinado endereço IP.

-   Um nome de escopo atribuído quando ele é criado.

-   Valores de duração de concessão, que são atribuídos a clientes DHCP que recebem endereços IP alocados dinamicamente.

-   Qualquer opção de escopo DHCP configurada para atribuição a clientes DHCP, como endereço IP do servidor DNS e endereço IP do roteador/padrão gateway.

-   Reservas, opcionalmente, são usadas para garantir que um cliente DHCP sempre receba o mesmo endereço IP.

Antes de implantar os servidores, liste seu sub-redes e o intervalo de endereços IP que deseja usar para cada sub-rede.

### <a name="planning-subnet-masks"></a>Planejando máscaras de sub-rede

Identificações de rede e host em um endereço IP distinguem usando uma máscara de sub-rede. Cada máscara de sub-rede é um número de 32 bits que usa grupos de bits consecutivos de todos os números (1) para identificar a rede ID e zeros (0) para identificar as partes de ID de host de um endereço IP.

Por exemplo, a máscara de sub-rede normalmente usada com o endereço IP 131.107.16.200 é o seguinte número de binário de 32 bits:

```
11111111 11111111 00000000 00000000
```

Esse número de máscara de sub-rede é 16 bits de um seguido de 16 bits de zero, indicando que a rede ID e host ID seções deste endereço IP são ambas 16 bits de comprimento. Normalmente, essa máscara de sub-rede é exibida em notação de ponto decimal como 255.255.0.0.

A tabela a seguir exibe máscaras de sub-rede para as classes de endereço de Internet.

|Classe Address|Bits de máscara de sub-rede|Máscara de sub-rede|
|-----------------|------------------------|---------------|
|Classe A|11111111 00000000 00000000 00000000|255.0.0.0|
|Classe B|11111111 11111111 00000000 00000000|255.255.0.0|
|Classe C|11111111 11111111 11111111 00000000|255.255.255.0|

Quando você cria um escopo em DHCP e insira o intervalo de endereços IP para o escopo, DHCP fornece esses valores de máscara de sub-rede padrão. Normalmente, os valores de máscara de sub-rede padrão são aceitáveis para a maioria das redes sem requisitos especiais e onde cada segmento de rede IP corresponde a uma única rede física.

Em alguns casos, você pode usar máscaras de sub-rede personalizadas para implementar sub-rede IP. Com a criação de sub-redes IP, você pode subdividir a parte de identificação de host do padrão de um endereço IP para especificar sub-redes, que são subdivisões da identificação de rede baseada em classes original.

Personalizando o comprimento da máscara de sub-rede, você pode reduzir o número de bits que são usados para o identificação de host real.

Para evitar problemas de endereçamento e roteamento, você deve garantir que todos os computadores de TCP/IP em um segmento de rede usam a mesma máscara de sub-rede e que cada computador ou dispositivo tem um endereço IP exclusivo.

### <a name="planning-exclusion-ranges"></a>Planejamento de intervalos de exclusão

Quando você cria um escopo em um servidor DHCP, você pode especificar um intervalo de endereços IP que inclui todos os endereços IP que o servidor DHCP tem permissão para serem concedidos aos clientes DHCP, como computadores e outros dispositivos. Se você vá e configurar manualmente alguns servidores e endereços IP de outros dispositivos com estático do mesmo intervalo de endereços IP que esteja usando o servidor DHCP, você pode acidentalmente criar um conflito de endereço IP, onde você e o servidor DHCP ambos atribuiu o mesmo endereço IP para dispositivos diferentes.

Para resolver esse problema, você pode criar um intervalo de exclusão para o escopo DHCP. Um intervalo de exclusão é um contíguos intervalo de endereços IP dentro do alcance de endereço IP do escopo que o servidor DHCP não tem permissão para usar. Se você criar um intervalo de exclusão, o servidor DHCP não atribui os endereços nesse intervalo, permitindo que você atribuir manualmente esses endereços sem criar um conflito de endereço IP.

Você pode excluir endereços IP da distribuição pelo servidor DHCP através da criação de um intervalo de exclusão para cada escopo. Você deve usar exclusões para todos os dispositivos que estejam configurados com um endereço IP estático. Os endereços excluídos devem incluir todos os endereços IP que você atribuiu manualmente a outros servidores, clientes não-DHCP, estações de trabalho sem disco ou clientes de roteamento e acesso remoto e PPP.

É recomendável que você configure seu intervalo de exclusão com endereços extras para acomodar o crescimento futuro de rede. A tabela a seguir fornece um intervalo de exclusão de exemplo para um escopo com um intervalo de endereços IP de 10.0.0.1 - 10.0.0.254 e uma máscara de sub-rede de 255.255.255.0.

|Itens de configuração|Valores de exemplo|
|-----------------------|------------------|
|Intervalo de exclusão Start IP Address|10.0.0.1|
|Endereço IP final do intervalo de exclusão|10.0.0.25|

### <a name="planning-tcpip-static-configuration"></a>Planejar a configuração de TCP/IP estática

Determinados dispositivos, como roteadores, servidores DHCP e DNS servidores, devem ser configurados com um endereço IP estático. Além disso, você pode ter dispositivos adicionais, como impressoras, que você quer garantir que sempre têm o mesmo endereço IP. Listar os dispositivos que deseja configurar estaticamente para cada sub-rede e, em seguida, planeje o intervalo de exclusão que você deseja usar no servidor DHCP para garantir que o servidor DHCP não conceder o endereço IP de um dispositivo configurado estaticamente. Um intervalo de exclusão é uma sequência limitada de endereços IP dentro de um escopo, excluído das ofertas de serviço DHCP. Intervalos de exclusão asseguram que quaisquer endereços nesses intervalos não são oferecidos pelo servidor para clientes DHCP na sua rede.

Por exemplo, se o intervalo de endereços IP para uma sub-rede é 192.168.0.1 por meio de 192.168.0.254 e você tiver dez dispositivos que você deseja configurar com um endereço IP estático, você pode criar um intervalo de exclusão para o 192.168.0. *x* escopo que inclua dez ou mais endereços IP: 192.168.0.1 por meio de 192.168.0.15.

Neste exemplo, use dez os endereços IP excluídos configurar servidores e outros dispositivos com endereços IP estáticos e cinco endereços IP adicionais ficam disponíveis para configuração estática de novos dispositivos que deseja adicionar no futuro. Esta variedade de exclusão, o servidor DHCP for deixado com um pool de endereços do 192.168.0.16 por meio de 192.168.0.254.

Itens de configuração de exemplo adicional para o AD DS e DNS são fornecidas na tabela a seguir.

|Itens de configuração|Valores de exemplo|
|-----------------------|------------------|
|Associações de conexão de rede|Ethernet|
|Configurações do servidor DNS|DC1.corp.contoso.com|
|Endereço IP do servidor DNS preferencial|10.0.0.2|
|Scope values<br /><br />1. Escopo nome<br />2. A partir do endereço IP<br />3. Endereço IP final<br />4. Máscara de sub-rede<br />5. Padrão Gateway (opcional)<br />6. Duração da concessão|1. Principal sub-rede<br />2.  10.0.0.1<br />3.  10.0.0.254<br />4.  255.255.255.0<br />5.  10.0.0.1<br />6. 8 dias|
|Modo de operação do servidor DHCP IPv6|Não habilitado|

## <a name="bkmk_lab"></a>Using This Guide in a Test Lab

You can use this guide to deploy DHCP in a test lab before you deploy in a production environment. 

>[!NOTE]
>If you do not want to deploy DHCP in a test lab, you can skip to the section [Deploy DHCP](#bkmk_deploy).

The requirements for your lab differ depending on whether you are using physical servers or virtual machines \(VMs\), and whether you are using an Active Directory domain or deploying a standalone DHCP server. 

You can use the following information to determine the minimum resources you need to test DHCP deployment using this guide.

### <a name="test-lab-requirements-with-vms"></a>Test Lab requirements with VMs

To deploy DHCP in a test lab with VMs, you need the following resources.

For either domain deployment or standalone deployment, you need one server that is configured as a Hyper\-V host.

**Domain deployment**

This deployment requires one physical server, one virtual switch, two virtual servers, and one virtual client:

On your physical server, in Hyper-V Manager, create the following items.

1. One **Internal** virtual switch. Do not create an **External** virtual switch, because if your Hyper\-V host is on a subnet that includes a DHCP server, your test VMs will receive an IP address from your DHCP server. In addition, the test DHCP server that you deploy might assign IP addresses to other computers on the subnet where the Hyper\-V host is installed.
1. One VM running Windows Server 2106 configured as a domain controller with Active Directory Domain Services that is connected to the Internal virtual switch you created. To match this guide, this server must have a statically configured IP address of 10.0.0.2. For information on deploying AD DS, see the section **Deploying DC1** in the Windows Server 2016 [Core Network Guide](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployADDNS01).
1. One VM running Windows Server 2106 that you will configure as a DHCP server by using this guide and  that is connected to the Internal virtual switch you created. 
1. One VM running a Windows client operating system  that is connected to the Internal virtual switch you created and that you will use to verify that your DHCP server is dynamically allocating IP addresses and DHCP options to DHCP clients.

**Standalone DHCP server deployment**

This deployment requires one physical server, one virtual switch, one virtual server, and one virtual client:

On your physical server, in Hyper-V Manager, create the following items.

1. One **Internal** virtual switch. Do not create an **External** virtual switch, because if your Hyper\-V host is on a subnet that includes a DHCP server, your test VMs will receive an IP address from your DHCP server. In addition, the test DHCP server that you deploy might assign IP addresses to other computers on the subnet where the Hyper\-V host is installed.
1. One VM running Windows Server 2106 that you will configure as a DHCP server by using this guide and  that is connected to the Internal virtual switch you created.
1. One VM running a Windows client operating system  that is connected to the Internal virtual switch you created and that you will use to verify that your DHCP server is dynamically allocating IP addresses and DHCP options to DHCP clients.

### <a name="test-lab-requirements-with-physical-servers"></a>Test Lab requirements with physical servers

To deploy DHCP in a test lab with physical servers, you need the following resources.

**Domain deployment**

This deployment requires one hub or switch, two physical servers and one physical client:

1. One Ethernet hub or switch to which you can connect the physical computers with Ethernet cables
1. One physical computer running Windows Server 2106 configured as a domain controller with Active Directory Domain Services. To match this guide, this server must have a statically configured IP address of 10.0.0.2. For information on deploying AD DS, see the section **Deploying DC1** in the Windows Server 2016 [Core Network Guide](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployADDNS01).
1. One physical computer running Windows Server 2106 that you will configure as a DHCP server by using this guide. 
1. One physical computer running a Windows client operating system that you will use to verify that your DHCP server is dynamically allocating IP addresses and DHCP options to DHCP clients.

>[!NOTE]
>If you do not have enough test machines for this deployment, you can use one test machine for both AD DS and DHCP - however this configuration is not recommended for a production environment.

**Standalone DHCP server deployment**

This deployment requires one hub or switch, one physical server, and one physical client:

1. One Ethernet hub or switch to which you can connect the physical computers with Ethernet cables
2. One physical computer running Windows Server 2106 that you will configure as a DHCP server by using this guide. 
3. One physical computer running a Windows client operating system that you will use to verify that your DHCP server is dynamically allocating IP addresses and DHCP options to DHCP clients.


## <a name="bkmk_deploy"></a>Deploy DHCP

This section provides example Windows PowerShell commands that you can use to deploy DHCP on one server. Before you run these example commands on your server, you must modify the commands to match your network and environment. 

For example, before you run the commands, you should replace example values in the commands for the following items:

- Computer names
- IP Address range for each scope you want to configure (1 scope per subnet)
- Subnet mask for each IP address range you want to configure
- Scope name for each scope
- Exclusion range for each scope
- DHCP option values, such as default gateway, domain name, and DNS or WINS servers
- Interface names

>[!IMPORTANT]
>Examine and modify every command for your environment before you run the command.

### <a name="where-to-install-dhcp---on-a-physical-computer-or-a-vm"></a>Where to Install DHCP - on a physical computer or a VM?

You can install the DHCP server role on a physical computer or on a virtual machine \(VM\) that is installed on a Hyper\-V host. If you are installing DHCP on a VM and you want the DHCP server to provide IP address assignments to computers on the physical network to which the Hyper-V host is connected, you must connect the VM virtual network adapter to a Hyper-V Virtual Switch that is **External**.

For more information, see the section **Create a Virtual Switch with Hyper-V Manager** in the topic [Create a virtual network](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/connect-to-network).

### <a name="run-windows-powershell-as-an-administrator"></a>Run Windows PowerShell as an Administrator

You can use the following procedure to run Windows PowerShell with Administrator privileges.

1. Em um computador executando o Windows Server 2016, clique em **iniciar**, em seguida, clique com botão direito no ícone do Windows PowerShell. Um menu é exibido. 

2. No menu, clique em **mais**e clique em **executar como administrador**. Se solicitado, digite as credenciais de uma conta que tenha privilégios de administrador no computador. If the user account with which you are logged on to the computer is an Administrator level account, you will not receive a credential prompt.

3. Abre do Windows PowerShell com privilégios de administrador.

### <a name="rename-the-dhcp-server-and-configure-a-static-ip-address"></a>Rename the DHCP server and configure a static IP address

If you have not already done so, you can use the following Windows PowerShell commands to rename the DHCP server and configure a static IP address for the server.

**Configurar um endereço IP estático**

You can use the following commands to assign a static IP address to the DHCP server, and to configure the DHCP server TCP/IP properties with the correct DNS server IP address. Você também deve substituir os nomes de interface e endereços IP neste exemplo com os valores que você deseja usar para configurar seu computador.

 `New-NetIPAddress -IPAddress 10.0.0.3 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24`

 `Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 10.0.0.2`

For more information about these commands, see the following topics.

- [New-NetIPAddress](https://technet.microsoft.com/itpro/powershell/windows/tcpip/new-netipaddress)
- [Set-DnsClientServerAddress](https://technet.microsoft.com/itpro/powershell/windows/dns-client/set-dnsclientserveraddress)

**Renomear o computador**

You can use the following commands to rename and then restart the computer.

`Rename-Computer -Name DHCP1`

 `Restart-Computer`

For more information about these commands, see the following topics.

- [Rename-Computer](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/rename-computer)
- [Restart-Computer](https://msdn.microsoft.com/powershell/reference/4.0/microsoft.powershell.management/restart-computer)

### <a name="join-the-computer-to-the-domain-optional"></a>Join the computer to the domain \(Optional\)

If you are installing your DHCP server in an Active Directory domain environment, you must join the computer to the domain. Open Windows PowerShell with Administrator privileges, and then run the following command after replacing the domain NetBios name **CORP** with a value that is appropriate for your environment.

    Add-Computer CORP

When prompted, type the credentials for a domain user account that has permission to join a computer to the domain. 

    Restart-Computer

For more information about the Add-Computer command, see the following topic.

- [Add-Computer](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/add-computer)

### <a name="install-dhcp"></a>Install DHCP

After the computer restarts, open Windows PowerShell with Administrator privileges, and then install DHCP by running the following command.

    Install-WindowsFeature DHCP -IncludeManagementTools

For more information about this command, see the following topic.

- [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/server-manager/install-windowsfeature)

### <a name="create-dhcp-security-groups"></a>Create DHCP security groups

To create security groups, you must run a Network Shell \(netsh\) command in Windows PowerShell, and then restart the DHCP service so that the new groups become active.

When you run the following netsh command on the DHCP server, the **DHCP Administrators** and **DHCP Users** security groups are created in **Local Users and Groups** on the DHCP server.

    netsh dhcp add securitygroups

The following command restarts the DHCP service on the local computer.

    Restart-service dhcpserver

For more information about these commands, see the following topics.

- [Shell de rede (Netsh)](../netsh/netsh.md)
- [Restart-Service](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/restart-service)

### <a name="authorize-the-dhcp-server-in-active-directory-optional"></a>Authorize the DHCP server in Active Directory \(Optional\)

If you are installing DHCP in a domain environment, you must perform the following steps to authorize the DHCP server to operate in the domain.

>[!NOTE]
>Unauthorized DHCP servers that are installed in Active Directory domains cannot function properly, and do not lease IP addresses to DHCP clients. The automatic disabling of unauthorized DHCP servers is a security feature that prevents unauthorized DHCP servers from assigning incorrect IP addresses to clients on your network.

You can use the following command to add the DHCP server to the list of authorized DHCP servers in Active Directory. 

>[!NOTE]
>If you do not have a domain environment, do not run this command.

 `Add-DhcpServerInDC -DnsName DHCP1.corp.contoso.com -IPAddress 10.0.0.3`

To verify that the DHCP server is authorized in Active Directory, you can use the following command.

    Get-DhcpServerInDC

Following are example results that are displayed in Windows PowerShell.

    
        IPAddress   DnsName
        ---------   -------
        10.0.0.3    DHCP1.corp.contoso.com
    

For more information about these commands, see the following topics.

- [Add-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverindc)
- [Get-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/get-dhcpserverindc)

### <a name="notify-server-manager-that-post-install-dhcp-configuration-is-complete-optional"></a>Notify Server Manager that post\-install DHCP configuration is complete \(Optional\)

After you have completed post\-installation tasks, such as creating security groups and authorizing the DHCP server in Active Directory, Server Manager might still display an alert in the user interface stating that post\-installation steps must be completed by using the DHCP Post Installation Configuration wizard.

You can prevent this now\-unnecessary and inaccurate message from appearing in Server Manager by configuring the following registry key using this Windows PowerShell command.

    Set-ItemProperty –Path registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ServerManager\Roles\12 –Name ConfigurationState –Value 2

For more information about this command, see the following topic.

- [Set-ItemProperty](https://msdn.microsoft.com/powershell/reference/4.0/microsoft.powershell.management/set-itemproperty?f=255&MSPPError=-2147217396)

### <a name="set-server-level-dns-dynamic-update-configuration-settings-optional"></a>Set server level DNS dynamic update configuration settings \(Optional\)

If you want the DHCP server to perform DNS dynamic updates for DHCP client computers, you can run the following command to configure this setting. This is a server level setting, not a scope level setting, so it will affect all scopes that you configure on the server. This example command also configures the DHCP server to delete DNS resource records for clients when the client least expires.

    Set-DhcpServerv4DnsSetting -ComputerName "DHCP1.corp.contoso.com" -DynamicUpdates "Always" -DeleteDnsRRonLeaseExpiry $True

You can use the following command to configure the credentials that the DHCP server uses to register or unregister client records on a DNS server. This example saves a credential on a DHCP server. The first command uses **Get-Credential** to create a **PSCredential** object, and then stores the object in the **$Credential** variable. The command prompts you for user name and password, so ensure that you provide credentials for an account that has permission to update resource records on your DNS server.

    
    $Credential = Get-Credential
    Set-DhcpServerDnsCredential -Credential $Credential -ComputerName "DHCP1.corp.contoso.com"
    

For more information about these commands, see the following topics.

- [Set-DhcpServerv4DnsSetting](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverv4dnssetting)
- [Set-DhcpServerDnsCredential](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverdnscredential)

### <a name="configure-the-corpnet-scope"></a>Configure the Corpnet Scope

After DHCP installation is completed, you can use the following commands to configure and activate the Corpnet scope, create an exclusion range for the scope, and configure the DHCP options default gateway, DNS server IP address, and DNS domain name.

    
    Add-DhcpServerv4Scope -name "Corpnet" -StartRange 10.0.0.1 -EndRange 10.0.0.254 -SubnetMask 255.255.255.0 -State Active`
    
    Add-DhcpServerv4ExclusionRange -ScopeID 10.0.0.0 -StartRange 10.0.0.1 -EndRange 10.0.0.15`
    
    Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.0.1 -ScopeID 10.0.0.0 -ComputerName DHCP1.corp.contoso.com`
    
    Set-DhcpServerv4OptionValue -DnsDomain corp.contoso.com -DnsServer 10.0.0.2
    

For more information about these commands, see the following topics.

- [Add-DhcpServerv4Scope](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverv4scope)
- [Add-DhcpServerv4ExclusionRange](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverv4exclusionrange)
- [Set-DhcpServerv4OptionValue](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverv4optionvalue)

### <a name="configure-the-corpnet2-scope-optional"></a>Configure the Corpnet2 Scope \(Optional\)

If you have a second subnet that is connected to the first subnet with a router where DHCP forwarding is enabled, you can use the following commands to add a second scope, named Corpnet2 for this example. This example also configures an exclusion range and the IP address for the default gateway \(the router IP address on the subnet\) of the Corpnet2 subnet.

 `Add-DhcpServerv4Scope -name "Corpnet2" -StartRange 10.0.1.1 -EndRange 10.0.1.254 -SubnetMask 255.255.255.0 -State Active`

 `Add-DhcpServerv4ExclusionRange -ScopeID 10.0.1.0 -StartRange 10.0.1.1 -EndRange 10.0.1.15`

 `Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.1.1 -ScopeID 10.0.1.0 -ComputerName DHCP1.corp.contoso.com`

If you have additional subnets that are serviced by this DHCP server, you can repeat these commands, using different values for all of the command parameters, to add scopes for each subnet.

>[!IMPORTANT]
>Ensure that all routers between your DHCP clients and your DHCP server are configured for DHCP message forwarding. See your router documentation for information on how to configure DHCP forwarding.

## <a name="bkmk_verify"></a>Verify Server Functionality

To verify that your DHCP server is providing dynamic allocation of IP addresses to DHCP clients, you can connect another computer to a serviced subnet. After you connect the Ethernet cable to the network adapter and power on the computer, it will request an IP address from your DHCP server. You can verify successful configuration by using the **ipconfig /all** command and reviewing the results, or by performing connectivity tests, such as attempting to access Web resources with your browser or file shares with Windows Explorer or other applications.

If the client does not receive an IP address from your DHCP server, perform the following troubleshooting steps.

1. Ensure that the Ethernet cable is plugged into both the computer and the Ethernet switch, hub,  or router.
1. If you plugged the client computer into a network segment that is separated from the DHCP server by a router, ensure that the router is configured to forward DHCP messages.
1. Ensure that the DHCP server is authorized in Active Directory by running the following command to retrieve the list of authorized DHCP servers from Active Directory. [Get-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/get-dhcpserverindc).
1. Ensure that your scopes are activated by opening the DHCP console \(Server Manager, **Tools**, **DHCP**\), expanding the server tree to review scopes, then right\-clicking each scope. If the resulting menu includes the selection **Activate**, click **Activate**. \(If the scope is already activated, the menu selection reads **Deactivate**.\) 

## <a name="bkmk_dhcpwps"></a>Windows PowerShell Commands for DHCP

The following reference provides command descriptions and syntax for all DHCP Server Windows PowerShell commands for Windows Server 2016. The topic lists commands in alphabetical order based on the verb at the beginning of the commands, such as **Get** or **Set**.

>[!NOTE]
>You can not use Windows Server 2016 commands in Windows Server 2012 R2.

- [DhcpServer Module](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/index)

The following reference provides command descriptions and syntax for all DHCP Server Windows PowerShell commands for Windows Server 2012 R2. The topic lists commands in alphabetical order based on the verb at the beginning of the commands, such as **Get** or **Set**.

>[!NOTE]
>You can use Windows Server 2012 R2 commands in Windows Server 2016.

- [DHCP Server Cmdlets in Windows PowerShell](https://technet.microsoft.com/library/jj590751.aspx)

## <a name="bkmk_list"></a>List of Windows PowerShell Commands in this guide

Following is a simple list of commands and example values that are used in this guide.

    
    New-NetIPAddress -IPAddress 10.0.0.3 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24
    Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 10.0.0.2
    Rename-Computer -Name DHCP1
    Restart-Computer
    
    Add-Computer CORP
    Restart-Computer
    
    Install-WindowsFeature DHCP -IncludeManagementTools
    netsh dhcp add securitygroups
    Restart-service dhcpserver
    
    Add-DhcpServerInDC -DnsName DHCP1.corp.contoso.com -IPAddress 10.0.0.3
    Get-DhcpServerInDC
    
    Set-ItemProperty –Path registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ServerManager\Roles\12 –Name ConfigurationState –Value 2
    
    Set-DhcpServerv4DnsSetting -ComputerName "DHCP1.corp.contoso.com" -DynamicUpdates "Always" -DeleteDnsRRonLeaseExpiry $True
    
    $Credential = Get-Credential
    Set-DhcpServerDnsCredential -Credential $Credential -ComputerName "DHCP1.corp.contoso.com"
    
    rem At prompt, supply credential in form DOMAIN\user, password
    
    
    rem Configure scope Corpnet
    
    Add-DhcpServerv4Scope -name "Corpnet" -StartRange 10.0.0.1 -EndRange 10.0.0.254 -SubnetMask 255.255.255.0 -State Active
    
    Add-DhcpServerv4ExclusionRange -ScopeID 10.0.0.0 -StartRange 10.0.0.1 -EndRange 10.0.0.15
    
    Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.0.1 -ScopeID 10.0.0.0 -ComputerName DHCP1.corp.contoso.com
    
    Set-DhcpServerv4OptionValue -DnsDomain corp.contoso.com -DnsServer 10.0.0.2
    
    rem Configure scope Corpnet2
    
    Add-DhcpServerv4Scope -name "Corpnet2" -StartRange 10.0.1.1 -EndRange 10.0.1.254 -SubnetMask 255.255.255.0 -State Active
    
    Add-DhcpServerv4ExclusionRange -ScopeID 10.0.1.0 -StartRange 10.0.1.1 -EndRange 10.0.1.15
    
    Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.1.1 -ScopeID 10.0.1.0 -ComputerName DHCP1.corp.contoso.com
    


