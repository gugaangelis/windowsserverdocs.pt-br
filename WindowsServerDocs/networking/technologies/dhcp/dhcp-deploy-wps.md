---
title: Implantar o DHCP usando o Windows PowerShell
description: Você pode usar este tópico para implantar um servidor DHCP do Windows Server 2016 Internet Protocol (IP) versão 4 que fornece endereços IP automáticos e opções DHCP para clientes DHCP IPv4 conectados a uma ou mais sub-redes em sua rede.
ms.prod: windows-server
ms.technology: networking-dhcp
ms.topic: article
ms.assetid: 7110ad21-a33e-48d5-bb3c-129982913bc8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1e750a72ac8d47f99ea3382d076b8854acc24576
ms.sourcegitcommit: 3f9bcd188dda12dc5803defb47b2c3a907504255
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/04/2020
ms.locfileid: "77001891"
---
# <a name="deploy-dhcp-using-windows-powershell"></a>Implantar o DHCP usando o Windows PowerShell

> Aplicável a: Windows Server (canal semestral), Windows Server 2016

Este guia fornece instruções sobre como usar o Windows PowerShell para implantar um protocolo de configuração de host dinâmico do protocolo IP versão 4 \(DHCP\) servidor que atribui automaticamente endereços IP e opções DHCP a clientes DHCP IPv4 que estão conectados a uma ou mais sub-redes em sua rede.

