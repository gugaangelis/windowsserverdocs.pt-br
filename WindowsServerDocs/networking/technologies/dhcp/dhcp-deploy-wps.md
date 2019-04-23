---
title: Implantar o DHCP usando o Windows PowerShell
description: Você pode usar este tópico para implantar um servidor DHCP do Windows Server 2016 IP (Internet Protocol) versão 4 que fornece endereços IP automáticos e opções de DHCP a clientes DHCP IPv4 conectados a uma ou mais sub-redes na sua rede.
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: article
ms.assetid: 7110ad21-a33e-48d5-bb3c-129982913bc8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 8a53f6293067fa0f7014e7794696cf75f7179545
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849087"
---
# <a name="deploy-dhcp-using-windows-powershell"></a>Implantar o DHCP usando o Windows PowerShell

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este guia fornece instruções sobre como usar o Windows PowerShell para implantar um IP (Internet Protocol) versão 4 Dynamic Host Configuration Protocol \(DHCP\) de opções de servidor que atribui automaticamente endereços IP e DHCP para DHCP IPv4 clientes que estão conectados a um ou mais sub-redes na sua rede.

>[!NOTE]
>Para baixar este documento no formato Word da Galeria do TechNet, consulte [implantar o DHCP usando o Windows PowerShell no Windows Server 2016](https://gallery.technet.microsoft.com/Deploy-DHCP-Using-Windows-246dd293).

Usando servidores DHCP para atribuir IP endereços salva na sobrecarga administrativa porque você não precisa configurar manualmente o TCP/IP v4 para cada adaptador de rede em cada computador na sua rede. Com o DHCP, configuração do TCP/IP v4 é executada automaticamente quando um computador ou outro cliente DHCP está conectado à sua rede.

Você pode implantar o servidor DHCP em um grupo de trabalho como um servidor autônomo ou como parte de um domínio do Active Directory.

Este guia contém as seções a seguir.

- [Visão geral da implantação do DHCP](#bkmk_overview)
- [Visões gerais de tecnologia](#bkmk_technologies)
- [Planejar a implantação do DHCP](#bkmk_plan)
- [Usando este guia em um laboratório de teste](#bkmk_lab)
- [Implantar o DHCP](#bkmk_deploy)
- [Verificar a funcionalidade do servidor](#bkmk_verify)
- [Comandos do Windows PowerShell para DHCP](#bkmk_dhcpwps)
- [Lista de comandos do Windows PowerShell neste guia](#bkmk_list)

## <a name="bkmk_overview"></a>Visão geral da implantação do DHCP

A ilustração a seguir ilustra o cenário que você pode implantar usando este guia. O cenário inclui um servidor DHCP em um domínio do Active Directory. O servidor está configurado para fornecer endereços IP a clientes DHCP em duas sub-redes diferentes. As sub-redes são separadas por um roteador que tem o encaminhamento de DHCP habilitado.

![Visão geral da topologia de rede do DHCP](../../media/Core-Network-Guide/cng16_overview.jpg)

## <a name="bkmk_technologies"></a>Visões gerais de tecnologia

As seções a seguir fornecem uma breve visão geral do TCP/IP e DHCP.

### <a name="dhcp-overview"></a>Visão geral do DHCP

O DHCP é um padrão de IP para simplificar o gerenciamento da configuração de IP do host. O padrão DHCP proporciona o uso de servidores DHCP como uma forma de gerenciar a alocação dinâmica de endereços IP e outros detalhes de configuração relacionados para clientes habilitados para DHCP em sua rede.

DHCP permite que você use um servidor DHCP para atribuir dinamicamente um endereço IP para um computador ou outro dispositivo, como uma impressora, em sua rede local, em vez de configurar manualmente cada dispositivo com um endereço IP estático.

Todos os computadores em uma rede TCP/IP devem ter um endereço IP exclusivo, porque o endereço IP e a máscara de sub-rede relacionada identificam o computador host e a sub-rede à qual o computador está conectado. Usando o DHCP, você pode garantir que todos os computadores configurados como clientes DHCP recebam um endereço IP que seja apropriado para seu local de rede e sua sub-rede e, usando as opções de DHCP (como o gateway padrão e os servidores DNS), você pode fornecer automaticamente aos clientes DHCP as informações que eles precisam para funcionar corretamente na rede.

Para redes baseadas em TCP/IP, o DHCP reduz a complexidade e a quantidade de trabalho administrativo envolvido na configuração de computadores.

### <a name="tcpip-overview"></a>Visão geral do TCP/IP

Por padrão, todas as versões dos sistemas operacionais Windows Server e o cliente do Windows têm configurações de TCP/IP IP versão 4 para conexões de rede configuradas para obter automaticamente um endereço IP e outras informações, chamado opções de DHCP de um servidor DHCP. Por isso, você não precisa definir configurações de TCP/IP manualmente, a menos que o computador é um computador servidor ou outro dispositivo que exige um endereço IP estático configurado manualmente. 

Por exemplo, é recomendável que você configure manualmente o endereço IP do servidor DHCP e os endereços IP dos servidores DNS e controladores de domínio que estão executando o Active Directory Domain Services \(AD DS\).

TCP/IP no Windows Server 2016 é a seguinte:

-   Software de rede baseado em protocolos de rede padrão do setor.

-   Um protocolo de rede de empresa roteável que suporta a conexão de seu computador baseado em Windows com os ambientes LAN (rede de área local) e WAN (rede de longa distância).

-   As principais tecnologias e utilitários para conectar seu computador baseado em Windows com os sistemas diferentes com a finalidade de compartilhar informações.

-   Uma base para obter acesso aos serviços de Internet globais, como servidores Web e o protocolo FTP (File Transfer).

-   Uma estrutura robusta, escalável, multiplataforma, de cliente/servidor.

O TCP/IP oferece utilitários TCP/IP básicos que permitem que computadores baseados no Windows se conectem e compartilhem informações com outros sistemas Microsoft e não Microsoft, incluindo:

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

- Hosts da Internet

- Sistemas Apple Macintosh

- Mainframes IBM

- Sistemas UNIX e Linux

- Sistemas VMS abertos

- Impressoras prontas para a rede

- Tablets e telefones celulares com Ethernet com fio ou sem fio 802.11 a tecnologia habilitada

## <a name="bkmk_plan"></a>Planejar a implantação do DHCP

Estas são as principais etapas de planejamento antes de instalar a função de servidor DHCP.

### <a name="planning-dhcp-servers-and-dhcp-forwarding"></a>Planejando os servidores DHCP e o encaminhamento DHCP

Como as mensagens DHCP são mensagens de difusão, elas não são encaminhadas entre as sub-redes por roteadores. Se você tiver várias sub-redes e desejar fornecer o serviço DHCP em cada sub-rede, deverá fazer o seguinte:

-   Instalar um servidor DHCP em cada sub-rede

-   Configurar roteadores para encaminhar as mensagens de difusão DHCP entre sub-redes e configurar vários escopos no servidor DHCP, um escopo por sub-rede.

Na maioria dos casos, a configuração de roteadores para encaminhar mensagens de difusão DHCP é mais econômica que a implantação de um servidor DHCP em cada segmento físico da rede.

### <a name="planning-ip-address-ranges"></a>Planejando intervalos de endereços IP

Cada sub-rede deve ter seu próprio intervalo de endereços IP exclusivo. Esses intervalos são representados em um servidor DHCP com escopos.

Um escopo é um agrupamento administrativo de endereços IP para computadores em uma sub-rede que usa o serviço DHCP. O administrador primeiro cria um escopo para cada sub-rede física e, em seguida, usa o escopo para definir os parâmetros usados pelos clientes.

Um escopo tem as seguintes propriedades:

-   Um intervalo de endereços IP do qual incluir ou excluir endereços usados para ofertas de concessão de serviço DHCP.

-   Uma máscara de sub-rede, que determina o prefixo de sub-rede para um determinado endereço IP.

-   Um nome de escopo atribuído quando é criado.

-   Valores de duração da concessão, que são atribuídos aos clientes DHCP que recebem endereços IP alocados dinamicamente.

-   Qualquer opção de escopo DHCP configurada para atribuição a clientes DHCP, como endereço ID do servidor DNS ou endereço IP do gateway do roteador/padrão.

-   As reservas são opcionalmente usadas para garantir que um cliente DHCP sempre receba o mesmo endereço IP.

Antes de implantar os servidores, liste suas sub-redes e o intervalo de endereços IP que você deseja usar para cada sub-rede.

### <a name="planning-subnet-masks"></a>Planejando máscaras de sub-rede

As IDs de rede e IDs de host são diferenciadas usando uma máscara de sub-rede. Cada máscara de sub-rede é um número de 32 bits que usa grupos de bits consecutivos de todos (1) para identificar a ID da rede e todos os zeros (0) para identificar as partes de ID do host de um endereço IP.

Por exemplo, a máscara de sub-rede normalmente usada com o endereço IP 131.107.16.200 é o seguinte número binário de 32 bits:

```
11111111 11111111 00000000 00000000
```

Esse número de máscara de sub-rede é 16 bits de um seguido de 16 bits de zero, indicando que a ID e o host ID seções de rede desse endereço IP têm 16 bits de comprimento. Normalmente, essa máscara de sub-rede é exibida em notação decimal com ponto como 255.255.0.0.

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

Para resolver esse problema, você pode criar um intervalo de exclusão para o escopo DHCP. Um intervalo de exclusão é um contíguos intervalo de endereços IP dentro do intervalo de endereços IP do escopo que o servidor DHCP não tem permissão para usar. Se você criar um intervalo de exclusão, o servidor DHCP não atribuirá os endereços nesse intervalo, permitindo que você atribua manualmente esses endereços sem criar um conflito de endereços IP.

Você pode excluir endereços IP da distribuição pelo servidor DHCP, criando um intervalo de exclusão para cada escopo. Use as exclusões para todos os dispositivos configurados com um endereço IP estático. Os endereços excluídos devem incluir todos os endereços IP que você atribuiu manualmente a outros servidores, clientes não-DHCP, estações de trabalho sem disco ou clientes de Roteamento, Acesso Remoto e PPP.

Recomendamos que você configure o intervalo de exclusão com endereços extras para acomodar o crescimento futuro da rede. A tabela a seguir fornece um intervalo de exclusão de exemplo para um escopo com um intervalo de endereços IP 10.0.0.1 - 10.0.0.254 e uma máscara de sub-rede de 255.255.255.0.

|Itens de configuração|Valores de exemplo|
|-----------------------|------------------|
|Endereço IP Inicial do intervalo de exclusão|10.0.0.1|
|Endereço IP Final do intervalo de exclusão|10.0.0.25|

### <a name="planning-tcpip-static-configuration"></a>Planejando a configuração de TCP/IP estática

Certos dispositivos, como roteadores, servidores DHCP e servidores DNS, devem ser configurados com um endereço IP estático. Além disso, você pode ter dispositivos adicionais, como impressoras, onde deve garantir sempre o mesmo endereço IP. Liste os dispositivos que você deseja configurar estaticamente para cada sub-rede e planeje o intervalo de exclusão que deseja usar no servidor DHCP para garantir que o servidor DHCP não conceda o endereço IP de um dispositivo configurado estaticamente. Um intervalo de exclusão é uma sequência limitada de endereços IP dentro de um escopo, excluído das ofertas de serviço DHCP. Os intervalos de exclusão garantem que os endereços nesses intervalos não sejam oferecidos pelo servidor a clientes DHCP na sua rede.

Por exemplo, se o intervalo de endereços IP para uma sub-rede for 192.168.0.1 a 192.168.0.254 e você tiver dez dispositivos que você deseja configurar com um endereço IP estático, você pode criar um intervalo de exclusão para o 192.168.0. *x* escopo que inclui dez ou mais endereços IP: 192.168.0.1 a 192.168.0.15.

Neste exemplo, você usa dez dos endereços IP excluídos para configurar servidores e outros dispositivos com endereços IP estáticos e cinco endereços IP adicionais ficam disponíveis para uma configuração estática de novos dispositivos que você possa desejar adicionar no futuro. Com este intervalo de exclusão, o servidor DHCP fica com um pool de endereços de 192.168.0.16 até 192.168.0.254.

Itens de configuração de exemplo adicionais para AD DS e DNS são fornecidos na tabela a seguir.

|Itens de configuração|Valores de exemplo|
|-----------------------|------------------|
|Ligações de Conexão de Rede|Ethernet|
|Configurações do servidor DNS|DC1.corp.contoso.com|
|Endereço IP do servidor DNS preferencial|10.0.0.2|
|Valores de escopo<br /><br />1.  Nome do escopo<br />2.  Endereço IP inicial<br />3.  Endereço IP final<br />4.  Máscara de sub-rede<br />5.  Gateway padrão (opcional)<br />6.  Duração da concessão|1.  Sub-rede primária<br />2.  10.0.0.1<br />3.  10.0.0.254<br />4.  255.255.255.0<br />5.  10.0.0.1<br />6. 8 dias|
|Modo de Operação do Servidor DHCP IPv6|Não habilitado|

## <a name="bkmk_lab"></a>Usando este guia em um laboratório de teste

Você pode usar este guia para implantar o DHCP em um laboratório de teste antes de implantar em um ambiente de produção. 

>[!NOTE]
>Se você não quiser implantar o DHCP em um laboratório de teste, você pode pular para a seção [DHCP implantar](#bkmk_deploy).

Os requisitos para seu laboratório diferem dependendo se você estiver usando servidores físicos ou máquinas virtuais \(VMs\), e você estiver usando um domínio do Active Directory ou implantando um servidor DHCP de autônomo. 

Você pode usar as informações a seguir para determinar os recursos mínimos que necessários para testar a implantação de DHCP usando este guia.

### <a name="test-lab-requirements-with-vms"></a>Requisitos de laboratório de teste com máquinas virtuais

Para implantar o DHCP em um laboratório de teste com máquinas virtuais, você precisa ter os seguintes recursos.

Para implantação do domínio ou implantação autônoma, você precisa de um servidor que esteja configurado como um Hyper\-host V.

**Implantação do domínio**

Essa implantação exige que um servidor físico, um comutador virtual, dois servidores virtuais e um cliente virtual:

Em seu servidor físico, no Gerenciador do Hyper-V, crie os itens a seguir.

1. Uma **interno** comutador virtual. Não crie uma **externos** comutador virtual, porque se seu Hyper\-host V está em uma sub-rede que inclui um servidor DHCP, suas VMs de teste receberá um endereço IP do seu servidor DHCP. Além disso, o servidor DHCP de teste que você implanta pode atribuir endereços IP para outros computadores na sub-rede em que o Hyper\-host V está instalado.
1. Uma VM que executa o Windows Server 2016, configurado como um controlador de domínio com o Active Directory Domain Services que está conectado ao comutador virtual interno que você criou. Para corresponder a este guia, esse servidor deve ter um endereço IP configurado estaticamente 10.0.0.2. Para obter informações sobre como implantar o AD DS, consulte a seção **Implantando o DC1** no Windows Server 2016 [guia da rede principal](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployADDNS01).
1. Uma VM que executa o Windows Server 2016, você configurará como um servidor DHCP usando este guia e que está conectada para o interno virtual mudar, você criou. 
1. Uma VM que executa um sistema de operacional de cliente do Windows que está conectado como o interno virtual switch você criou e que você usará para verificar que seu servidor DHCP é alocar dinamicamente endereços IP e opções de DHCP a clientes DHCP.

**Implantação do servidor DHCP Standalone**

Essa implantação exige que um servidor físico, um comutador virtual, um servidor virtual e um cliente virtual:

Em seu servidor físico, no Gerenciador do Hyper-V, crie os itens a seguir.

1. Uma **interno** comutador virtual. Não crie uma **externos** comutador virtual, porque se seu Hyper\-host V está em uma sub-rede que inclui um servidor DHCP, suas VMs de teste receberá um endereço IP do seu servidor DHCP. Além disso, o servidor DHCP de teste que você implanta pode atribuir endereços IP para outros computadores na sub-rede em que o Hyper\-host V está instalado.
1. Uma VM que executa o Windows Server 2016, você configurará como um servidor DHCP usando este guia e que está conectada para o interno virtual mudar, você criou.
1. Uma VM que executa um sistema de operacional de cliente do Windows que está conectado como o interno virtual switch você criou e que você usará para verificar que seu servidor DHCP é alocar dinamicamente endereços IP e opções de DHCP a clientes DHCP.

### <a name="test-lab-requirements-with-physical-servers"></a>Requisitos de laboratório de teste com servidores físicos

Para implantar o DHCP em um laboratório de teste com servidores físicos, você precisa ter os seguintes recursos.

**Implantação do domínio**

Essa implantação exige que um hub ou comutador, dois servidores físicos e um cliente físico:

1. Um hub ou comutador Ethernet para o qual você pode conectar os computadores físicos com cabos Ethernet
1. Um computador físico executando o Windows Server 2016, configurado como um controlador de domínio com o Active Directory Domain Services. Para corresponder a este guia, esse servidor deve ter um endereço IP configurado estaticamente 10.0.0.2. Para obter informações sobre como implantar o AD DS, consulte a seção **Implantando o DC1** no Windows Server 2016 [guia da rede principal](https://technet.microsoft.com/windows-server-docs/networking/core-network-guide/core-network-guide#BKMK_deployADDNS01).
1. Um computador físico executando o Windows Server 2016 que será configurado como um servidor DHCP usando este guia. 
1. Um computador físico, executando um sistema de operacional de cliente do Windows que você usará para verificar se seu servidor DHCP é alocar dinamicamente endereços IP e opções de DHCP a clientes DHCP.

>[!NOTE]
>Se você não tem suficiente máquinas de teste para essa implantação, você pode usar uma máquina de teste para AD DS e o DHCP – no entanto, essa configuração não é recomendada para um ambiente de produção.

**Implantação do servidor DHCP Standalone**

Essa implantação exige que um hub ou comutador, um servidor físico e um cliente físico:

1. Um hub ou comutador Ethernet para o qual você pode conectar os computadores físicos com cabos Ethernet
2. Um computador físico executando o Windows Server 2016 que será configurado como um servidor DHCP usando este guia. 
3. Um computador físico, executando um sistema de operacional de cliente do Windows que você usará para verificar se seu servidor DHCP é alocar dinamicamente endereços IP e opções de DHCP a clientes DHCP.


## <a name="bkmk_deploy"></a>Implantar o DHCP

Esta seção fornece comandos do Windows PowerShell de exemplo que você pode usar para implantar o DHCP em um servidor. Antes de executar esses comandos de exemplo em seu servidor, você deve modificar os comandos para corresponder ao seu ambiente e rede. 

Por exemplo, antes de executar os comandos, você deve substituir os valores de exemplo nos comandos para os seguintes itens:

- Nomes de computador
- Intervalo de endereços IP para cada escopo que você deseja configurar (1 escopo por sub-rede)
- Máscara de sub-rede para cada intervalo de endereços IP que você deseja configurar
- Nome do escopo para cada escopo
- Intervalo de exclusão para cada escopo
- Valores de opção do DHCP, como o gateway padrão, o nome de domínio e servidores DNS ou WINS
- Nomes de interface

>[!IMPORTANT]
>Examinar e modificar todos os comandos para o seu ambiente antes de executar o comando.

### <a name="where-to-install-dhcp---on-a-physical-computer-or-a-vm"></a>Onde instalar DHCP - em um computador físico ou uma VM?

Você pode instalar a função de servidor DHCP em um computador físico ou em uma máquina virtual \(VM\) que é instalado em um Hyper\-host V. Se você estiver instalando o DHCP em uma máquina virtual e o servidor DHCP para fornecer as atribuições de endereço IP a computadores na rede física ao qual o host Hyper-V está conectado, você deve se conectar o adaptador de rede virtual da VM a um comutador Virtual Hyper-V que está **Externo**.

Para obter mais informações, consulte a seção **criar um comutador Virtual com o Gerenciador do Hyper-V** no tópico [criar uma rede virtual](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/connect-to-network).

### <a name="run-windows-powershell-as-an-administrator"></a>Execute o Windows PowerShell como administrador

Você pode usar o procedimento a seguir para executar o Windows PowerShell com privilégios de administrador.

1. Em um computador executando o Windows Server 2016, clique em **iniciar**, em seguida, clique com botão direito no ícone do Windows PowerShell. Um menu é exibido. 

2. No menu, clique em **mais**e, em seguida, clique em **executar como administrador**. Se solicitado, digite as credenciais para uma conta que tenha privilégios de administrador no computador. Se a conta de usuário com a qual você está conectado ao computador é uma conta de nível de administrador, você não receberá uma solicitação de credenciais.

3. Windows PowerShell é aberto com privilégios de administrador.

### <a name="rename-the-dhcp-server-and-configure-a-static-ip-address"></a>Renomear o servidor DHCP e configure um endereço IP estático

Se você ainda não fez isso, você pode usar os seguintes comandos do Windows PowerShell para renomear o servidor DHCP e configure um endereço IP estático para o servidor.

**Configurar um endereço IP estático**

Você pode usar os comandos a seguir para atribuir um endereço IP estático para o servidor DHCP e configurar as propriedades de TCP/IP do servidor DHCP com o endereço IP de servidor DNS correto. Você também deve substituir os nomes de interface e os endereços IP neste exemplo pelos valores que deseja usar para configurar o seu computador.

 `New-NetIPAddress -IPAddress 10.0.0.3 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24`

 `Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 10.0.0.2`

Para obter mais informações sobre esses comandos, consulte os tópicos a seguir.

- [New-NetIPAddress](https://technet.microsoft.com/itpro/powershell/windows/tcpip/new-netipaddress)
- [Set-DnsClientServerAddress](https://technet.microsoft.com/itpro/powershell/windows/dns-client/set-dnsclientserveraddress)

**Renomear o computador**

Você pode usar os comandos a seguir para renomear e, em seguida, reinicie o computador.

`Rename-Computer -Name DHCP1`

 `Restart-Computer`

Para obter mais informações sobre esses comandos, consulte os tópicos a seguir.

- [Rename-Computer](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/rename-computer)
- [Restart-Computer](https://msdn.microsoft.com/powershell/reference/4.0/microsoft.powershell.management/restart-computer)

### <a name="join-the-computer-to-the-domain-optional"></a>Ingressar o computador ao domínio \(opcional\)

Se você estiver instalando o servidor DHCP em um ambiente de domínio do Active Directory, você deve associar o computador ao domínio. Abra o Windows PowerShell com privilégios de administrador e, em seguida, execute o comando a seguir depois de substituir o nome NetBios do domínio **CORP** com um valor que é apropriado para seu ambiente.

    Add-Computer CORP

Quando solicitado, digite as credenciais para uma conta de usuário de domínio que tenha permissão para ingressar um computador no domínio. 

    Restart-Computer

Para obter mais informações sobre o comando Add-Computer, consulte o tópico a seguir.

- [Adicionar computador](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/add-computer)

### <a name="install-dhcp"></a>Instalar o DHCP

Após reiniciar o computador, abra o Windows PowerShell com privilégios de administrador e, em seguida, instalar o DHCP executando o comando a seguir.

    Install-WindowsFeature DHCP -IncludeManagementTools

Para obter mais informações sobre esse comando, consulte o tópico a seguir.

- [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/server-manager/install-windowsfeature)

### <a name="create-dhcp-security-groups"></a>Criar grupos de segurança do DHCP

Para criar grupos de segurança, você deve executar um Shell de rede \(netsh\) de comando no Windows PowerShell e, em seguida, reiniciar o serviço DHCP para que os novos grupos se tornar ativos.

Quando você executa os seguintes comandos netsh no servidor DHCP, o **Administradores DHCP** e **usuários DHCP** grupos de segurança são criados no **usuários e grupos locais** no DHCP servidor.

    netsh dhcp add securitygroups

O comando a seguir reinicia o serviço DHCP no computador local.

    Restart-service dhcpserver

Para obter mais informações sobre esses comandos, consulte os tópicos a seguir.

- [Network Shell (Netsh)](../netsh/netsh.md)
- [Restart-Service](https://msdn.microsoft.com/powershell/reference/5.1/microsoft.powershell.management/restart-service)

### <a name="authorize-the-dhcp-server-in-active-directory-optional"></a>Autorizar o servidor DHCP no Active Directory \(opcional\)

Se você estiver instalando o DHCP em um ambiente de domínio, você deve executar as seguintes etapas para autorizar o servidor DHCP para operar no domínio.

>[!NOTE]
>Servidores DHCP não autorizados que estejam instalados em domínios do Active Directory não podem funcionar corretamente e não concessões de endereços IP a clientes DHCP. A desativação automática de servidores DHCP não autorizados é um recurso de segurança que impede que os servidores DHCP não autorizados atribuindo endereços IP incorretos aos clientes em sua rede.

Você pode usar o comando a seguir para adicionar o servidor DHCP para a lista de servidores DHCP autorizados no Active Directory. 

>[!NOTE]
>Se você não tiver um ambiente de domínio, não execute esse comando.

 `Add-DhcpServerInDC -DnsName DHCP1.corp.contoso.com -IPAddress 10.0.0.3`

Para verificar se o servidor DHCP é autorizado no Active Directory, você pode usar o comando a seguir.

    Get-DhcpServerInDC

A seguir estão os resultados de exemplo que são exibidos no Windows PowerShell.

    
        IPAddress   DnsName
        ---------   -------
        10.0.0.3    DHCP1.corp.contoso.com
    

Para obter mais informações sobre esses comandos, consulte os tópicos a seguir.

- [Add-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverindc)
- [Get-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/get-dhcpserverindc)

### <a name="notify-server-manager-that-post-install-dhcp-configuration-is-complete-optional"></a>Notificar o Gerenciador do servidor que lançar\-configuração de DHCP de instalação for concluída \(opcional\)

Depois de concluir post\-tarefas de instalação, como criar grupos de segurança e autorização de um servidor DHCP no Active Directory, o Gerenciador do servidor ainda podem exibir um alerta na interface do usuário informando que essa postagem\- etapas de instalação devem ser concluídas usando o Assistente de configuração de instalação de postagem de DHCP.

Você pode evitar isso agora\-desnecessária e imprecisa mensagem apareça no Gerenciador do servidor ao configurar a seguinte chave do registro usando este comando do Windows PowerShell.

    Set-ItemProperty –Path registry::HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ServerManager\Roles\12 –Name ConfigurationState –Value 2

Para obter mais informações sobre esse comando, consulte o tópico a seguir.

- [Set-ItemProperty](https://msdn.microsoft.com/powershell/reference/4.0/microsoft.powershell.management/set-itemproperty?f=255&MSPPError=-2147217396)

### <a name="set-server-level-dns-dynamic-update-configuration-settings-optional"></a>Definir as configurações de atualização dinâmica de servidor nível DNS \(opcional\)

Se você quiser que o servidor DHCP para executar atualizações dinâmicas de DNS para computadores cliente DHCP, você pode executar o comando a seguir para definir essa configuração. Esse é um nível de servidor definindo, não um escopo nível definindo, portanto, isso afetará todos os escopos que você configura no servidor. Este exemplo de comando também configura o servidor DHCP para excluir registros de recurso DNS para clientes quando o cliente menos expira.

    Set-DhcpServerv4DnsSetting -ComputerName "DHCP1.corp.contoso.com" -DynamicUpdates "Always" -DeleteDnsRRonLeaseExpiry $True

Você pode usar o comando a seguir para configurar as credenciais que o servidor DHCP usa para registrar ou cancelar o registro de registros de clientes em um servidor DNS. Este exemplo salva uma credencial em um servidor DHCP. O primeiro comando usa **Get-Credential** para criar um **PSCredential** de objeto e, em seguida, armazena o objeto no **$Credential** variável. O comando solicitará o nome de usuário e senha, portanto, certifique-se de que você forneça credenciais para uma conta que tenha permissão para atualizar registros de recursos em seu servidor DNS.

    
    $Credential = Get-Credential
    Set-DhcpServerDnsCredential -Credential $Credential -ComputerName "DHCP1.corp.contoso.com"
    

Para obter mais informações sobre esses comandos, consulte os tópicos a seguir.

- [Set-DhcpServerv4DnsSetting](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverv4dnssetting)
- [Set-DhcpServerDnsCredential](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverdnscredential)

### <a name="configure-the-corpnet-scope"></a>Configure o escopo da rede corporativa

Após a instalação do DHCP, você pode usar os comandos a seguir para configurar e ativar o escopo da rede corporativa, criar um intervalo de exclusão para o escopo e configurar o gateway de padrão de opções de DHCP, o endereço IP do servidor DNS e o nome de domínio DNS.

    
    Add-DhcpServerv4Scope -name "Corpnet" -StartRange 10.0.0.1 -EndRange 10.0.0.254 -SubnetMask 255.255.255.0 -State Active`
    
    Add-DhcpServerv4ExclusionRange -ScopeID 10.0.0.0 -StartRange 10.0.0.1 -EndRange 10.0.0.15`
    
    Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.0.1 -ScopeID 10.0.0.0 -ComputerName DHCP1.corp.contoso.com`
    
    Set-DhcpServerv4OptionValue -DnsDomain corp.contoso.com -DnsServer 10.0.0.2
    

Para obter mais informações sobre esses comandos, consulte os tópicos a seguir.

- [Add-DhcpServerv4Scope](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverv4scope)
- [Add-DhcpServerv4ExclusionRange](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/add-dhcpserverv4exclusionrange)
- [Set-DhcpServerv4OptionValue](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/set-dhcpserverv4optionvalue)

### <a name="configure-the-corpnet2-scope-optional"></a>Configurar o escopo de Corpnet2 \(opcional\)

Se você tiver uma segunda sub-rede que está conectada à primeira sub-rede com um roteador, onde o encaminhamento de DHCP está habilitado, você pode usar os comandos a seguir para adicionar um segundo escopo nomeado Corpnet2 para este exemplo. Este exemplo também configura um intervalo de exclusão e o endereço IP do gateway padrão \(endereço IP do roteador na sub-rede\) da sub-rede Corpnet2.

 `Add-DhcpServerv4Scope -name "Corpnet2" -StartRange 10.0.1.1 -EndRange 10.0.1.254 -SubnetMask 255.255.255.0 -State Active`

 `Add-DhcpServerv4ExclusionRange -ScopeID 10.0.1.0 -StartRange 10.0.1.1 -EndRange 10.0.1.15`

 `Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.1.1 -ScopeID 10.0.1.0 -ComputerName DHCP1.corp.contoso.com`

Se você tiver sub-redes adicionais que são atendidas por este servidor DHCP, você pode repetir esses comandos, usando valores diferentes para todos os parâmetros de comando, para adicionar escopos para cada sub-rede.

>[!IMPORTANT]
>Certifique-se de que todos os roteadores entre os clientes DHCP e o servidor DHCP são configurados para encaminhamento de mensagens DHCP. Consulte a documentação do roteador para obter informações sobre como configurar o encaminhamento de DHCP.

## <a name="bkmk_verify"></a>Verificar a funcionalidade do servidor

Para verificar que seu servidor DHCP está fornecendo a alocação dinâmica de endereços IP a clientes DHCP, você pode se conectar a outro computador a uma sub-rede de serviço. Depois de conectar o cabo Ethernet para o adaptador de rede e ligue o computador, ele solicitará um endereço IP do servidor DHCP. Você pode verificar a configuração bem-sucedida usando o **ipconfig/all** comando e examinar os resultados ou executando testes de conectividade, como a tentativa de acessar recursos da Web com seu navegador ou compartilhamentos de arquivos com Windows Explorer ou outros aplicativos.

Se o cliente recebe um endereço IP do seu servidor DHCP, execute as seguintes etapas de solução de problemas.

1. Certifique-se de que o cabo Ethernet é conectado tanto o computador e a Ethernet switch, hub ou roteador.
1. Se você conectou o computador cliente em um segmento de rede que é separado do servidor DHCP por um roteador, certifique-se de que o roteador está configurado para encaminhar mensagens DHCP.
1. Certifique-se de que o servidor DHCP está autorizado no Active Directory, executando o seguinte comando para recuperar a lista de servidores DHCP autorizados do Active Directory. [Get-DhcpServerInDC](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/get-dhcpserverindc).
1. Certifique-se de que os escopos são ativados, abrindo o console do DHCP \(Gerenciador de servidores **ferramentas**, **DHCP**\), expandir a árvore de servidores para analisar escopos, em seguida, com o botão direito\-clicando em cada escopo. Se o menu resultante inclui a seleção **Activate**, clique em **ativar**. \(Se o escopo já estiver ativado, a seleção de menu lê **Deactivate**.\) 

## <a name="bkmk_dhcpwps"></a>Comandos do Windows PowerShell para DHCP

A seguinte referência fornece descrições de comando e a sintaxe para todos os comandos do PowerShell do DHCP Server Windows para Windows Server 2016. Este tópico lista os comandos em ordem alfabética com base no verbo no início dos comandos, como **Obtenha** ou **definir**.

>[!NOTE]
>Você não pode usar comandos do Windows Server 2016 no Windows Server 2012 R2.

- [DhcpServer Module](https://technet.microsoft.com/itpro/powershell/windows/dhcp-server/index)

A referência a seguir fornece descrições de comando e a sintaxe para todos os comandos do PowerShell do DHCP Server Windows para Windows Server 2012 R2. Este tópico lista os comandos em ordem alfabética com base no verbo no início dos comandos, como **Obtenha** ou **definir**.

>[!NOTE]
>Você pode usar comandos do Windows Server 2012 R2 no Windows Server 2016.

- [Cmdlets do servidor DHCP no Windows PowerShell](https://technet.microsoft.com/library/jj590751.aspx)

## <a name="bkmk_list"></a>Lista de comandos do Windows PowerShell neste guia

A seguir está uma lista simples de comandos e valores de exemplo que são usados neste guia.

    
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
    