> [!NOTE]
> Para baixar este documento no formato do Word na galeria do TechNet, consulte [implantar o DHCP usando o Windows PowerShell no Windows Server 2016](https://gallery.technet.microsoft.com/Deploy-DHCP-Using-Windows-246dd293).

O uso de servidores DHCP para atribuir endereços IP economiza em sobrecarga administrativa porque você não precisa configurar manualmente as configurações de TCP/IP V4 para cada adaptador de rede em cada computador na rede. Com DHCP, a configuração de TCP/IP V4 é executada automaticamente quando um computador ou outro cliente DHCP está conectado à sua rede.

Você pode implantar o servidor DHCP em um grupo de trabalho como um servidor autônomo ou como parte de um domínio de Active Directory.

Este guia contém as seções a seguir.

- [Visão geral da implantação de DHCP](#bkmk_overview)
- [Visões gerais de tecnologia](#bkmk_technologies)
- [Planejar a implantação do DHCP](#bkmk_plan)
- [Usando este guia em um laboratório de teste](#bkmk_lab)
- [Implantar o DHCP](#bkmk_deploy)
- [Verificar a funcionalidade do servidor](#bkmk_verify)
- [Comandos do Windows PowerShell para DHCP](#bkmk_dhcpwps)
- [Lista de comandos do Windows PowerShell neste guia](#bkmk_list)

## <a name="bkmk_overview"></a>Visão geral da implantação de DHCP

A ilustração a seguir descreve o cenário que você pode implantar usando este guia. O cenário inclui um servidor DHCP em um domínio Active Directory. O servidor está configurado para fornecer endereços IP a clientes DHCP em duas sub-redes diferentes. As sub-redes são separadas por um roteador que tem o encaminhamento de DHCP habilitado.

![Visão geral da topologia de rede do DHCP](../../media/Core-Network-Guide/cng16_overview.jpg)

## <a name="bkmk_technologies"></a>Visões gerais de tecnologia

As seções a seguir fornecem breves visões gerais de DHCP e TCP/IP.

### <a name="dhcp-overview"></a>Visão geral do DHCP

O DHCP é um padrão de IP para simplificar o gerenciamento da configuração de IP do host. O padrão DHCP proporciona o uso de servidores DHCP como uma forma de gerenciar a alocação dinâmica de endereços IP e outros detalhes de configuração relacionados para clientes habilitados para DHCP em sua rede.

O DHCP permite que você use um servidor DHCP para atribuir dinamicamente um endereço IP a um computador ou outro dispositivo, como uma impressora, em sua rede local, em vez de configurar manualmente cada dispositivo com um endereço IP estático.

Todos os computadores em uma rede TCP/IP devem ter um endereço IP exclusivo, porque o endereço IP e a máscara de sub-rede relacionada identificam o computador host e a sub-rede à qual o computador está conectado. Usando o DHCP, você pode garantir que todos os computadores configurados como clientes DHCP recebam um endereço IP que seja apropriado para seu local de rede e sua sub-rede e, usando as opções de DHCP (como o gateway padrão e os servidores DNS), você pode fornecer automaticamente aos clientes DHCP as informações que eles precisam para funcionar corretamente na rede.

Para redes baseadas em TCP/IP, o DHCP reduz a complexidade e a quantidade de trabalho administrativo envolvidos na configuração de computadores.

### <a name="tcpip-overview"></a>Visão geral de TCP/IP

Por padrão, todas as versões dos sistemas operacionais Windows Server e Windows Client têm configurações de TCP/IP para conexões de rede IP versão 4 configuradas para obter automaticamente um endereço IP e outras informações, chamadas de opções DHCP, de um servidor DHCP. Por isso, você não precisa definir as configurações de TCP/IP manualmente, a menos que o computador seja um computador servidor ou outro dispositivo que exija um endereço IP estático configurado manualmente. 

Por exemplo, é recomendável que você configure manualmente o endereço IP do servidor DHCP e os endereços IP dos servidores DNS e controladores de domínio que estão executando Active Directory Domain Services \(AD DS\).

O TCP/IP no Windows Server 2016 é o seguinte:

- Software de rede baseado em protocolos de rede padrão do setor.

- Um protocolo de rede de empresa roteável que suporta a conexão de seu computador baseado em Windows com os ambientes LAN (rede de área local) e WAN (rede de longa distância).

- As principais tecnologias e utilitários para conectar seu computador baseado em Windows com os sistemas diferentes com a finalidade de compartilhar informações.

- Uma base para obter acesso aos serviços globais da Internet, como servidores Web e protocolo FTP (FTP).

- Uma estrutura robusta, escalável, multiplataforma, de cliente/servidor.

O TCP/IP oferece utilitários TCP/IP básicos que permitem que computadores baseados no Windows se conectem e compartilhem informações com outros sistemas Microsoft e não Microsoft, incluindo:

- Windows Server 2016

- Windows 10

- R2 do Windows Server 2012

- Windows 8.1

- Windows Server 2012

- O Windows 8

- Windows Server 2008 R2

- Windows 7

- Windows Server 2008

- Windows Vista

- Hosts da Internet

- Sistemas Apple Macintosh

- Mainframes IBM

- Sistemas UNIX e Linux

- Sistemas VMS abertos

- Impressoras prontas para a rede

- Tablets e telefones celulares com Ethernet com fio ou tecnologia sem fio 802,11 habilitada

## <a name="bkmk_plan"></a>Planejar a implantação do DHCP

A seguir estão as principais etapas de planejamento antes de instalar a função de servidor DHCP.

### <a name="planning-dhcp-servers-and-dhcp-forwarding"></a>Planejando os servidores DHCP e o encaminhamento DHCP

Como as mensagens DHCP são mensagens de difusão, elas não são encaminhadas entre as sub-redes por roteadores. Se você tiver várias sub-redes e desejar fornecer o serviço DHCP em cada sub-rede, deverá fazer o seguinte:

- Instalar um servidor DHCP em cada sub-rede

- Configurar roteadores para encaminhar as mensagens de difusão DHCP entre sub-redes e configurar vários escopos no servidor DHCP, um escopo por sub-rede.

Na maioria dos casos, a configuração de roteadores para encaminhar mensagens de difusão DHCP é mais econômica que a implantação de um servidor DHCP em cada segmento físico da rede.

### <a name="planning-ip-address-ranges"></a>Planejando intervalos de endereços IP

Cada sub-rede deve ter seu próprio intervalo de endereços IP exclusivo. Esses intervalos são representados em um servidor DHCP com escopos.

Um escopo é um agrupamento administrativo de endereços IP para computadores em uma sub-rede que usa o serviço DHCP. O administrador primeiro cria um escopo para cada sub-rede física e, em seguida, usa o escopo para definir os parâmetros usados pelos clientes.

Um escopo tem as seguintes propriedades:

- Um intervalo de endereços IP do qual incluir ou excluir endereços usados para ofertas de concessão de serviço DHCP.

- Uma máscara de sub-rede, que determina o prefixo de sub-rede para um determinado endereço IP.

- Um nome de escopo atribuído quando é criado.

- Valores de duração da concessão, que são atribuídos aos clientes DHCP que recebem endereços IP alocados dinamicamente.

- Qualquer opção de escopo DHCP configurada para atribuição a clientes DHCP, como endereço ID do servidor DNS ou endereço IP do gateway do roteador/padrão.

- As reservas são opcionalmente usadas para garantir que um cliente DHCP sempre receba o mesmo endereço IP.

Antes de implantar os servidores, liste suas sub-redes e o intervalo de endereços IP que você deseja usar para cada sub-rede.

### <a name="planning-subnet-masks"></a>Planejando máscaras de sub-rede

As IDs de rede e IDs de host são diferenciadas usando uma máscara de sub-rede. Cada máscara de sub-rede é um número de 32 bits que usa grupos de bits consecutivos de todos (1) para identificar a ID da rede e todos os zeros (0) para identificar as partes de ID do host de um endereço IP.

Por exemplo, a máscara de sub-rede normalmente usada com o endereço IP 131.107.16.200 é o seguinte número binário de 32 bits:

```
11111111 11111111 00000000 00000000
```

Esse número de máscara de sub-rede é 16 1 bits seguido de 16 bits, indicando que a ID de rede e as seções de ID de host desse endereço IP têm 16 bits de comprimento. Normalmente, essa máscara de sub-rede é exibida em notação decimal com ponto como 255.255.0.0.

A tabela a seguir exibe as máscaras de sub-rede para as classes de endereço de Internet.

|Classe de endereço|Bits para máscara de sub-rede|Máscara de sub-rede|
|-----------------|------------------------|---------------|
|Classe A|11111111 00000000 00000000 00000000|255.0.0.0|
|Classe B|11111111 11111111 00000000 00000000|255.255.0.0|
|Classe C|11111111 11111111 11111111 00000000|255.255.255.0|

Quando você cria um escopo no DHCP e insere o intervalo de endereços IP do escopo, o DHCP fornece esses valores de máscara de sub-rede padrão. Normalmente, os valores de máscara de sub-rede padrão são aceitáveis para a maioria das redes sem requisitos especiais e onde cada segmento de rede IP corresponde a uma única rede física.

Em alguns casos, você pode usar máscaras de sub-rede personalizadas para implementar a criação de sub-redes IP. Com a criação de sub-redes IP, você pode subdividir a parte padrão de ID de host de um endereço IP para especificar sub-redes, que são subdivisões de ID de rede baseada na classe original.

Personalizando o comprimento da máscara de sub-rede, você pode reduzir o número de bits que são usados para o ID de host real.

Para evitar a solução e o roteamento de problemas, verifique se todos os computadores TCP/IP em um segmento de rede usam a mesma máscara de sub-rede e se cada computador ou dispositivo tem um endereço IP exclusivo.

### <a name="planning-exclusion-ranges"></a>Planejando intervalos de exclusão

Quando você cria um escopo em um servidor DHCP, pode especificar um intervalo de endereços IP que inclui todos os endereços IP que o servidor DHCP pode conceder aos clientes DHCP, como computadores e outros dispositivos. Se você acessar e configurar manualmente alguns servidores e outros dispositivos com endereços IP estáticos do mesmo intervalo de endereços IP usado pelo servidor DHCP, poderá criar acidentalmente um conflito de endereços IP, onde você e o servidor DHCP atribuem o mesmo endereço IP a dispositivos diferentes.

Para resolver esse problema, você pode criar um intervalo de exclusão para o escopo DHCP. Um intervalo de exclusão é um intervalo contíguo de endereços IP dentro do intervalo de endereços IP do escopo que o servidor DHCP não tem permissão para usar. Se você criar um intervalo de exclusão, o servidor DHCP não atribuirá os endereços nesse intervalo, permitindo que você atribua manualmente esses endereços sem criar um conflito de endereços IP.

Você pode excluir endereços IP da distribuição pelo servidor DHCP, criando um intervalo de exclusão para cada escopo. Use as exclusões para todos os dispositivos configurados com um endereço IP estático. Os endereços excluídos devem incluir todos os endereços IP que você atribuiu manualmente a outros servidores, clientes não-DHCP, estações de trabalho sem disco ou clientes de Roteamento, Acesso Remoto e PPP.

Recomendamos que você configure o intervalo de exclusão com endereços extras para acomodar o crescimento futuro da rede. A tabela a seguir fornece um intervalo de exclusão de exemplo para um escopo com um intervalo de endereços IP de 10.0.0.1-10.0.0.254 e uma máscara de sub-rede de 255.255.255.0.

|Itens de configuração|Valores de exemplo|
|-----------------------|------------------|
|Endereço IP Inicial do intervalo de exclusão|10.0.0.1|
|Endereço IP Final do intervalo de exclusão|endereços 10.0.0.25|

### <a name="planning-tcpip-static-configuration"></a>Planejando a configuração estática do TCP/IP

Certos dispositivos, como roteadores, servidores DHCP e servidores DNS, devem ser configurados com um endereço IP estático. Além disso, você pode ter dispositivos adicionais, como impressoras, onde deve garantir sempre o mesmo endereço IP. Liste os dispositivos que você deseja configurar estaticamente para cada sub-rede e planeje o intervalo de exclusão que deseja usar no servidor DHCP para garantir que o servidor DHCP não conceda o endereço IP de um dispositivo configurado estaticamente. Um intervalo de exclusão é uma sequência limitada de endereços IP dentro de um escopo, excluído das ofertas de serviço DHCP. Os intervalos de exclusão garantem que os endereços nesses intervalos não sejam oferecidos pelo servidor a clientes DHCP na sua rede.

Por exemplo, se o intervalo de endereços IP de uma sub-rede for 192.168.0.1 a 192.168.0.254 e você tiver dez dispositivos que deseja configurar com um endereço IP estático, poderá criar um intervalo de exclusão para o escopo 192.168.0.*x* que inclui dez ou mais endereços IP: 192.168.0.1 a 192.168.0.15.

Neste exemplo, você usa dez dos endereços IP excluídos para configurar servidores e outros dispositivos com endereços IP estáticos e cinco endereços IP adicionais ficam disponíveis para uma configuração estática de novos dispositivos que você possa desejar adicionar no futuro. Com este intervalo de exclusão, o servidor DHCP fica com um pool de endereços de 192.168.0.16 até 192.168.0.254.

Itens de configuração de exemplo adicionais para AD DS e DNS são fornecidos na tabela a seguir.

|Itens de configuração|Valores de exemplo|
|-----------------------|------------------|
|Ligações de Conexão de Rede|Ethernet|
|Configurações do servidor DNS|DC1.corp.contoso.com|
|Endereço IP do servidor DNS preferencial|10.0.0.2|
|Valores de escopo<br /><br />1. nome do escopo<br />2. endereço IP inicial<br />3. endereço IP final<br />4. máscara de sub-rede<br />5. gateway padrão (opcional)<br />6. duração da concessão|1. sub-rede primária<br />2.10.0.0.1<br />3.10.0.0.254<br />4.255.255.255.0<br />5.10.0.0.1<br />6.8 dias|
|Modo de Operação do Servidor DHCP IPv6|Não habilitado|

## <a name="bkmk_lab"></a>Usando este guia em um laboratório de teste

Você pode usar este guia para implantar o DHCP em um laboratório de teste antes de implantá-lo em um ambiente de produção. 

> [!NOTE]
> Se você não quiser implantar o DHCP em um laboratório de teste, poderá pular para a seção [implantar o DHCP](#bkmk_deploy).

Os requisitos para seu laboratório diferem dependendo se você estiver usando servidores físicos ou máquinas virtuais \(VMs\)e se estiver usando um domínio de Active Directory ou implantando um servidor DHCP autônomo.

Você pode usar as informações a seguir para determinar os recursos mínimos necessários para testar a implantação do DHCP usando este guia.

### <a name="test-lab-requirements-with-vms"></a>Requisitos de laboratório de teste com VMs

Para implantar o DHCP em um laboratório de teste com VMs, você precisa dos seguintes recursos.

Para a implantação de domínio ou a implantação autônoma, você precisa de um servidor configurado como um host Hyper\-V.

**Implantação de domínio**

Essa implantação requer um servidor físico, um comutador virtual, dois servidores virtuais e um cliente virtual:

No servidor físico, no Gerenciador do Hyper-V, crie os itens a seguir.

1. Um comutador virtual **interno** . Não crie um comutador virtual **externo** , porque se o host do Hyper\-V estiver em uma sub-rede que inclui um servidor DHCP, suas VMs de teste receberão um endereço IP do servidor DHCP. Além disso, o servidor DHCP de teste que você implanta pode atribuir endereços IP a outros computadores na sub-rede em que o host Hyper\-V está instalado.
1. Uma VM que executa o Windows Server 2016 configurado como um controlador de domínio com Active Directory Domain Services que está conectado ao comutador virtual interno que você criou. Para corresponder a este guia, esse servidor deve ter um endereço IP de 10.0.0.2 configurado estaticamente. Para obter informações sobre como implantar AD DS, consulte a seção **implantando DC1** no guia de [rede](https://docs.microsoft.com/windows-server/networking/core-network-guide/core-network-guide#BKMK_deployADDNS01)do Windows Server 2016 Core.
1. Uma VM que executa o Windows Server 2016 que será configurada como um servidor DHCP usando este guia e que está conectada ao comutador virtual interno que você criou. 
1. Uma VM que executa um sistema operacional cliente Windows que está conectado ao comutador virtual interno que você criou e que será usado para verificar se o servidor DHCP está alocando dinamicamente endereços IP e opções DHCP para clientes DHCP.

**Implantação de servidor DHCP autônomo**

Essa implantação requer um servidor físico, um comutador virtual, um servidor virtual e um cliente virtual:

No servidor físico, no Gerenciador do Hyper-V, crie os itens a seguir.

1. Um comutador virtual **interno** . Não crie um comutador virtual **externo** , porque se o host do Hyper\-V estiver em uma sub-rede que inclui um servidor DHCP, suas VMs de teste receberão um endereço IP do servidor DHCP. Além disso, o servidor DHCP de teste que você implanta pode atribuir endereços IP a outros computadores na sub-rede em que o host Hyper\-V está instalado.
2. Uma VM que executa o Windows Server 2016 que será configurada como um servidor DHCP usando este guia e que está conectada ao comutador virtual interno que você criou.
3. Uma VM que executa um sistema operacional cliente Windows que está conectado ao comutador virtual interno que você criou e que será usado para verificar se o servidor DHCP está alocando dinamicamente endereços IP e opções DHCP para clientes DHCP.

### <a name="test-lab-requirements-with-physical-servers"></a>Requisitos de laboratório de teste com servidores físicos

Para implantar o DHCP em um laboratório de teste com servidores físicos, você precisa dos seguintes recursos.

**Implantação de domínio**

Essa implantação requer um Hub ou comutador, dois servidores físicos e um cliente físico:

1. Um hub Ethernet ou um comutador ao qual você pode conectar os computadores físicos com cabos Ethernet
2. Um computador físico executando o Windows Server 2016 configurado como um controlador de domínio com Active Directory Domain Services. Para corresponder a este guia, esse servidor deve ter um endereço IP de 10.0.0.2 configurado estaticamente. Para obter informações sobre como implantar AD DS, consulte a seção **implantando DC1** no guia de [rede](https://docs.microsoft.com/windows-server/networking/core-network-guide/core-network-guide#BKMK_deployADDNS01)do Windows Server 2016 Core.
3. Um computador físico executando o Windows Server 2016 que será configurado como um servidor DHCP usando este guia.
4. Um computador físico executando um sistema operacional cliente Windows que será usado para verificar se o servidor DHCP está alocando dinamicamente endereços IP e opções DHCP para clientes DHCP.

> [!NOTE]
> Se você não tiver computadores de teste suficientes para essa implantação, poderá usar um computador de teste para AD DS e DHCP, mas essa configuração não é recomendada para um ambiente de produção.

**Implantação de servidor DHCP autônomo**

Essa implantação requer um Hub ou comutador, um servidor físico e um cliente físico:

1. Um hub Ethernet ou um comutador ao qual você pode conectar os computadores físicos com cabos Ethernet
2. Um computador físico executando o Windows Server 2016 que será configurado como um servidor DHCP usando este guia.
3. Um computador físico executando um sistema operacional cliente Windows que será usado para verificar se o servidor DHCP está alocando dinamicamente endereços IP e opções DHCP para clientes DHCP.


## <a name="bkmk_deploy"></a>Implantar o DHCP

Esta seção fornece exemplos de comandos do Windows PowerShell que você pode usar para implantar o DHCP em um servidor. Antes de executar esses comandos de exemplo em seu servidor, você deve modificar os comandos para corresponder à sua rede e ao seu ambiente. 

Por exemplo, antes de executar os comandos, você deve substituir os valores de exemplo nos comandos para os seguintes itens:

- Nomes de computador
- Intervalo de endereços IP para cada escopo que você deseja configurar (1 escopo por sub-rede)
- Máscara de sub-rede para cada intervalo de endereços IP que você deseja configurar
- Nome do escopo para cada escopo
- Intervalo de exclusão para cada escopo
- Valores de opção DHCP, como gateway padrão, nome de domínio e servidores DNS ou WINS
- Nomes de interface

> [!IMPORTANT]
> Examine e modifique cada comando para seu ambiente antes de executar o comando.

### <a name="where-to-install-dhcp---on-a-physical-computer-or-a-vm"></a>Onde instalar o DHCP-em um computador físico ou uma VM?

Você pode instalar a função de servidor DHCP em um computador físico ou em uma máquina virtual \(\) de VM instalado em um host Hyper\-V. Se você estiver instalando o DHCP em uma VM e quiser que o servidor DHCP forneça atribuições de endereço IP a computadores na rede física à qual o host do Hyper-V está conectado, você deve conectar o adaptador de rede virtual da VM a um comutador virtual do Hyper-V **externo**.

Para obter mais informações, consulte a seção **criar um comutador virtual com o Gerenciador do Hyper-V** no tópico [criar uma rede virtual](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/connect-to-network).

### <a name="run-windows-powershell-as-an-administrator"></a>Executar o Windows PowerShell como administrador

Você pode usar o procedimento a seguir para executar o Windows PowerShell com privilégios de administrador.

1. Em um computador que executa o Windows Server 2016, clique em **Iniciar**e clique com o botão direito do mouse no ícone do Windows PowerShell. Um menu é exibido.

2. No menu, clique em **mais**e, em seguida, clique em **Executar como administrador**. Se solicitado, digite as credenciais para uma conta que tenha privilégios de administrador no computador. Se a conta de usuário com a qual você fez logon no computador for uma conta de nível de administrador, você não receberá uma solicitação de credencial.

3. O Windows PowerShell é aberto com privilégios de administrador.

### <a name="rename-the-dhcp-server-and-configure-a-static-ip-address"></a>Renomear o servidor DHCP e configurar um endereço IP estático

Se ainda não tiver feito isso, você poderá usar os seguintes comandos do Windows PowerShell para renomear o servidor DHCP e configurar um endereço IP estático para o servidor.

**Configurar um endereço IP estático**

Você pode usar os comandos a seguir para atribuir um endereço IP estático ao servidor DHCP e configurar as propriedades TCP/IP do servidor DHCP com o endereço IP correto do servidor DNS. Você também deve substituir os nomes de interface e os endereços IP neste exemplo pelos valores que deseja usar para configurar o seu computador.

```
New-NetIPAddress -IPAddress 10.0.0.3 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 10.0.0.2
```

Para obter mais informações sobre esses comandos, consulte os tópicos a seguir.

- [Novo-netipaddress](https://docs.microsoft.com/powershell/module/nettcpip/New-NetIPAddress)
- [Set-DnsClientServerAddress](https://docs.microsoft.com/powershell/module/dnsclient/Set-DnsClientServerAddress)

**Renomear o computador**

Você pode usar os comandos a seguir para renomear e reiniciar o computador.

```
Rename-Computer -Name DHCP1
Restart-Computer
```

Para obter mais informações sobre esses comandos, consulte os tópicos a seguir.

- [Renomear-computador](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/rename-computer)
- [Restart-Computer](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/restart-computer)

### <a name="join-the-computer-to-the-domain-optional"></a>Ingresse o computador no domínio \(\) opcional

Se você estiver instalando o servidor DHCP em um ambiente de domínio Active Directory, deverá ingressar o computador no domínio. Abra o Windows PowerShell com privilégios de administrador e execute o comando a seguir depois de substituir o nome NetBios do domínio **Corp** por um valor apropriado para o seu ambiente.

```
Add-Computer CORP
```

Quando solicitado, digite as credenciais para uma conta de usuário de domínio que tenha permissão para ingressar um computador no domínio. 

```
Restart-Computer
```

Para obter mais informações sobre o comando Add-Computer, consulte o tópico a seguir.

- [Adicionar computador](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/add-computer?view=powershell-5.1)

### <a name="install-dhcp"></a>Instalar o DHCP

Depois que o computador for reiniciado, abra o Windows PowerShell com privilégios de administrador e, em seguida, instale o DHCP executando o comando a seguir.

```
Install-WindowsFeature DHCP -IncludeManagementTools
```

Para obter mais informações sobre esse comando, consulte o tópico a seguir.

- [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/servermanager/install-windowsfeature)

### <a name="create-dhcp-security-groups"></a>Criar grupos de segurança do DHCP

Para criar grupos de segurança, você deve executar um shell de rede \(comando netsh\) no Windows PowerShell e reiniciar o serviço DHCP para que os novos grupos se tornem ativos.

Quando você executa o comando netsh a seguir no servidor DHCP, os grupos de segurança **Administradores DHCP** e **usuários DHCP** são criados em **usuários e grupos locais** no servidor DHCP.

```
netsh dhcp add securitygroups
```

O comando a seguir reinicia o serviço DHCP no computador local.

```
Restart-Service dhcpserver
```

Para obter mais informações sobre esses comandos, consulte os tópicos a seguir.

- [Shell de Rede (Netsh)](../netsh/netsh.md)
- [Restart-Service](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/restart-service)

### <a name="authorize-the-dhcp-server-in-active-directory-optional"></a>Autorizar o servidor DHCP em Active Directory \(opcional\)

Se você estiver instalando o DHCP em um ambiente de domínio, deverá executar as etapas a seguir para autorizar o servidor DHCP a operar no domínio.

> [!NOTE]
> Os servidores DHCP não autorizados instalados no Active Directory domínios não podem funcionar corretamente e não concedem endereços IP a clientes DHCP. A desabilitação automática de servidores DHCP não autorizados é um recurso de segurança que impede que servidores DHCP não autorizados atribuam endereços IP incorretos a clientes na rede.

Você pode usar o comando a seguir para adicionar o servidor DHCP à lista de servidores DHCP autorizados no Active Directory. 

> [!NOTE]
> Se você não tiver um ambiente de domínio, não execute este comando.

```
Add-DhcpServerInDC -DnsName DHCP1.corp.contoso.com -IPAddress 10.0.0.3
```

Para verificar se o servidor DHCP está autorizado no Active Directory, você pode usar o comando a seguir.

```
Get-DhcpServerInDC
```

Veja a seguir os resultados de exemplo que são exibidos no Windows PowerShell.

```
IPAddress   DnsName
---------   -------
10.0.0.3    DHCP1.corp.contoso.com
```

Para obter mais informações sobre esses comandos, consulte os tópicos a seguir.

- [Add-DhcpServerInDC](https://docs.microsoft.com/powershell/module/dhcpserver/add-dhcpserverindc)
- [Get-DhcpServerInDC](https://docs.microsoft.com/powershell/module/dhcpserver/get-dhcpserverindc)

### <a name="notify-server-manager-that-post-install-dhcp-configuration-is-complete-optional"></a>Notifique Gerenciador do Servidor que o post\-instalar a configuração do DHCP está completo \(opcional\)

Depois de concluir o post\-tarefas de instalação, como criar grupos de segurança e autorizar o servidor DHCP no Active Directory, Gerenciador do Servidor ainda poderá exibir um alerta na interface do usuário informando que as etapas de instalação do post\-devem ser concluídas usando o assistente de configuração de pós-instalação do DHCP.

Você pode impedir que agora\-mensagem desnecessária e imprecisa apareça em Gerenciador do Servidor Configurando a seguinte chave do registro usando este comando do Windows PowerShell.

```
Set-ItemProperty –Path registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ServerManager\Roles\12 –Name ConfigurationState –Value 2
```

Para obter mais informações sobre esse comando, consulte o tópico a seguir.

- [Set-ItemProperty](https://docs.microsoft.com/powershell/module/microsoft.powershell.management/set-itemproperty)

### <a name="set-server-level-dns-dynamic-update-configuration-settings-optional"></a>Definir definições de configuração de atualização dinâmica de DNS no nível de servidor \(opcional\)

Se desejar que o servidor DHCP execute atualizações dinâmicas de DNS para computadores cliente DHCP, você poderá executar o comando a seguir para definir essa configuração. Essa é uma configuração de nível de servidor, não uma configuração de nível de escopo, portanto, afetará todos os escopos configurados no servidor. Esse comando de exemplo também configura o servidor DHCP para excluir registros de recursos de DNS para clientes quando o cliente do não expira.

```
Set-DhcpServerv4DnsSetting -ComputerName "DHCP1.corp.contoso.com" -DynamicUpdates "Always" -DeleteDnsRRonLeaseExpiry $True
```

Você pode usar o comando a seguir para configurar as credenciais que o servidor DHCP usa para registrar ou cancelar o registro de registros de cliente em um servidor DNS. Este exemplo salva uma credencial em um servidor DHCP. O primeiro comando usa **Get-Credential** para criar um objeto **PSCredential** e, em seguida, armazena o objeto na variável **$Credential** . O comando solicita o nome de usuário e a senha, portanto, certifique-se de fornecer credenciais para uma conta que tenha permissão para atualizar os registros de recursos no servidor DNS.
 
```
$Credential = Get-Credential
Set-DhcpServerDnsCredential -Credential $Credential -ComputerName "DHCP1.corp.contoso.com"
``` 

Para obter mais informações sobre esses comandos, consulte os tópicos a seguir.

- [Set-DhcpServerv4DnsSetting](https://docs.microsoft.com/en-us/powershell/module/dhcpserver/set-dhcpserverv4dnssetting)
- [Set-DhcpServerDnsCredential](https://docs.microsoft.com/en-us/powershell/module/dhcpserver/set-dhcpserverdnscredential)

### <a name="configure-the-corpnet-scope"></a>Configurar o escopo corpnet

Após a conclusão da instalação do DHCP, você pode usar os seguintes comandos para configurar e ativar o escopo corpnet, criar um intervalo de exclusão para o escopo e configurar o gateway padrão de opções DHCP, o endereço IP do servidor DNS e o nome de domínio DNS.

```
Add-DhcpServerv4Scope -name "Corpnet" -StartRange 10.0.0.1 -EndRange 10.0.0.254 -SubnetMask 255.255.255.0 -State Active    
Add-DhcpServerv4ExclusionRange -ScopeID 10.0.0.0 -StartRange 10.0.0.1 -EndRange 10.0.0.15
Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.0.1 -ScopeID 10.0.0.0 -ComputerName DHCP1.corp.contoso.com
Set-DhcpServerv4OptionValue -DnsDomain corp.contoso.com -DnsServer 10.0.0.2
```

Para obter mais informações sobre esses comandos, consulte os tópicos a seguir.

- [Add-DhcpServerv4Scope](https://docs.microsoft.com/powershell/module/dhcpserver/Add-DhcpServerv4Scope)
- [Add-DhcpServerv4ExclusionRange](https://docs.microsoft.com/powershell/module/dhcpserver/Add-DhcpServerv4ExclusionRange)
- [Set-DhcpServerv4OptionValue](https://docs.microsoft.com/powershell/module/dhcpserver/Set-DhcpServerv4OptionValue)

### <a name="configure-the-corpnet2-scope-optional"></a>Configurar o escopo Corpnet2 \(opcional\)

Se você tiver uma segunda sub-rede conectada à primeira sub-rede com um roteador em que o encaminhamento DHCP está habilitado, você poderá usar os comandos a seguir para adicionar um segundo escopo, chamado Corpnet2, para este exemplo. Este exemplo também configura um intervalo de exclusão e o endereço IP para o gateway padrão \(o endereço IP do roteador na sub-rede\) da sub-rede Corpnet2.

```
Add-DhcpServerv4Scope -name "Corpnet2" -StartRange 10.0.1.1 -EndRange 10.0.1.254 -SubnetMask 255.255.255.0 -State Active
Add-DhcpServerv4ExclusionRange -ScopeID 10.0.1.0 -StartRange 10.0.1.1 -EndRange 10.0.1.15
Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.1.1 -ScopeID 10.0.1.0 -ComputerName DHCP1.corp.contoso.com
```

Se você tiver sub-redes adicionais que são atendidas por esse servidor DHCP, poderá repetir esses comandos, usando valores diferentes para todos os parâmetros de comando, para adicionar escopos para cada sub-rede.

> [!IMPORTANT]
> Verifique se todos os roteadores entre os clientes DHCP e o servidor DHCP estão configurados para o encaminhamento de mensagens DHCP. Consulte a documentação do roteador para obter informações sobre como configurar o encaminhamento de DHCP.

## <a name="bkmk_verify"></a>Verificar a funcionalidade do servidor

Para verificar se o servidor DHCP está fornecendo a alocação dinâmica de endereços IP para clientes DHCP, você pode conectar outro computador a uma sub-rede atendida. Depois de conectar o cabo Ethernet ao adaptador de rede e ligar o computador, ele solicitará um endereço IP do servidor DHCP. Você pode verificar a configuração bem-sucedida usando o comando **ipconfig/all** e revisar os resultados, ou executando testes de conectividade, como tentar acessar recursos da Web com o navegador ou compartilhamentos de arquivos com o Windows Explorer ou outros aplicativos.

Se o cliente não receber um endereço IP do servidor DHCP, execute as seguintes etapas de solução de problemas.

1. Verifique se o cabo Ethernet está conectado ao computador e ao comutador Ethernet, Hub ou roteador.
2. Se você conectou o computador cliente em um segmento de rede separado do servidor DHCP por um roteador, verifique se o roteador está configurado para encaminhar mensagens DHCP.
3. Verifique se o servidor DHCP está autorizado no Active Directory executando o comando a seguir para recuperar a lista de servidores DHCP autorizados do Active Directory. [Get-DhcpServerInDC](https://docs.microsoft.com/powershell/module/dhcpserver/Get-DhcpServerInDC).
4. Verifique se os escopos estão ativados abrindo o console do DHCP \(Gerenciador do Servidor, **ferramentas**,\)**DHCP** , expandindo a árvore do servidor para examinar escopos e, em seguida,\-clicando em cada escopo. Se o menu resultante incluir a seleção **Ativar**, clique em **Ativar**. \(se o escopo já estiver ativado, a seleção de menu exibirá **desativar**.\)

## <a name="bkmk_dhcpwps"></a>Comandos do Windows PowerShell para DHCP

A referência a seguir fornece descrições de comando e sintaxe para todos os comandos do Windows PowerShell do servidor DHCP para o Windows Server 2016. O tópico lista os comandos em ordem alfabética com base no verbo no início dos comandos, como **Get** ou **set**.

> [!NOTE]
> Você não pode usar os comandos do Windows Server 2016 no Windows Server 2012 R2.

- [Módulo DhcpServer](https://docs.microsoft.com/en-us/powershell/module/dhcpserver/)

A referência a seguir fornece descrições de comando e sintaxe para todos os comandos do Windows PowerShell do servidor DHCP para o Windows Server 2012 R2. O tópico lista os comandos em ordem alfabética com base no verbo no início dos comandos, como **Get** ou **set**.

> [!NOTE]
> Você pode usar os comandos do Windows Server 2012 R2 no Windows Server 2016.

- [Cmdlets do servidor DHCP no Windows PowerShell](https://docs.microsoft.com/windows-server/networking/technologies/dhcp/dhcp-deploy-wps)

## <a name="bkmk_list"></a>Lista de comandos do Windows PowerShell neste guia

A seguir está uma lista simples de comandos e valores de exemplo que são usados neste guia.

```
New-NetIPAddress -IPAddress 10.0.0.3 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24
Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 10.0.0.2
Rename-Computer -Name DHCP1
Restart-Computer

Add-Computer CORP
Restart-Computer

Install-WindowsFeature DHCP -IncludeManagementTools
netsh dhcp add securitygroups
Restart-Service dhcpserver

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
```
