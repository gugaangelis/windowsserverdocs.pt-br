---
title: Guia da rede principal
description: Este guia fornece instruções sobre como planejar e implantar os componentes principais necessários para uma rede totalmente funcional e um novo domínio do Active Directory em uma nova floresta com o Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: b3cd60f7-d380-4712-9a78-0a8f551e1121
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 90390a7b975bc358fb96715d23fe542bc261220e
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="core-network-guide"></a>Guia da rede principal

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este guia fornece instruções sobre como planejar e implantar os componentes principais necessários para uma rede totalmente funcional e um novo domínio do Active Directory em uma nova floresta.

> [!NOTE]
> Este guia está disponível para download no formato do Microsoft Word da Galeria do TechNet. Para obter mais informações, consulte [Core rede guia para Windows Server 2016](https://gallery.technet.microsoft.com/Core-Network-Guide-for-9da2e683).

Este guia contém as seguintes seções.

- [Sobre este guia](#BKMK_about)

- [Visão geral da rede principal](#BKMK_overview)

- [Planejamento de rede principal](#BKMK_planning)

- [Implantação de rede principal](#BKMK_deployment)

- [Recursos técnicos adicionais](#BKMK_resources)

- [Apêndices A por meio eletrônico](#BKMK_appendix)

## <a name="BKMK_about"></a>Sobre este guia
Este guia destina-se a administradores de rede e do sistema que estiver instalando uma nova rede ou que querem criar uma rede baseada em domínio para substituir uma rede que consiste em grupos de trabalho. O cenário de implantação fornecido neste guia é especialmente útil se você prever a necessidade de adicionar mais recursos e serviços para sua rede no futuro.

É recomendável que você reveja o design e implantação de guias para cada uma das tecnologias usadas neste cenário de implantação para ajudá-lo a determinar se este guia fornece os serviços e configuração que você precisa.

Uma rede principal é uma coleção de hardware de rede, dispositivos, e precisa de um software que fornece os principais serviços de tecnologia da informação (TI) da sua organização.

Uma rede do Windows Server core fornece muitos benefícios, incluindo o seguinte.

-   Protocolos de Core para conectividade de rede entre computadores e outros dispositivos compatíveis com o protocolo/Internet (TCP). TCP/IP é um conjunto de protocolos padrão para conectar computadores e criação de redes. TCP/IP é um software de protocolo de rede fornecido com sistemas operacionais Microsoft Windows que implementa e compatível com o pacote de protocolo TCP/IP.

-   Dynamic Host Configuration Protocol (DHCP) automática atribuição de endereço IP a computadores e outros dispositivos que estão configurados como clientes DHCP. Configuração manual de endereços IP em todos os computadores em sua rede é demorado e menos flexível que fornecendo dinamicamente outros dispositivos e computadores com configurações de endereço IP usando um servidor DHCP.

-   Serviço de resolução de nomes do sistema de nome de domínio (DNS). DNS permite que os usuários, computadores, aplicativos e serviços encontrar os endereços IP dos computadores e dispositivos na rede usando o nome totalmente qualificado domínio do computador ou dispositivo.

-   Uma floresta, que é um ou mais domínios do Active Directory que compartilham o mesmo definições de classe e atributo (esquema), informações de site e replicação (configuração) e recursos de pesquisa de toda a floresta (catálogo global).

-   Um domínio raiz da floresta, que é o primeiro domínio criado em uma nova floresta. Os grupos de administradores de empresa e administradores de esquema, que são grupos administrativos toda a floresta, estão localizados no domínio raiz da floresta. Além disso, um domínio raiz da floresta, assim como acontece com outros domínios, é uma coleção de computador, usuários e objetos de grupo que são definidos pelo administrador nos serviços de domínio do Active Directory (AD DS). Esses objetos compartilham um políticas comuns de banco de dados e segurança do diretório. Eles também podem compartilhar relações de segurança com outros domínios se você adicionar domínios que sua organização cresce. O serviço de diretório também armazena dados de diretório e permite que computadores autorizados, aplicativos e os usuários acessem os dados.

-   Um usuário e computador conta banco de dados. O serviço de diretório fornece um banco de dados de contas de usuário de maneira centralizada que permite que você crie contas de usuário e computador para pessoas e computadores que estão autorizados a se conectar à sua rede e acesso a rede, recursos, como aplicativos, bancos de dados, arquivos compartilhados e pastas e impressoras.

Uma rede principal também permite que você dimensione sua rede que sua organização cresce e mudam de requisitos de TI. Por exemplo, com uma rede principal, você pode adicionar domínios, sub-redes IP, serviços de acesso remoto, serviços sem fio e outros recursos e funções de servidor fornecidas pelo Windows Server 2016.

### <a name="network-hardware-requirements"></a>Requisitos de hardware de rede
Para implantar com êxito uma rede principal, você deve implantar o hardware de rede, incluindo o seguinte:

-   Ethernet, Fast Ethernet ou Ethernet Gigabyte cabeamento

-   Um hub, camada 2 ou 3 switch, roteador ou outro dispositivo que executa a função de retransmitir o tráfego de rede entre computadores e dispositivos.

-   Computadores que atendem aos requisitos mínimos de hardware para seus respectivos sistemas operacionais cliente e servidor.

## <a name="what-this-guide-does-not-provide"></a>O que este guia não fornece
Este guia não fornece instruções para implantar o seguinte:

-   Hardware de rede, como cabeamento, roteadores, comutadores e hubs

-   Recursos de rede adicionais, como impressoras e servidores de arquivos

-   Conectividade com a Internet

-   Acesso remoto

-   Acesso sem fio

-   Implantação de computador cliente

> [!NOTE]
> Computadores que executam sistemas operacionais clientes Windows são configurados por padrão para receber concessões de endereço IP do servidor DHCP. Portanto, nenhuma adicionais DHCP ou protocolo IP versão 4 (IPv4) configuração de computadores cliente é necessária.

## <a name="technology-overviews"></a>Visões gerais de tecnologia
As seções a seguir apresentam um breve resumo das tecnologias necessários são implantados para criar uma rede principal.

### <a name="active-directory-domain-services"></a>Serviços de domínio do Active Directory
Um diretório é uma estrutura hierárquica que armazena informações sobre objetos na rede, como os usuários e computadores. Um serviço de diretório, como o AD DS, fornece os métodos para armazenar dados de diretório e tornar esses dados disponíveis para administradores e usuários de rede. Por exemplo, o AD DS armazena informações sobre contas de usuário, incluindo nomes, endereços de email, senhas e números de telefone e permite que outros usuários autorizados na mesma rede acessar essas informações.

### <a name="dns"></a>DNS
DNS é um protocolo de resolução de nome para redes TCP/IP, como a Internet ou uma rede da organização. Um servidor DNS hospeda as informações que permite que os computadores cliente e serviços para resolver facilmente reconhecido, nomes DNS alfanuméricos para os endereços IP que os computadores usam para se comunicar uns com os outros.

### <a name="dhcp"></a>DHCP
DHCP é um padrão para simplificar o gerenciamento de configuração de IP do host IP. O padrão DHCP fornece o uso de servidores DHCP como uma maneira de gerenciar a alocação dinâmica de endereços IP e outros detalhes de configuração relacionados para clientes DHCP em sua rede.

DHCP permite que você use um servidor DHCP para atribuir um endereço IP dinamicamente a um computador ou outro dispositivo, como uma impressora, em sua rede local. Todos os computadores em uma rede TCP/IP devem ter um endereço IP exclusivo, porque o endereço IP e máscara de sub-rede relacionada é identificam o computador host e a sub-rede ao qual o computador está conectado. Usando o DHCP, você pode garantir que todos os computadores que estão configurados como clientes DHCP recebem um endereço IP que é adequado para seu local de rede e a sub-rede e usando as opções de DHCP, como gateway padrão e servidores DNS, você pode fornecer automaticamente clientes DHCP com as informações que eles precisam funcionar corretamente em sua rede.

Para redes baseadas em TCP/IP, o DHCP reduz a complexidade e a quantidade de trabalho administrativo envolvido na reconfiguração dos computadores.

### <a name="tcpip"></a>TCP/IP
TCP/IP no Windows Server 2016 é o seguinte:

-   Software de rede com base em protocolos de rede padrão da indústria.

-   Um protocolo de rede corporativa pode ser roteado que dá suporte a conexão do computador baseado no Windows para a rede local (LAN) e ambientes de rede (WAN) de longa distância.

-   Principais tecnologias e utilitários para conectar seu computador baseado em Windows sistemas diferentes para fins de compartilhamento de informações.

-   Uma base para obter acesso aos serviços de Internet globais, como os servidores do World Wide Web e File Transfer Protocol (FTP).

-   Uma estrutura de cliente/servidor robusta, escalonável e de plataforma cruzada.

TCP/IP oferece utilitários TCP/IP básicos que permitem aos computadores baseados no Windows para se conectar e compartilhar informações com outros sistemas Microsoft e não são da Microsoft, incluindo:

-    Windows Server 2016

-   Windows 10

-    Windows Server 2012 R2

-   Windows 8.1

-    Windows Server 2012

-   Windows 8

-    Windows Server 2008 R2

-    Windows 7

-    Windows Server 2008

-   Windows Vista

-   Hosts de Internet

-   Sistemas Apple Macintosh

-   Mainframes IBM

-   Sistemas UNIX e Linux

-   Sistemas VMS abertos

-   Impressoras prontas para a rede

-   Tablets e telefones celulares com Ethernet com fio ou tecnologia sem fio 802.11 habilitada

## <a name="BKMK_overview"></a>Visão geral da rede principal
A ilustração a seguir mostra a topologia de rede do Windows Server Core.

![Topologia de rede do Windows Server Core](../media/Core-Network-Guide/cng16_overview.jpg)

> [!NOTE]
> Este guia também inclui instruções para adicionar servidores opcionais de servidor de política de rede (NPS) e Web Server (IIS) para a topologia de rede para fornecer a base para soluções de acesso de rede seguras, como 802.1 X com fio e sem fio implantações que você pode implementar usando guias complementar de rede principal. Para obter mais informações, consulte [implantar recursos opcionais para autenticação de acesso de rede e serviços Web](#BKMK_optionalfeatures).

### <a name="core-network-components"></a>Principais componentes de rede
A seguir estão os componentes de uma rede principal.

##### <a name="router"></a>Roteador
Este guia de implantação fornece instruções para implantar uma rede principal com duas sub-redes separados por um roteador que tenha habilitado o encaminhamento de DHCP. No entanto, você pode implantar um comutador de camada 2, um comutador de camada 3 ou um hub, dependendo de seus requisitos e recursos. Se você implantar um switch, a opção deve ser capaz de encaminhamento de DHCP ou você deve colocar um servidor DHCP em cada sub-rede. Se você implantar um hub, você está implantando uma única sub-rede e não é necessário o encaminhamento de DHCP ou um segundo escopo no servidor DHCP.

##### <a name="static-tcpip-configurations"></a>Configurações de TCP/IP estáticas
Os servidores nesta implantação são configurados com endereços IPv4 estáticos. Computadores cliente são configurados por padrão para receber concessões de endereço IP do servidor DHCP.

##### <a name="active-directory-domain-services-global-catalog-and-dns-server-dc1"></a>Catálogo global do Active Directory serviços de domínio e o servidor DNS DC1
Tanto os serviços de domínio do Active Directory (AD DS) e sistema de nome de domínio (DNS) instalados no servidor, denominado DC1, que fornece o diretório e nomeie os serviços de resolução para todos os computadores e dispositivos na rede.

##### <a name="dhcp-server-dhcp1"></a>Servidor DHCP DHCP1
O servidor DHCP, denominado DHCP1, está configurado com um escopo que fornece concessões de endereço IP (Internet Protocol) para computadores na sub-rede local. O servidor DHCP também pode ser configurado com escopos adicionais para fornecer concessões de endereço IP a computadores em outras sub-redes se o encaminhamento de DHCP é configurado no roteadores.

##### <a name="client-computers"></a>Computadores cliente
Computadores que executam sistemas operacionais clientes Windows são configurados por padrão, como os clientes de DHCP, que obtém automaticamente opções de DHCP e endereços IP do servidor DHCP.

## <a name="BKMK_planning"></a>Planejamento de rede principal
Antes de implantar uma rede principal, você deve planejar os itens a seguir.

-   [Planejamento de sub-redes](#bkmk_NetFndtn_Pln_Subnt)

-   [Configuração básica de todos os servidores de planejamento](#bkmk_NetFndtn_Pln_AllSrvrs)

-   [Planejamento da implantação do DC1](#bkmk_NetFndtn_Pln_AD-DNS-01)

-   [Planejando o acesso ao domínio](#bkmk_NetFndtn_Pln_DomAccess)

-   [Planejar a implantação de DHCP1](#bkmk_NetFndtn_Pln_DHCP-01)

As seções a seguir fornecem mais detalhes sobre cada um desses itens.

> [!NOTE]
> Para obter ajuda com a planejar sua implantação, consulte também [Apêndice E - Core rede planejamento preparação folha](#BKMK_E).

### <a name="bkmk_NetFndtn_Pln_Subnt"></a>Planejamento de sub-redes
Em redes de protocolo/Internet (TCP), roteadores são usados para interconexão de hardware e software usados em segmentos de rede física diferentes chamados sub-redes. Roteadores também são usados para encaminhar pacotes IP entre cada uma das sub-redes. Determine o layout físico de sua rede, incluindo o número de roteadores e sub-redes, que você precisa, antes de prosseguir com as instruções neste guia.

Além disso, para configurar os servidores em sua rede com endereços IP estáticos, você deve determinar o intervalo de endereços IP que você deseja usar para a sub-rede onde estão localizados os servidores de rede principal. Neste guia, os intervalos de endereços IP privados 10.0.0.1 - 10.0.0.254 e 10.0.1.1 - 10.0.1.254 são usados como exemplos, mas você pode usar qualquer intervalo de endereços IP privado que preferir.

> [!IMPORTANT]
> Depois de selecionar os intervalos de endereços IP que você deseja usar para cada sub-rede, certifique-se de que você configure seu roteadores com um endereço IP do intervalo de endereços IP mesmo que o usado na sub-rede onde o roteador está instalado. Por exemplo, se o roteador está configurado por padrão com um endereço IP 192.168.1.1, mas você está instalando o roteador em uma sub-rede com um intervalo de endereços IP de 10.0.0.0/24, você deve reconfigurar o roteador para usar um endereço IP do intervalo de endereços IP 10.0.0.0/24.

O seguinte reconhecido endereço IP privado intervalos são especificados pelo Internet solicitar para Comments (RFC) 1918:

-   10.0.0.0 - 10.255.255.255

-   172.16.0.0 - 172.31.255.255

-   192.168.0.0 - 192.168.255.255

Quando você usa os intervalos de endereços IP privados conforme especificado na RFC 1918, você não consegue se conectar diretamente à Internet usando um endereço IP privado porque as solicitações de ou para esses endereços são automaticamente descartadas por roteadores de provedor de serviço de Internet. Para adicionar a conectividade com a Internet para sua rede principal mais tarde, você deve entrar em contato com um ISP para obter um endereço IP público.

> [!IMPORTANT]
> Ao usar endereços IP particulares, você deve usar algum tipo de servidor NAT (conversão) de endereço de proxy ou de rede para converter os intervalos de endereços IP privados em sua rede local para um endereço IP público que pode ser roteado na Internet. A maioria dos roteadores fornecer serviços NAT, portanto, selecionar um roteador é capaz de NAT deve ser bastante simple.

Para obter mais informações, consulte [planejamento da implantação do DHCP1](#bkmk_NetFndtn_Pln_DHCP-01).

### <a name="bkmk_NetFndtn_Pln_AllSrvrs"></a>Configuração básica de todos os servidores de planejamento
Para cada servidor na rede principal, você deve renomear o computador e atribuir e configurar um endereço IPv4 estático e outras propriedades de TCP/IP do computador.

#### <a name="planning-naming-conventions-for-computers-and-devices"></a>Planejando convenções de nomenclatura para computadores e dispositivos
Para manter a consistência entre sua rede, é uma boa ideia usar nomes consistentes para outros dispositivos, impressoras e servidores. Nomes de computador podem ser usados para ajudar os usuários e administradores identificam facilmente a finalidade e o local do servidor, impressora ou outro dispositivo. Por exemplo, se você tiver três servidores DNS, um em San Francisco, um em Los Angeles e um em Chicago, você pode usar a convenção de nomenclatura *função do servidor*-*local*-*número*:

-   SALA 01 DNS. Esse nome representa o servidor DNS em Denver, Colorado. Se a servidores DNS adicionais são adicionados na Denver, o valor numérico no nome pode ser incrementado, como DNS-sala-02 e DNS-sala-03.

-   DNS-SPAS-01. Esse nome representa o servidor DNS no Sul Pasadena, Califórnia.

-   DNS-ORL-01. Esse nome representa o servidor DNS em Orlando, Flórida.

Para este guia, a convenção de nomenclatura do servidor é muito simple e consiste a função de servidor primário e um número. Por exemplo, o controlador de domínio é denominado DC1, e o servidor DHCP DHCP1.

É recomendável que você escolher uma convenção de nomenclatura antes de instalar sua rede principal usando neste guia.

#### <a name="planning-static-ip-addresses"></a>Planejando endereços IP estáticos
Antes de configurar cada computador com um endereço IP estático, você deve planejar suas sub-redes e intervalos de endereço IP. Além disso, você deve determinar os endereços IP dos servidores DNS. Se você pretende instalar um roteador que fornece acesso a outras redes, como sub-redes adicionais ou à Internet, você deve saber o endereço IP do roteador, também chamado de um gateway padrão, para configuração de endereço IP estático.

A tabela a seguir fornece valores de exemplo para configuração de endereço IP estático.

|Itens de configuração|Valores de exemplo|
|-----------------------|------------------|
|Endereço IP|10.0.0.2|
|Máscara de sub-rede|255.255.255.0|
|Gateway padrão (endereço IP do roteador)|10.0.0.1|
|Servidor DNS preferencial|10.0.0.2|

> [!NOTE]
> Se você planeja implantar mais de um servidor DNS, você também pode planejar o endereço IP do servidor DNS alternativo.

### <a name="bkmk_NetFndtn_Pln_AD-DNS-01"></a>Planejamento da implantação do DC1
A seguir está as etapas de planejamento principais antes de instalar os serviços de domínio do Active Directory (AD DS) e DNS em DC1.

#### <a name="planning-the-name-of-the-forest-root-domain"></a>Planejando o nome do domínio raiz da floresta
Uma primeira etapa no processo de design do AD DS é determinar quantas florestas sua organização exige. Uma floresta é o contêiner nível superior do AD DS e consiste em um ou mais domínios que compartilham um esquema e um catálogo global. Uma organização pode ter várias florestas, mas para a maioria das organizações, um design de floresta única é o mais simples administrar e o modelo preferido.

Quando você cria o primeiro controlador de domínio em sua organização, você está criando o primeiro domínio (também chamado de domínio raiz da floresta) e a primeira floresta. No entanto, antes de tirar essa ação usando este guia, você deve determinar o melhor nome de domínio para sua organização. Na maioria dos casos, o nome da organização é usado como o nome de domínio e, em muitos casos, esse nome de domínio é registrado. Se você planeja implantar externa da face Internet com base em servidores Web para fornecer informações e serviços para seus clientes ou parceiros, escolha um nome de domínio que não ainda estiver em uso e, em seguida, registre o nome de domínio para que sua organização possui.

#### <a name="planning-the-forest-functional-level"></a>Planejando o nível funcional da floresta
Ao instalar o AD DS, você deve escolher o nível funcional da floresta que você deseja usar. Funcionalidade de floresta e domínio, introduzida no Windows Server 2003 Active Directory, fornece uma maneira de permitir domínio ou floresta toda recursos do Active Directory dentro do ambiente de rede. Diferentes níveis de funcionalidade do domínio e a funcionalidade de floresta estão disponíveis, dependendo do seu ambiente.

A funcionalidade de floresta habilita recursos em todos os domínios na floresta. Os seguintes níveis funcionais de floresta estão disponíveis:

-    Windows Server 2008. Esse nível funcional da floresta dá suporte somente controladores de domínio que executam o Windows Server 2008 e versões posteriores do sistema operacional Windows Server.

-    Windows Server 2008 R2. Esse nível funcional da floresta dá suporte a controladores de domínio do Windows Server 2008 R2 e controladores de domínio que executam versões posteriores do sistema operacional Windows Server.

-    Windows Server 2012. Esse nível funcional da floresta dá suporte a controladores de domínio do Windows Server 2012 e controladores de domínio que executam versões posteriores do sistema operacional Windows Server.

-    Windows Server 2012 R2. Esse nível funcional da floresta dá suporte a controladores de domínio do Windows Server 2012 R2 e controladores de domínio que executam versões posteriores do sistema operacional Windows Server.

-    Windows Server 2016. Esse nível funcional da floresta suporta apenas controladores de domínio do Windows Server 2016 e controladores de domínio que executam versões posteriores do sistema operacional Windows Server.

Se você estiver implantando um novo domínio em uma nova floresta e todos os controladores de domínio serão executado Windows Server 2016, é recomendável que você configura o AD DS com o nível funcional da floresta Windows Server 2016 durante a instalação do AD DS.

> [!IMPORTANT]
> Depois que o nível funcional da floresta é acionado, controladores de domínio que executam sistemas operacionais anteriores não podem ser introduzidos na floresta. Por exemplo, se você aumentar o nível funcional da floresta para o Windows Server 2016, controladores de domínio que executam o Windows Server 2012 R2 ou Windows Server 2008 não podem ser adicionados à floresta.

Itens de configuração de exemplo para o AD DS são fornecidas na tabela a seguir.

|Itens de configuração:|Valores de exemplo:|
|------------------------|-------------------|
|Nome completo do DNS|Exemplos:<br /><br />-corp.contoso.com<br />-example.com|
|Nível funcional da floresta|-Windows Server 2008 <br />-Windows Server 2008 R2 <br />-Windows Server 2012 <br />-Windows Server 2012 R2 <br />-Windows Server 2016|
|Local da pasta Active Directory banco de dados de serviços de domínio|E:\Configuration\\<br /><br />Ou aceite o local padrão.|
|Local da pasta de arquivos de Log de serviços de domínio do Active Directory|E:\Configuration\\<br /><br />Ou aceite o local padrão.|
|Local da pasta SYSVOL de serviços de domínio do diretório ativo|E:\Configuration\\<br /><br />Ou aceite o local padrão|
|Senha de administrador do modo de restauração de diretório|**J\ * p2leO4$ F**|
|Nome do arquivo de resposta (opcional)|**AD DS_AnswerFile**|

#### <a name="planning-dns-zones"></a>Planejando zonas DNS
Em servidores DNS primários, integrada do Active Directory, uma zona de pesquisa direta é criada por padrão durante a instalação da função de servidor DNS. Uma zona de pesquisa direta permite que os computadores e dispositivos para consultar endereço IP do dispositivo ou de outro computador, com base em seu nome DNS. Além de uma zona de pesquisa direta, é recomendável que você crie uma zona de pesquisa inversa DNS. Com um DNS pesquisa inversa consulta, um computador ou dispositivo pode descobrir o nome de outro computador ou dispositivo usando seu endereço IP. Implantando uma zona de pesquisa inversa normalmente melhora o desempenho de DNS e aumenta o sucesso das consultas do DNS.

Quando você cria uma zona de pesquisa inversa, o domínio in-addr.arpa, que é definido nos padrões DNS e reservado no namespace DNS de Internet para fornecer uma maneira prática e confiável para executar consultas reversa, é configurado no DNS. Para criar o namespace inverso, subdomínios dentro do domínio in-addr.arpa são formados, usando a ordem inversa dos números na notação de ponto decimal de endereços IP.

O domínio in-addr.arpa se aplica a todas as redes TCP/IP que são baseadas em protocolo IP versão 4 (IPv4) endereçamento. Assistente de nova zona automaticamente pressupõe que você está usando esse domínio quando você cria uma nova zona de pesquisa inversa.

Enquanto você estiver executando o Assistente de nova zona, as seleções a seguir são recomendadas:

|Itens de configuração|Valores de exemplo|
|-----------------------|------------------|
|Tipo de zona|**Zona primária**, e **armazenar a zona no Active Directory** é selecionado|
|Escopo de replicação de zona do Active Directory|**Para todos os servidores DNS neste domínio**|
|Página do Assistente de nome de zona de pesquisa do primeiro inversa|**Zona de pesquisa inversa IPv4**|
|Página do Assistente de nome de segundo Reverse Lookup Zone|ID de rede = 10.0.0.|
|Atualizações dinâmicas|**Permitir somente atualizações dinâmicas seguras**|

### <a name="bkmk_NetFndtn_Pln_DomAccess"></a>Planejando o acesso ao domínio
Para fazer logon no domínio, o computador deve ser um computador membro do domínio e a conta de usuário deve ser criada no AD DS antes da tentativa de logon.

> [!NOTE]
> Os computadores individuais que estão executando o Windows tem um local usuários e grupos do usuário da conta banco de dados que é chamado o banco de dados de contas de usuário do Gerenciador de contas de segurança (SAM). Quando você cria uma conta de usuário no computador local no banco de dados SAM, você pode fazer logon no computador local, mas você não pode fazer logon um domínio. Contas de usuário de domínio são criadas com o Active Directory Users e o Console de gerenciamento de computadores Microsoft (MMC) em um controlador de domínio, não com os usuários locais e grupos no computador local.

Após o primeiro logon bem-sucedido com credenciais de logon de domínio, as configurações de logon persistirem, a menos que o computador for removido do domínio ou as configurações de logon são alteradas manualmente.

Antes você fizer logon no domínio:

-   Crie contas de usuário no Active Directory Users and Computers. Cada usuário deve ter uma conta de usuário no Active Directory usuários e computadores do Active Directory Domain Services. Para obter mais informações, consulte [criar uma conta de usuário no Active Directory Users and Computers](#BKMK_createUA).

-   Certifique-se a configuração correta de endereço IP. Para ingressar em um computador ao domínio, o computador deve ter um endereço IP. Neste guia, os servidores são configurados com endereços IP estáticos e computadores cliente recebem concessões de endereço IP do servidor DHCP. Por esse motivo, o servidor DHCP deve ser implantado antes de adicionar clientes ao domínio. Para obter mais informações, consulte [Implantando DHCP1](#BKMK_deployDHCP01).

-   Ingresse o computador ao domínio. Qualquer computador que fornece ou acessa recursos de rede deve ter ingressado no domínio. Para obter mais informações, consulte [ingressar computadores de servidor para o domínio e o registro em log em](#BKMK_joinlogserver) e [ingressar computadores ao domínio e o registro em log em](#BKMK_joinlogclients).

### <a name="bkmk_NetFndtn_Pln_DHCP-01"></a>Planejar a implantação de DHCP1
A seguir está as etapas de planejamento chaves antes de instalar a função de servidor DHCP no DHCP1.

#### <a name="planning-dhcp-servers-and-dhcp-forwarding"></a>Planejamento servidores DHCP e o encaminhamento de DHCP
Como mensagens DHCP são mensagens de transmissão, eles não são encaminhados entre sub-redes por roteadores. Se você tiver várias sub-redes e fornecer o serviço DHCP para cada sub-rede, faça um destes procedimentos:

-   Instalar um servidor DHCP em cada sub-rede

-   Configure roteadores para encaminhar mensagens de transmissão de DHCP em sub-redes e configurar vários escopos no servidor DHCP, um escopo por sub-rede.

Na maioria dos casos, é mais econômico que a implantação de um servidor DHCP em cada segmento físico da rede configurar roteadores para encaminhar mensagens de transmissão de DHCP.

#### <a name="planning-ip-address-ranges"></a>Planejamento de intervalos de endereço IP
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

#### <a name="planning-subnet-masks"></a>Planejando máscaras de sub-rede
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

#### <a name="planning-exclusion-ranges"></a>Planejamento de intervalos de exclusão
Quando você cria um escopo em um servidor DHCP, você pode especificar um intervalo de endereços IP que inclui todos os endereços IP que o servidor DHCP tem permissão para serem concedidos aos clientes DHCP, como computadores e outros dispositivos. Se você vá e configurar manualmente alguns servidores e endereços IP de outros dispositivos com estático do mesmo intervalo de endereços IP que esteja usando o servidor DHCP, você pode acidentalmente criar um conflito de endereço IP, onde você e o servidor DHCP ambos atribuiu o mesmo endereço IP para dispositivos diferentes.

Para resolver esse problema, você pode criar um intervalo de exclusão para o escopo DHCP. Um intervalo de exclusão é um contíguos intervalo de endereços IP dentro do alcance de endereço IP do escopo que o servidor DHCP não tem permissão para usar. Se você criar um intervalo de exclusão, o servidor DHCP não atribui os endereços nesse intervalo, permitindo que você atribuir manualmente esses endereços sem criar um conflito de endereço IP.

Você pode excluir endereços IP da distribuição pelo servidor DHCP através da criação de um intervalo de exclusão para cada escopo. Você deve usar exclusões para todos os dispositivos que estejam configurados com um endereço IP estático. Os endereços excluídos devem incluir todos os endereços IP que você atribuiu manualmente a outros servidores, clientes não-DHCP, estações de trabalho sem disco ou clientes de roteamento e acesso remoto e PPP.

É recomendável que você configure seu intervalo de exclusão com endereços extras para acomodar o crescimento futuro de rede. A tabela a seguir fornece um intervalo de exclusão de exemplo para um escopo com um intervalo de endereços IP de 10.0.0.1 - 10.0.0.254 e uma máscara de sub-rede de 255.255.255.0.

|Itens de configuração|Valores de exemplo|
|-----------------------|------------------|
|Intervalo de exclusão Start IP Address|10.0.0.1|
|Endereço IP final do intervalo de exclusão|10.0.0.25|

#### <a name="planning-tcpip-static-configuration"></a>Planejar a configuração de TCP/IP estática
Determinados dispositivos, como roteadores, servidores DHCP e DNS servidores, devem ser configurados com um endereço IP estático. Além disso, você pode ter dispositivos adicionais, como impressoras, que você quer garantir que sempre têm o mesmo endereço IP. Listar os dispositivos que deseja configurar estaticamente para cada sub-rede e, em seguida, planeje o intervalo de exclusão que você deseja usar no servidor DHCP para garantir que o servidor DHCP não conceder o endereço IP de um dispositivo configurado estaticamente. Um intervalo de exclusão é uma sequência limitada de endereços IP dentro de um escopo, excluído das ofertas de serviço DHCP. Intervalos de exclusão asseguram que quaisquer endereços nesses intervalos não são oferecidos pelo servidor para clientes DHCP na sua rede.

Por exemplo, se o intervalo de endereços IP para uma sub-rede é 192.168.0.1 por meio de 192.168.0.254 e você tiver dez dispositivos que você deseja configurar com um endereço IP estático, você pode criar um intervalo de exclusão para o 192.168.0. *x* escopo que inclua dez ou mais endereços IP: 192.168.0.1 por meio de 192.168.0.15.

Neste exemplo, use dez os endereços IP excluídos configurar servidores e outros dispositivos com endereços IP estáticos e cinco endereços IP adicionais ficam disponíveis para configuração estática de novos dispositivos que deseja adicionar no futuro. Esta variedade de exclusão, o servidor DHCP for deixado com um pool de endereços do 192.168.0.16 por meio de 192.168.0.254.

Itens de configuração de exemplo adicional para o AD DS e DNS são fornecidas na tabela a seguir.

|Itens de configuração|Valores de exemplo|
|-----------------------|------------------|
|Associações de conexão de rede|Ethernet|
|Configurações do servidor DNS|DC1.corp.contoso.com|
|Endereço IP do servidor DNS preferencial|10.0.0.2|
|Adicionar os valores da caixa de diálogo escopo<br /><br />1. Escopo nome<br />2. A partir do endereço IP<br />3. Endereço IP final<br />4. Máscara de sub-rede<br />5. Padrão Gateway (opcional)<br />6. Duração da concessão|1. Principal sub-rede<br />2.  10.0.0.1<br />3.  10.0.0.254<br />4.  255.255.255.0<br />5.  10.0.0.1<br />6. 8 dias|
|Modo de operação do servidor DHCP IPv6|Não habilitado|

## <a name="BKMK_deployment"></a>Implantação de rede principal
Para implantar uma rede principal, as etapas básicas são:

1.  [Configurando a todos os servidores](#BKMK_configuringAll)

2.  [Implantando DC1](#BKMK_deployADDNS01)

3.  [Adicionando computadores de servidor ao domínio e fazer logon](#BKMK_joinlogserver)

4.  [Implantando DHCP1](#BKMK_deployDHCP01)

5.  [Adicionando computadores ao domínio e fazer logon](#BKMK_joinlogclients)

6.  [Implantar recursos opcionais para autenticação de acesso de rede e serviços da Web](#BKMK_optionalfeatures)

> [!NOTE]
> -   Comandos equivalentes do Windows PowerShell são fornecidos para a maioria dos procedimentos neste guia. Antes da execução desses cmdlets do Windows PowerShell, substitua os valores de exemplo com valores que são apropriados para a implantação de rede. Além disso, você deve inserir cada cmdlet em uma única linha no Windows PowerShell. Neste guia, cmdlets individuais podem aparecer em várias linhas devido a restrições e a exibição do documento de formatação por seu navegador ou outro aplicativo.
> -   Os procedimentos neste guia não inclua instruções para os casos em que o **User Account Control** abre a caixa de diálogo para solicitar sua permissão para continuar. Se essa caixa de diálogo Abrir enquanto você estiver executando os procedimentos neste guia e se a caixa de diálogo estava aberta em resposta a ações, clique em **continuar**.

### <a name="BKMK_configuringAll"></a>Configurando a todos os servidores
Antes de instalar outras tecnologias, como serviços de domínio do Active Directory ou o DHCP, é importante configurar os itens a seguir.

-   [Renomear o computador](#BKMK_rename)

-   [Configurar um endereço IP estático](#BKMK_ip)

Você pode usar as seções a seguir para executar as seguintes ações para cada servidor.

A associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para executar esses procedimentos.

#### <a name="BKMK_rename"></a>Renomear o computador
Você pode usar o procedimento nesta seção para alterar o nome de um computador. Renomear o computador é útil para situações em que o sistema operacional automaticamente criou um nome de computador que você não deseja usar.

> [!NOTE]
> Para executar este procedimento usando o Windows PowerShell, abra o PowerShell, digite os seguintes cmdlets em linhas separadas e pressione ENTER. Você também deve substituir *ComputerName* com o nome que você deseja usar.
>
> `Rename-Computer`*ComputerName*
>
> `Restart-Computer`

###### <a name="to-rename-computers-running-windows-server-2016-windows-server-2012-r2-and-windows-server-2012"></a>Para renomear computadores que executam o Windows Server 2012, Windows Server 2012 R2 e Windows Server 2016

1.  No Gerenciador do servidor, clique em **servidor Local**. O computador **propriedades** são exibidos no painel de detalhes.

2.  Em **propriedades**, na **nome do computador**, clique no nome de computador existente. O **propriedades do sistema** caixa de diálogo é aberta. Clique em **alteração**. O **alterações de nome/domínio do computador** caixa de diálogo é aberta.

3.  No **alterações de nome/domínio do computador** na caixa **nome do computador**, digite um novo nome para o seu computador. Por exemplo, se você quiser nomear o computador DC1, digite **DC1**.

4.  Clique em **Okey** duplicadas e, em seguida, clique em **fechar**. Se você deseja reiniciar o computador imediatamente para concluir a alteração do nome, clique em **reiniciar agora**. Caso contrário, clique em **reiniciar mais tarde**.

> [!NOTE]
> Para obter informações sobre como renomear computadores que executam outros sistemas operacionais da Microsoft, consulte [Apêndice A - renomeando computadores](#BKMK_A).

#### <a name="BKMK_ip"></a>Configurar um endereço IP estático
Você pode usar os procedimentos neste tópico para configurar o protocolo IP versão 4 (IPv4) lidar com propriedades de uma conexão de rede com um IP estático para computadores que executam o Windows Server 2016.

> [!NOTE]
> Para executar este procedimento usando o Windows PowerShell, abra o PowerShell, digite os seguintes cmdlets em linhas separadas e pressione ENTER. Você também deve substituir os nomes de interface e endereços IP neste exemplo com os valores que você deseja usar para configurar seu computador.
>
> `New-NetIPAddress -IPAddress 10.0.0.2 -InterfaceAlias "Ethernet" -DefaultGateway 10.0.0.1 -AddressFamily IPv4 -PrefixLength 24`
>
> `Set-DnsClientServerAddress -InterfaceAlias "Ethernet" -ServerAddresses 127.0.0.1`

###### <a name="to-configure-a-static-ip-address-on-computers-running-windows-server-2016-windows-server-2012-r2-and-windows-server-2012"></a>Para configurar um endereço IP estático em computadores que executam o Windows Server 2012, Windows Server 2012 R2 e Windows Server 2016

1.  Na barra de tarefas, clique com botão direito no ícone de rede e, em seguida, clique em **abrir Central de rede e compartilhamento**.

2.  Em **Central de rede e compartilhamento**, clique em **alterar as configurações do adaptador**. O **conexões de rede** pasta abre e exibe as conexões de rede disponíveis.

3.  Em **conexões de rede**, clique com botão direito a conexão que você deseja configurar e, em seguida, clique em **propriedades**. A conexão de rede **propriedades** caixa de diálogo é aberta.

4.  Na conexão de rede **propriedades** na caixa **esta conexão usa os seguintes itens**, selecione **Internet Protocol versão 4 (TCP/IPv4)**e clique em **propriedades**. O **propriedades de Internet/protocolo IP versão 4 (TCP/IPv4)** caixa de diálogo é aberta.

5.  Em **propriedades da Internet Protocol versão 4 (TCP/IPv4)**diante do **geral**, clique em **usar o seguinte endereço IP**. Em **endereço IP**, digite o endereço IP que você deseja usar.

6.  Pressione tab para colocar o cursor no **máscara de sub-rede**. Um valor padrão de máscara de sub-rede for inserido automaticamente. Aceite a máscara de sub-rede padrão ou digite a máscara de sub-rede que você deseja usar.

7.  Em **gateway padrão**, digite o endereço IP do seu gateway padrão.

    > [!NOTE]
    > Você deve configurar **gateway padrão** com o mesmo endereço IP que você usar na interface de rede local (LAN) do roteador. Por exemplo, se você tiver um roteador que esteja conectado a uma rede de longa distância (WAN), como a Internet, bem como para sua LAN, configurar a interface de LAN com o mesmo endereço IP, em seguida, você especifica como o **gateway padrão**. Em outro exemplo, se você tiver um roteador que esteja conectado a duas LANs, onde LAN A usa o 10.0.0.0 24 # endereço intervalo e LAN B usa o endereço de intervalo 192.168.0.0/24, configure o endereço IP do roteador LAN A com um endereço de intervalo de endereços, como 10.0.0.1. Além disso, o escopo DHCP para esse intervalo de endereços, configure **gateway padrão** com o endereço IP 10.0.0.1. Para B LAN, configurar a interface de roteador LAN B com um endereço de intervalo de endereços, como 192.168.0.1 e, em seguida, configure o LAN B escopo 192.168.0.0/24 com um **gateway padrão** valor de 192.168.0.1.

8.  Em **servidor DNS preferencial**, digite o endereço IP do seu servidor DNS. Se você pretende usar o computador local como o servidor DNS preferencial, digite o endereço IP do computador local.

9. Em **servidor DNS alternativo**, digite o endereço IP do seu servidor DNS alternativo, se houver. Se você pretende usar o computador local como um servidor DNS alternativo, digite o endereço IP do computador local.

10. Clique em **Okey**e clique em **fechar**.

> [!NOTE]
> Para obter informações sobre como configurar um endereço IP estático em computadores que executam outros sistemas operacionais da Microsoft, consulte [Apêndice B - Configurando endereços IP estáticos](#BKMK_B).

#### <a name="BKMK_deployADDNS01"></a>Implantando DC1
Para implantar DC1, que é o computador executando os serviços de domínio do Active Directory (AD DS) e DNS, você deve concluir estas etapas na seguinte ordem:

-   Siga as etapas na seção [configurando todos os servidores](#BKMK_configuringAll).

-   [Instalar o AD DS e DNS para uma nova floresta](#BKMK_installAD-DNS)

-   [Criar uma conta de usuário em computadores e usuários do Active Directory](#BKMK_createUA)

-   [Atribuir membros do grupo](#BKMK_assigngroup)

-   [Configurar uma zona de pesquisa inversa DNS](#BKMK_reverse)

**Privilégios administrativos**

Se você estiver instalando uma rede de pequena e é o único administrador para a rede, é recomendável que você cria uma conta de usuário para si mesmo e, em seguida, adicione sua conta de usuário como um membro de administradores corporativos e administradores de domínio. Isso tornará mais fácil para você atuar como o administrador para todos os recursos de rede. Também é recomendável que você faça logon com esta conta apenas quando você precisa executar tarefas administrativas e que você criar uma conta de usuário separada para a execução não são tarefas relacionadas.

Se você tiver uma organização maior com vários administradores, consulte a documentação do AD DS para determinar a melhor associação do grupo de funcionários da organização.

**Diferenças entre contas de usuário de domínio e contas de usuário no computador local**

Uma das vantagens de uma infraestrutura de domínio é que você não precisa criar contas de usuário em cada computador no domínio. Isso é verdadeiro se o computador é um computador cliente ou um servidor.

Por isso, você não deve criar contas de usuário em cada computador no domínio. Crie todas as contas de usuário no Active Directory Users and Computers e use os procedimentos anteriores para atribuir a associação ao grupo. Por padrão, todas as contas de usuário são membros do grupo usuários do domínio.

Todos os membros do grupo usuários do domínio podem fazer logon qualquer computador cliente depois que ele ingressar no domínio.

Você pode configurar contas de usuário para designar os dias e horários que o usuário tem permissão para fazer logon no computador. Você também pode designar os computadores de cada usuário tem permissão para usar. Para configurar essas configurações, abrir e computadores do Active Directory Users, localize a conta de usuário que você deseja configurar e clique duas vezes na conta. Na conta de usuário **propriedades**, clique no **conta** guia e, em seguida, clique em **horas de Logon** ou **Log para**.

#### <a name="BKMK_installAD-DNS"></a>Instalar o AD DS e DNS para uma nova floresta

Você pode usar um dos procedimentos a seguir para instalar os serviços de domínio do Active Directory (AD DS) e DNS e para criar um novo domínio em uma nova floresta. 

O primeiro procedimento fornece instruções sobre como executar essas ações usando o Windows PowerShell, enquanto o segundo procedimento mostra como instalar o AD DS e DNS usando o Gerenciador do servidor.

>[!IMPORTANT]
>Após concluir as etapas neste procedimento, o computador é reiniciado automaticamente.

**Instalar o AD DS e DNS usando o Windows PowerShell**

Você pode usar os seguintes comandos para instalar e configurar o AD DS e DNS. Você deve substituir o nome do domínio neste exemplo com o valor que você deseja usar para seu domínio.

>[!NOTE]
>Para obter mais informações sobre esses comandos do Windows PowerShell, consulte os seguintes tópicos de referência.
>- [Install-WindowsFeature](https://technet.microsoft.com/itpro/powershell/windows/server-manager/install-windowsfeature)
>- [Install-ADDSForest](https://technet.microsoft.com/itpro/powershell/windows/adds/deployment/install-addsforest)

A associação ao grupo **administradores** é o requisito mínimo para executar este procedimento.

- Execute o Windows PowerShell como administrador, digite o seguinte comando e pressione ENTER:  

`Install-WindowsFeature AD-Domain-Services -IncludeManagementTools`

Quando a instalação foi concluída com êxito, a seguinte mensagem é exibida no Windows PowerShell.

    
    Success Restart Needed  Exit Code   Feature Result
    ------- --------------  ---------   --------------
    True    No              Success     {Active Directory Domain Services, Group P...
    

- No Windows PowerShell, digite o comando a seguir, substituindo o texto **corp.contoso.com** com seu nome de domínio e pressione ENTER:

````
Install-ADDSForest -DomainName "corp.contoso.com"
````

- Durante o processo de instalação e configuração, que é visível na parte superior da janela do Windows PowerShell, a seguinte solicitação é exibida. Depois que for exibida, digite uma senha e pressione ENTER.

    **SafeModeAdministratorPassword:**

- Depois que você digite uma senha e pressione ENTER, a seguinte solicitação de confirmação é exibida. Digite a mesma senha e pressione ENTER.

    **Confirme SafeModeAdministratorPassword:**

- Quando a seguinte solicitação for exibida, digite a letra **Y** e pressione ENTER.

    
    O servidor de destino será configurado como um controlador de domínio e reiniciado quando essa operação for concluída.
    Você quer continuar com essa operação?
    [Y] [A] Sim Sim para todos os [N] Nenhum [L] não para todos os [S] suspender [?] Ajuda (o padrão é "Y"):
    
- Se você quiser, você pode ler as mensagens de aviso que são exibidas durante a instalação normal, bem-sucedida do AD DS e DNS. Essas mensagens são normais e não são uma indicação de falha de instalação.

- Após a instalação for bem-sucedida, aparece uma mensagem informando que você está prestes a ser desconectado do computador para que possa reiniciar o computador. Se você clicar em **fechar**, você está conectado imediatamente logoff do computador e o computador for reiniciado. Se você não clicar **fechar**, o computador é reiniciado após um período de padrão de tempo.

- Depois que o servidor for reiniciado, você pode verificar a instalação bem-sucedida do Active Directory Domain Services e DNS. Abra o Windows PowerShell, digite o seguinte comando e pressione ENTER.

````
Get-WindowsFeature
````

Os resultados desse comando são exibidos no Windows PowerShell e devem ser parecidos com os resultados na imagem abaixo. Para as tecnologias instaladas, os colchetes à esquerda do nome da tecnologia contenham o caractere **X**e o valor de **estado de instalação** é **instalado**.

![Resultados do comando Get-WindowsFeature](../media/Core-Network-Guide/server-roles-installed.jpg)

**Instalar o AD DS e DNS usando o Gerenciador do servidor**

1.  Em DC1, em **Gerenciador do servidor**, clique em **gerenciar**e clique em **adicionar funções e recursos**. Abre o assistente Adicionar funções e recursos.

2.  Em **Before You Begin**, clique em **próxima**.

    > [!NOTE]
    > O **Before You Begin** página do Assistente de recursos de adição de funções e não é exibida se você tiver selecionado anteriormente **ignorar esta página por padrão** quando a adição de funções e recursos de assistente foi executado.

3.  Em **selecione tipo de instalação**, certifique-se de que **instalação baseada em função ou recurso baseado** está selecionado e clique em **próxima**.

4.  Em **servidor de destino Select**, certifique-se de que **selecionar um servidor do pool servidor** é selecionado. Em **Pool de servidores**, verifique se o computador local está selecionado. Clique em **próxima**.

5.  Em **selecionar funções de servidor**, na **funções**, clique em **Active Directory Domain Services**. Em **adicionar recursos que são necessários para os serviços de domínio do Active Directory**, clique em **adicionar recursos**. Clique em **próxima**.

6.  Em **Selecione recursos**, clique em **próxima**e em **Active Directory Domain Services**, examine as informações que são fornecidas e, em seguida, clique em **próxima**.

7.  Em **confirmar seleções de instalação**, clique em **instalar**. Página Installation progress exibe o status durante o processo de instalação. Quando o processo for concluído, os detalhes de mensagem, clique em **promover esse servidor para um controlador de domínio**. Abre o Assistente de configuração de serviços de domínio Active Directory.

8.  Em **implantação configuração**, selecione **adicionar uma nova floresta**. Em **nome de domínio raiz**, digite o nome de domínio totalmente qualificado (FQDN) do seu domínio. Por exemplo, se o FQDN corp.contoso.com, digite **corp.contoso.com**. Clique em **próxima**.

9. Em **opções de controlador de domínio**, na **selecione nível funcional da nova floresta e domínio raiz**, selecione o nível funcional da floresta e o nível funcional do domínio que você deseja usar. Em **especificar as funcionalidades de controlador de domínio**, certifique-se de que **servidor de sistema de nome de domínio (DNS)** e **Catálogo Global (GC)** são selecionados. Em **senha** e **Confirmar senha**, digite a senha do modo de restauração de serviços de diretório (DSRM) que você deseja usar. Clique em **próxima**.

10. Em **DNS opções**, clique em **próxima**.

11. Em **opções adicionais**, verifique o nome NetBIOS que é atribuído ao domínio e alterá-lo se necessário. Clique em **próxima**.

12. Em **caminhos**, na **especificar o local do banco de dados do AD DS, arquivos de log e SYSVOL**, siga um destes procedimentos:

    -   Aceite os valores padrão.

    -   Digite locais de pasta que você deseja usar para **pasta banco de dados**, **pasta de arquivos de Log**, e **pasta SYSVOL**.

13. Clique em **próxima**.

14. Em **revisar opções**, revise suas seleções.

15. Se você quiser exportar as configurações para um script do Windows PowerShell, clique em **exibir script**. O script abre no bloco de notas, e você pode salvá-lo para o local da pasta que você deseja. Clique em **próxima**. Em **pré-requisitos verificar**, suas seleções são validadas. Quando a verificação é concluída, clique em **instalar**. Quando solicitado pelo Windows, clique em **fechar**. O servidor for reiniciado para concluir a instalação do AD DS e DNS.

16. Para verificar a instalação bem-sucedida, exiba o console do Gerenciador do servidor depois que o servidor for reiniciado. AD DS e DNS devem aparecer no painel à esquerda, como os itens realçados na imagem abaixo.

![AD DS e no Gerenciador do servidor DNS](../media/Core-Network-Guide/server-roles-installed-sm.jpg)

##### <a name="BKMK_createUA"></a>Criar uma conta de usuário em computadores e usuários do Active Directory
Você pode usar este procedimento para criar uma nova conta de usuário de domínio no Active Directory Users e no Console de gerenciamento de computadores Microsoft (MMC).

A associação ao grupo **Admins. do domínio**, ou equivalente, é o requisito mínimo para executar este procedimento.

> [!NOTE]
> Para executar este procedimento usando o Windows PowerShell, abra o PowerShell e digite o seguinte cmdlet em uma única linha e pressione ENTER. Você também deve substituir o nome da conta de usuário neste exemplo com o valor que você deseja usar.
>
> `New-ADUser -SamAccountName User1 -AccountPassword (read-host "Set user password" -assecurestring) -name "User1" -enabled $true -PasswordNeverExpires $true -ChangePasswordAtLogon $false`
>
> Depois que você pressiona ENTER, digite a senha da conta de usuário. A conta é criada e, por padrão, é concedida associação ao grupo usuários de domínio.
>
> Com o seguinte cmdlet, é possível atribuir participações adicionais para a nova conta de usuário. O exemplo a seguir adiciona User1 aos grupos de administradores de domínio e administradores corporativos. Certifique-se antes de executar esse comando que você altere o nome da conta de usuário, nome de domínio e grupos para atender às suas necessidades.
>
> `Add-ADPrincipalGroupMembership -Identity "CN=User1,CN=Users,DC=corp,DC=contoso,DC=com" -MemberOf "CN=Enterprise Admins,CN=Users,DC=corp,DC=contoso,DC=com","CN=Domain Admins,CN=Users,DC=corp,DC=contoso,DC=com"`

###### <a name="to-create-a-user-account"></a>Para criar uma conta de usuário

1.  Em DC1, no Gerenciador do servidor, clique em **ferramentas**e clique em **usuários e Active Directory computadores**. Abre o Active Directory MMC Usuários e computadores. Se não ainda estiver selecionada, clique no nó do seu domínio. Por exemplo, se o domínio for corp.contoso.com, clique em **corp.contoso.com**.

2.  No painel de detalhes, clique com botão direito na pasta em que você deseja adicionar uma conta de usuário.

    **Onde?**

    -   Diretório de usuários e computadores do Active /*nó domínio*/*pasta*

3.  Aponte para **nova**e clique em **usuário**. O **New Object - User** caixa de diálogo é aberta.

4.  Em **nome**, digite o nome do usuário.

5.  Em **iniciais**, digite iniciais do usuário.

6.  Em **Sobrenome**, digite o sobrenome do usuário.

7.  Modificar **nome completo** para adicionar iniciais ou inverter a ordem dos nomes e sobrenomes.

8.  Em **nome de logon do usuário**, digite o nome de logon do usuário. Clique em **próxima**.

9. Em **New Object - User**, na **senha** e **Confirmar senha**, digite a senha do usuário e, em seguida, selecione as opções de senha apropriada.

10. Clique em **próxima**, examine as novas configurações de conta de usuário e, em seguida, clique em **concluir**.

##### <a name="BKMK_assigngroup"></a>Atribuir membros do grupo
Você pode usar este procedimento para adicionar um usuário, computador ou grupo a um grupo de usuários e Active Directory computadores Microsoft Management Console (MMC).

A associação ao grupo **Admins. do domínio**, ou equivalente é o requisito mínimo para executar este procedimento.

###### <a name="to-assign-group-membership"></a>Para atribuir a associação de grupo

1.  Em DC1, no Gerenciador do servidor, clique em **ferramentas**e clique em **usuários e Active Directory computadores**. Abre o Active Directory MMC Usuários e computadores. Se não ainda estiver selecionada, clique no nó do seu domínio. Por exemplo, se o domínio for corp.contoso.com, clique em **corp.contoso.com**.

2.  No painel de detalhes, clique duas vezes na pasta que contém o grupo ao qual você deseja adicionar um membro.

    Onde?

    -   **Diretório de usuários e computadores do Active**/*nó domínio*/*pasta que contém o grupo*

3.  No painel de detalhes, clique com botão direito do objeto que você deseja adicionar a um grupo, como um usuário ou computador e clique em **propriedades**. O objeto **propriedades** caixa de diálogo é aberta. Clique no **membro do** guia.

4.  Sobre o **membro do**, clique em **adicionar**.

5.  Em **digite os nomes de objeto para selecionar**, digite o nome do grupo ao qual você deseja adicionar o objeto e, em seguida, clique em **Okey**.

6.  Para atribuir a associação de grupo para outros usuários, grupos ou computadores, repita as etapas 4 e 5 deste procedimento.

##### <a name="BKMK_reverse"></a>Configurar uma zona de pesquisa inversa DNS
Você pode usar este procedimento para configurar uma zona de pesquisa inversa no sistema de nome de domínio (DNS).

A associação ao grupo **Admins. do domínio** é o requisito mínimo para executar este procedimento.

> [!NOTE]
> -   Para organizações médias e grandes, é recomendável que você configure e usa o grupo DNSAdmins no Active Directory Users and Computers. Para obter mais informações, consulte [recursos técnicos adicionais](#BKMK_resources)
> -   Para executar este procedimento usando o Windows PowerShell, abra o PowerShell e digite o seguinte cmdlet em uma única linha e pressione ENTER. Você também deve substituir os nomes de zona e zonefile da pesquisa inversa de DNS neste exemplo com os valores que você deseja usar. Certifique-se de que você reverter a identificação de rede para o nome de zona inversa. Por exemplo, se a ID de rede for 192.168.0, crie o nome de zona de pesquisa inversa **0.168.192.in in-addr**.
>
> `Add-DnsServerPrimaryZone 0.0.10.in-addr.arpa -ZoneFile 0.0.10.in-addr.arpa.dns`

###### <a name="to-configure-a-dns-reverse-lookup-zone"></a>Para definir uma zona de pesquisa inversa DNS

1.  Em DC1, no Gerenciador do servidor, clique em **ferramentas**e clique em **DNS**. Abre o DNS MMC.

2.  No DNS, se ele já não estiver expandido, clique duas vezes o nome do servidor para expandir a árvore. Por exemplo, se o nome do servidor DNS é DC1, clique duas vezes em **DC1**.

3.  Selecione **zonas de pesquisa inversa**, clique com botão direito **zonas de pesquisa inversa**e clique em **nova zona**. Assistente de nova zona é aberta.

4.  Em **bem-vindo ao Assistente de nova zona**, clique em **próxima**.

5.  Em **tipo de zona**, selecione **zona primária**.

6.  Se o servidor DNS é um controlador de domínio gravável, certifique-se de que **armazenar a zona no Active Directory** é selecionado. Clique em **próxima**.

7.  Em **o escopo de replicação de zona do Active Directory**, selecione **para todos os servidores DNS funcionando em controladores de domínio nesse domínio**, a menos que você tenha um motivo específico para escolher uma opção diferente. Clique em **próxima**.

8.  No primeiro **nome de zona de pesquisa inversa** página, selecione **zona de pesquisa reversa IPv4**. Clique em **próxima**.

9. Na segunda **nome de zona de pesquisa inversa** página, siga um destes procedimentos:

    -   Em **ID de rede**, digite a ID de rede do seu intervalo de endereços IP. Por exemplo, se o intervalo de endereços IP 10.0.0.1 por meio de 10.0.0.254, digite **10.0.0**.

    -   Em **nome de zona de pesquisa inversa**, seu nome de zona de pesquisa inversa IPv4 é automaticamente adicionada. Clique em **próxima**.

10. Em **atualização dinâmica**, selecione o tipo de atualizações dinâmicas que você deseja permitir. Clique em **próxima**.

11. Em **concluir o Assistente de nova zona**, reveja suas opções e, em seguida, clique em **concluir**.

#### <a name="BKMK_joinlogserver"></a>Adicionando computadores de servidor ao domínio e fazer logon
Depois que você tiver instalado os serviços de domínio do Active Directory (AD DS) e criou uma ou mais contas de usuário que tem permissões para ingressar um computador ao domínio, você pode ingressar servidores de rede principal para o domínio e os servidores de logon para instalar tecnologias adicionais, como o DHCP Dynamic Host Configuration Protocol ().

Em todos os servidores que você está implantando, exceto para o servidor que executa o AD DS, faça o seguinte:

1.  Concluir os procedimentos fornecidos no [configurando todos os servidores](#BKMK_configuringAll).

2.  Use as instruções nos dois procedimentos a seguir para ingressar em seus servidores no domínio e para fazer logon em servidores para executar tarefas de implantação adicionais:

> [!NOTE]
> Para executar este procedimento usando o Windows PowerShell, abra o PowerShell, digite o seguinte cmdlet e pressione ENTER. Você também deve substituir o nome de domínio com o nome que você deseja usar.
>
> `Add-Computer -DomainName corp.contoso.com`
>
> Quando você for solicitado a fazer isso, digite o nome de usuário e senha para uma conta que tenha permissão para adicionar um computador ao domínio. Para reiniciar o computador, digite o seguinte comando e pressione ENTER.
>
> `Restart-Computer`

###### <a name="to-join-computers-running--windows-server-2016--windows-server-2012-r2--and--windows-server-2012--to-the-domain"></a>Para participar de computadores que executam o Windows Server 2012, Windows Server 2012 R2 e Windows Server 2016 ao domínio

1.  No Gerenciador do servidor, clique em **servidor Local**. No painel de detalhes, clique em **WORKGROUP**. O **propriedades do sistema** caixa de diálogo é aberta.

2.  No **propriedades do sistema** caixa de diálogo, clique em **alteração**. O **alterações de nome/domínio do computador** caixa de diálogo é aberta.

3.  Em **nome do computador**, na **membro do**, clique em **domínio**e, em seguida, digite o nome do domínio que você deseja ingressar. Por exemplo, se o nome de domínio for corp.contoso.com, digite **corp.contoso.com**.

4.  Clique em **Okey**. O **segurança do Windows** caixa de diálogo é aberta.

5.  Em **alterações de nome/domínio do computador**, na **nome de usuário**, digite o nome de usuário e em **senha**, digite a senha e clique em **Okey**. O **alterações de nome/domínio do computador** abre a caixa de diálogo, boas-vindas ao domínio. Clique em **Okey**.

6.  O **alterações de nome/domínio do computador** caixa de diálogo exibe uma mensagem indicando que você deve reiniciar o computador para aplicar as alterações. Clique em **Okey**.

7.  Sobre o **propriedades do sistema** caixa de diálogo, no **nome do computador**, clique em **fechar**. O **Microsoft Windows** caixa de diálogo abre e exibe uma mensagem, novamente, indicando que você deve reiniciar o computador para aplicar as alterações. Clique em **reiniciar agora**.

> [!NOTE]
> Para obter informações sobre como ingressar computadores que executam outros sistemas operacionais da Microsoft ao domínio, consulte [Apêndice C - ingressar computadores ao domínio](#BKMK_C).

###### <a name="to-log-on-to-the-domain-using-computers-running-windows-server-2016"></a>Para fazer logon no domínio usando computadores que executam o Windows Server 2016

1.  Faça logoff do computador ou reiniciá-lo.

2.  Pressione CTRL + ALT + DELETE. A tela de logon é exibida.

3.  No canto inferior esquerdo, clique em **outro usuário**.

4.  Em **nome de usuário**, digite seu nome de usuário.

5.  Em **senha**, digite sua senha de domínio e, em seguida, clique na seta ou pressione ENTER.

> [!NOTE]
> Para obter informações sobre como fazer logon no domínio usando computadores que executam outros sistemas operacionais da Microsoft, consulte [Apêndice D - logon no domínio](#BKMK_D).

#### <a name="BKMK_deployDHCP01"></a>Implantando DHCP1
Antes de implantar esse componente da rede principal, você deve fazer o seguinte:

-   Siga as etapas na seção [configurando todos os servidores](#BKMK_configuringAll).

-   Siga as etapas na seção [ingressar computadores de servidor para o domínio e o registro em log em](#BKMK_joinlogserver).

Para implantar DHCP1, que é o computador executa a função de servidor Dynamic Host Configuration Protocol (DHCP), você deve concluir estas etapas na seguinte ordem:

-   [Instalar o protocolo DHCP (protocolo DHCP)](#BKMK_installDHCP)

-   [Criar e ativar um novo escopo DHCP](#BKMK_newscopeDHCP)

> [!NOTE]
> Para executar esses procedimentos usando o Windows PowerShell, abra o PowerShell, digite os seguintes cmdlets em linhas separadas e pressione ENTER. Você também deve substituir o nome do escopo, tela inicial de endereço IP e intervalos de fim, máscara de sub-rede e outros valores neste exemplo com os valores que você deseja usar.
>
> `Install-WindowsFeature DHCP -IncludeManagementTools`
>
> `Add-DhcpServerv4Scope -name "Corpnet" -StartRange 10.0.0.1 -EndRange 10.0.0.254 -SubnetMask 255.255.255.0 -State Active`
>
> `Add-DhcpServerv4ExclusionRange -ScopeID 10.0.0.0 -StartRange 10.0.0.1 -EndRange 10.0.0.15`
>
> `Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.0.1 -ScopeID 10.0.0.0 -ComputerName DHCP1.corp.contoso.com`
>
> `Add-DhcpServerv4Scope -name "Corpnet2" -StartRange 10.0.1.1 -EndRange 10.0.1.254 -SubnetMask 255.255.255.0 -State Active`
>
> `Add-DhcpServerv4ExclusionRange -ScopeID 10.0.1.0 -StartRange 10.0.1.1 -EndRange 10.0.1.15`
>
> `Set-DhcpServerv4OptionValue -OptionID 3 -Value 10.0.1.1 -ScopeID 10.0.1.0 -ComputerName DHCP1.corp.contoso.com`
>
> `Set-DhcpServerv4OptionValue -DnsDomain corp.contoso.com -DnsServer 10.0.0.2`
>
> `Add-DhcpServerInDC -DnsName DHCP1.corp.contoso.com`

##### <a name="BKMK_installDHCP"></a>Instalar o protocolo DHCP (protocolo DHCP)
Você pode usar este procedimento para instalar e configurar a função de servidor DHCP usando a adição de funções e recursos do assistente.

A associação ao grupo **Admins. do domínio**, ou equivalente, é o requisito mínimo para executar este procedimento.

###### <a name="to-install-dhcp"></a>Para instalar o DHCP

1.  No DHCP1, no Gerenciador do servidor, clique em **gerenciar**e clique em **adicionar funções e recursos**. Abre o assistente Adicionar funções e recursos.

2.  Em **Before You Begin**, clique em **próxima**.

    > [!NOTE]
    > O **Before You Begin** página do Assistente de recursos de adição de funções e não é exibida se você tiver selecionado anteriormente **ignorar esta página por padrão** quando a adição de funções e recursos de assistente foi executado.

3.  Em **selecione tipo de instalação**, certifique-se de que **instalação baseada em função ou recurso baseado** está selecionado e clique em **próxima**.

4.  Em **servidor de destino Select**, certifique-se de que **selecionar um servidor do pool servidor** é selecionado. Em **Pool de servidores**, verifique se o computador local está selecionado. Clique em **próxima**.

5.  Em **selecionar funções de servidor**, na **funções**, selecione **servidor DHCP**. Em **adicionar recursos que são necessários para o servidor DHCP**, clique em **adicionar recursos**. Clique em **próxima**.

6.  Em **Selecione recursos**, clique em **próxima**e em **servidor DHCP**, examine as informações que são fornecidas e, em seguida, clique em **próxima**.

7.  Em **confirmar seleções de instalação**, clique em **reiniciar o servidor de destino automaticamente, se necessário**. Quando você for solicitado a confirmar a seleção, clique em **Sim**e clique em **instalar**. O **progresso da instalação** página exibe o status durante o processo de instalação. Quando o processo for concluído, a mensagem "configuração necessária. Instalação foi bem-sucedida no *ComputerName*"é exibido, onde *ComputerName* é o nome do computador no qual você instalou o servidor DHCP. Na janela de mensagem, clique em **configuração DHCP completa**. O Assistente de configuração DHCP pós-Post-Install é aberta. Clique em **próxima**.

8.  Em **autorização**, especifique as credenciais que você deseja usar para autorizar o servidor DHCP nos serviços de domínio do Active Directory e clique em **confirmar**. Após a conclusão da autorização, clique em **fechar**.

##### <a name="BKMK_newscopeDHCP"></a>Criar e ativar um novo escopo DHCP
Você pode usar este procedimento para criar um novo escopo DHCP usando o Console de gerenciamento Microsoft (MMC) de DHCP. Quando você concluir o procedimento, o escopo é ativado e o intervalo de exclusão que você criar impede que o servidor DHCP arrendar os endereços IP que você usa para definir estaticamente seus servidores e outros dispositivos que requerem um endereço IP estático.

A associação ao grupo **Administradores DHCP**, ou equivalente, é o requisito mínimo para executar este procedimento.

###### <a name="to-create-and-activate-a-new-dhcp-scope"></a>Para criar e ativar um novo escopo DHCP

1.  No DHCP1, no Gerenciador do servidor, clique em **ferramentas**e clique em **DHCP**. Abre o MMC DHCP.

2.  Em **DHCP**, expanda o nome do servidor. Por exemplo, se o nome do servidor DHCP for DHCP1.corp.contoso.com, clique na seta para baixo ao lado de **DHCP1.corp.contoso.com**.

3.  Abaixo do nome do servidor, clique com botão direito **IPv4**e clique em **novo escopo**. Abre o Assistente para criar novo escopo.

4.  Em **bem-vindo ao Assistente para nova escopo**, clique em **próxima**.

5.  Em **escopo nome**, na **nome**, digite um nome para o escopo. Por exemplo, digite **sub-rede 1**.

6.  Em **descrição**, digite uma descrição para o novo escopo e, em seguida, clique em **próxima**.

7.  Em **intervalo de endereços IP**, faça o seguinte:

    1.  Em **endereço IP inicial**, digite o endereço IP que é o primeiro endereço IP no intervalo. Por exemplo, digite **10.0.0.1**.

    2.  Em **endereço IP final**, digite o endereço IP que é o último endereço IP no intervalo. Por exemplo, digite **10.0.0.254**. Valores **comprimento** e **máscara de sub-rede** são inseridos automaticamente, com base no endereço IP que você inseriu para **endereço IP inicial**.

    3.  Se necessário, modifique os valores no **comprimento** ou **máscara de sub-rede**, conforme apropriado para seu esquema de endereçamento.

    4.  Clique em **próxima**.

8.  Em **adicionar exclusões**, faça o seguinte:

    1.  Em **endereço IP inicial**, digite o endereço IP que é o primeiro endereço IP no intervalo de exclusão. Por exemplo, digite **10.0.0.1**.

    2.  Em **endereço IP final**, tipo de lidar com o endereço IP que é o último IP no intervalo de exclusão, por exemplo, digite **10.0.0.15**.

9. Clique em **adicionar**e clique em **próxima**.

10. Em **duração da concessão**, modificar os valores padrão para **dias**, **horas**, e **minutos**, conforme apropriado para a rede e, em seguida, clique em **próxima**.

11. Em **configurar opções de DHCP**, selecione **Sim, quero configurar essas opções agora**e clique em **próxima**.

12. Em **roteador (Gateway padrão)**, siga um destes procedimentos:

    -   Se você não tiver roteadores em sua rede, clique em **próxima**.

    -   Em **endereço IP**, digite o endereço IP do roteador ou gateway padrão. Por exemplo, digite **10.0.0.1**. Clique em **adicionar**e clique em **próxima**.

13. Em **nome de domínio e servidores DNS**, faça o seguinte:

    1.  Em **domínio pai**, digite o nome do domínio DNS que os clientes usam para resolução de nomes. Por exemplo, digite **corp.contoso.com**.

    2.  Em **nome do servidor**, digite o nome do computador DNS que os clientes usam para resolução de nomes. Por exemplo, digite **DC1**.

    3.  Clique em **resolver**. O endereço IP do servidor DNS é adicionado na **endereço IP**. Clique em **adicionar**, aguarde a validação de endereço IP de servidor DNS concluir e clique em **próxima**.

14. Em **servidores WINS**, pois você não tiver servidores WINS em sua rede, clique em **próxima**.

15. Em **Ativar escopo**, selecione **Sim, quero ativar este escopo agora**.

16. Clique em **próxima**e clique em **concluir**.

> [!IMPORTANT]
> Para criar novos escopos de sub-redes adicionais, repita este procedimento. Use um intervalo de endereços IP diferente para cada sub-rede que você pretende implantar e verifique se o encaminhamento de mensagens DHCP está habilitado em todos os roteadores que levam para outras sub-redes.

### <a name="BKMK_joinlogclients"></a>Adicionando computadores ao domínio e fazer logon

> [!NOTE]
> Para executar este procedimento usando o Windows PowerShell, abra o PowerShell, digite o seguinte cmdlet e pressione ENTER. Você também deve substituir o nome de domínio com o nome que você deseja usar.
>
> `Add-Computer -DomainName corp.contoso.com`
>
> Quando você for solicitado a fazer isso, digite o nome de usuário e senha para uma conta que tenha permissão para adicionar um computador ao domínio. Para reiniciar o computador, digite o seguinte comando e pressione ENTER.
>
> `Restart-Computer`

##### <a name="to-join-computers-running-windows-10-to-the-domain"></a>Para participar de computadores que executam o Windows 10 no domínio

1.  Faça logon no computador com a conta de administrador local.

2.  Em **pesquisar a web e o Windows**, tipo **sistema**. Nos resultados da pesquisa, clique em **sistema (painel de controle)**. O **sistema** caixa de diálogo é aberta.

3.  Em **sistema**, clique em **configurações avançadas do sistema**. O **propriedades do sistema** caixa de diálogo é aberta. Clique no **nome do computador** guia.

4.  Em **nome do computador**, clique em **alteração**. O **alterações de nome/domínio do computador** caixa de diálogo é aberta.

5.  Em **alterações de nome/domínio do computador**, na **membro do**, clique em **domínio**e, em seguida, digite o nome do domínio que você deseja ingressar. Por exemplo, se o nome de domínio for corp.contoso.com, digite **corp.contoso.com**.

6.  Clique em **Okey**. O **segurança do Windows** caixa de diálogo é aberta.

7.  Em **alterações de nome/domínio do computador**, na **nome de usuário**, digite o nome de usuário e em **senha**, digite a senha e clique em **Okey**. O **alterações de nome/domínio do computador** abre a caixa de diálogo, boas-vindas ao domínio. Clique em **Okey**.

8.  O **alterações de nome/domínio do computador** caixa de diálogo exibe uma mensagem indicando que você deve reiniciar o computador para aplicar as alterações. Clique em **Okey**.

9. Sobre o **propriedades do sistema** caixa de diálogo, no **nome do computador**, clique em **fechar**. O **Microsoft Windows** caixa de diálogo abre e exibe uma mensagem, novamente, indicando que você deve reiniciar o computador para aplicar as alterações. Clique em **reiniciar agora**.

##### <a name="to-join-computers-running-windows-81-to-the-domain"></a>Para participar de computadores que executam o Windows 8.1 no domínio

1.  Faça logon no computador com a conta de administrador local.

2.  Clique com botão direito **iniciar**e clique em **sistema**. O **sistema** caixa de diálogo é aberta.

3.  Em **sistema**, clique em **configurações avançadas do sistema**. O **propriedades do sistema** caixa de diálogo é aberta. Clique no **nome do computador** guia.

4.  Em **nome do computador**, clique em **alteração**. O **alterações de nome/domínio do computador** caixa de diálogo é aberta.

5.  Em **alterações de nome/domínio do computador**, na **membro do**, clique em **domínio**e, em seguida, digite o nome do domínio que você deseja ingressar. Por exemplo, se o nome de domínio for corp.contoso.com, digite **corp.contoso.com**.

6.  Clique em **Okey**. O **segurança do Windows** caixa de diálogo é aberta.

7.  Em **alterações de nome/domínio do computador**, na **nome de usuário**, digite o nome de usuário e em **senha**, digite a senha e clique em **Okey**. O **alterações de nome/domínio do computador** abre a caixa de diálogo, boas-vindas ao domínio. Clique em **Okey**.

8.  O **alterações de nome/domínio do computador** caixa de diálogo exibe uma mensagem indicando que você deve reiniciar o computador para aplicar as alterações. Clique em **Okey**.

9. Sobre o **propriedades do sistema** caixa de diálogo, no **nome do computador**, clique em **fechar**. O **Microsoft Windows** caixa de diálogo abre e exibe uma mensagem, novamente, indicando que você deve reiniciar o computador para aplicar as alterações. Clique em **reiniciar agora**.

##### <a name="to-log-on-to-the-domain-using-computers-running-windows-10"></a>Para fazer logon no domínio usando computadores que executam o Windows 10

1.  Faça logoff do computador ou reiniciá-lo.

2.  Pressione CTRL + ALT + DELETE. A tela de logon é exibida.

3.  No canto inferior esquerdo, clique em **outro usuário**.

4.  Em **nome de usuário**, digite seu nome de domínio e do usuário no formato *domínio \ usuário*. Por exemplo, para fazer logon no domínio corp.contoso.com com uma conta chamada **usuário-01**, tipo **CORP\User-01**.

5.  Em **senha**, digite sua senha de domínio e, em seguida, clique na seta ou pressione ENTER.

### <a name="BKMK_optionalfeatures"></a>Implantar recursos opcionais para autenticação de acesso de rede e serviços da Web
Se você pretende implantar servidores de acesso à rede, como pontos de acesso sem fio ou servidores VPN, depois de instalar sua rede principal, é recomendável que você implantar um servidor NPS e um servidor Web. Para implantações de acesso de rede, é recomendável o uso dos métodos de autenticação segura baseada em certificado. Você pode usar o NPS para gerenciar políticas de acesso de rede e implementar os métodos de autenticação segura. Você pode usar um servidor Web para publicar a lista de certificados revogados (CRL) da sua autoridade de certificação (CA) que fornece certificados para autenticação segura.

> [!NOTE]
> Você pode implantar certificados de servidor e outros recursos adicionais usando guias de complementar de rede principal. Para obter mais informações, consulte [recursos técnicos adicionais](#BKMK_resources).

A ilustração a seguir mostra a topologia de rede do Windows Server Core com os servidores NPS e Web adicionados.

![Topologia de rede do Windows Server Core com os servidores NPS e Web adicionados](../media/Core-Network-Guide/cng16_overview_2.jpg)

As seções a seguir fornecem informações sobre como adicionar servidores NPS e da Web à sua rede.

-   [Implantando NPS1](#BKMK_deployNPS1)

-   [Implantando WEB1](#BKMK_IIS)

#### <a name="BKMK_deployNPS1"></a>Implantando NPS1
O servidor de servidor de política de rede (NPS) é instalado como uma etapa preparatórias para implantar a outras tecnologias de acesso de rede, como servidores de rede virtual privada (VPN), pontos de acesso sem fio e comutadores de autenticação 802.1 X.

Servidor de política de rede (NPS) permite que você centralmente configurar e gerenciar políticas de rede com os seguintes recursos: remoto Authentication Dial-In User Service (RADIUS) servidor e proxy RADIUS.

NPS é um componente opcional de uma rede principal, mas você deve instalar NPS se qualquer uma das opções a seguir for verdadeira:

-   Você pretende expandir sua rede para incluir os servidores de acesso remoto que são compatíveis com o protocolo RADIUS, como um computador que executa o Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008 e serviço de roteamento e acesso remoto, Gateway de serviços de Terminal ou Gateway de área de trabalho remota.


-   Você pretende implantar autenticação 802.1 X com fio ou acesso sem fio.

Antes de implantar esse serviço de função, você deve executar as etapas a seguir no computador que você estiver configurando como um servidor NPS.

-   Siga as etapas na seção [configurando todos os servidores](#BKMK_configuringAll).

-   Siga as etapas na seção [ingressar computadores de servidor para o domínio e o registro em log no](#BKMK_joinlogserver)

Para implantar NPS1, que é o computador executando o serviço de função de servidor de política de rede (NPS) da função de servidor Serviços de acesso e política de rede, você deve concluir essa etapa:

-   [Planejar a implantação de NPS1](#bkmk_NetFndtn_Pln_NPS-01)

-   [Instalar o servidor de política de rede (NPS)](#BKMK_installNPS)

-   [Registrar o servidor NPS no domínio padrão](#BKMK_registerNPS)

> [!NOTE]
> Este guia fornece instruções para implantar o NPS em um servidor autônomo ou VM denominado NPS1.  Outro modelo de implantação recomendada é a instalação do NPS em um controlador de domínio. Se você preferir instalando NPS em um controlador de domínio, em vez de em um servidor autônomo, instale o NPS no DC1.

##### <a name="bkmk_NetFndtn_Pln_NPS-01"></a>Planejar a implantação de NPS1
Se você pretende implantar servidores de acesso à rede, como pontos de acesso sem fio ou servidores VPN, depois de implantar sua rede principal, é recomendável que você implanta o NPS.

Quando você usa o NPS como um servidor remoto Authentication Dial-In User Service (RADIUS), o NPS realiza autenticação e autorização para solicitações de conexão por meio de seus servidores de acesso de rede. NPS também permite que você configure e gerencie políticas de rede centralmente que determinar quem pode acessar a rede, como eles podem acessar a rede e quando eles podem acessar a rede.

A seguir está as etapas de planejamento chaves antes de instalar o NPS.

- Planeje o banco de dados de contas de usuário. Por padrão, se você ingressar o servidor NPS em um domínio do Active Directory, NPS realiza autenticação e autorização usando o banco de dados de contas de usuário do AD DS. Em alguns casos, como com redes grandes que usam NPS como um proxy RADIUS para encaminhar as solicitações de conexão para outros servidores RADIUS, convém instalar NPS em um computador não membro do domínio.

- Planeje estatísticas RADIUS. NPS permite que você registre os dados contábeis para um banco de dados do SQL Server ou para um arquivo de texto no computador local. Se você quiser usar o log do SQL Server, planeje a instalação e configuração do servidor que executa o SQL Server.

##### <a name="BKMK_installNPS"></a>Instalar o servidor de política de rede (NPS)
Você pode usar este procedimento para instalar o servidor de política de rede (NPS) usando a adição de funções e recursos do assistente. NPS é um serviço de função da função de servidor Serviços de acesso e política de rede.

> [!NOTE]
> Por padrão, o NPS escuta tráfego RADIUS nas portas 1812, 1813, 1645 e 1646 em todos os adaptadores de rede instalados. Se o Firewall do Windows com segurança avançada é habilitado quando você instala o NPS, exceções de firewall para estas portas são criadas automaticamente durante o processo de instalação para o protocolo IP versão 6 \(IPv6\) e tráfego IPv4. Caso os servidores de acesso de rede estejam configurados para enviar tráfego RADIUS através de portas que não sejam esses padrões, remova as exceções criadas no Firewall do Windows com segurança avançada durante a instalação de NPS e crie exceções para as portas que você usa para o tráfego de RAIO.

**Credenciais administrativas**

Para concluir este procedimento, você deve ser um membro do **Admins. do domínio** grupo.

> [!NOTE]
> Para executar este procedimento usando o Windows PowerShell, abra o PowerShell e digite o seguinte e pressione ENTER.
>
> `Install-WindowsFeature NPAS -IncludeManagementTools`

###### <a name="to-install-nps"></a>Para instalar o NPS

1.  No NPS1, no Gerenciador do servidor, clique em **gerenciar**e clique em **adicionar funções e recursos**. Abre o assistente Adicionar funções e recursos.

2.  Em **Before You Begin**, clique em **próxima**.

    > [!NOTE]
    > O **Before You Begin** página do Assistente de recursos de adição de funções e não é exibida se você tiver selecionado anteriormente **ignorar esta página por padrão** quando a adição de funções e recursos de assistente foi executado.

3.  Em **selecione tipo de instalação**, certifique-se de que **instalação baseada em função ou recurso baseado** está selecionado e clique em **próxima**.

4.  Em **servidor de destino Select**, certifique-se de que **selecionar um servidor do pool servidor** é selecionado. Em **Pool de servidores**, verifique se o computador local está selecionado. Clique em **próxima**.

5.  Em **selecionar funções de servidor**, na **funções**, selecione **serviços de acesso e política de rede**. É aberta uma caixa de diálogo perguntando se ele deve adicionar recursos que são necessários para serviços de acesso e política de rede. Clique em **adicionar recursos**e clique em **próxima**.

6.  Em **Selecione recursos**, clique em **próxima**e em **serviços de acesso e política de rede**, examine as informações que são fornecidas e, em seguida, clique em **próxima**.

7.  Em **selecione Serviços de função**, clique em **NPS**.  Em **adicionar recursos que são necessários para o servidor de políticas de rede**, clique em **adicionar recursos**. Clique em **próxima**.

8.  Em **confirmar seleções de instalação**, clique em **reiniciar o servidor de destino automaticamente, se necessário**. Quando você for solicitado a confirmar a seleção, clique em **Sim**e clique em **instalar**. Página Installation progress exibe o status durante o processo de instalação. Quando o processo for concluído, a mensagem "instalação foi bem-sucedida no *ComputerName*" é exibido, onde *ComputerName* é o nome do computador no qual você instalou o servidor de políticas de rede. Clique em **fechar**.

##### <a name="BKMK_registerNPS"></a>Registrar o servidor NPS no domínio padrão
Você pode usar este procedimento para registrar um servidor NPS no domínio em que o servidor é um membro do domínio.

Os servidores NPS devem ser registrados no Active Directory para que ele tenha permissão para ler as propriedades de discagem rápida de contas de usuário durante o processo de autorização. Registrar um servidor NPS adiciona o servidor para o **servidores RAS e IAS** grupo no Active Directory.

**Credenciais administrativas**

Para concluir este procedimento, você deve ser um membro do **Admins. do domínio** grupo.

> [!NOTE]
> Para executar este procedimento usando comandos de shell (Netsh) de rede no Windows PowerShell, abra o PowerShell e digite o seguinte e pressione ENTER.
>
> `netsh nps add registeredserver domain=corp.contoso.com server=NPS1.corp.contoso.com`

###### <a name="to-register-an-nps-server-in-its-default-domain"></a>Para registrar um servidor NPS no domínio padrão

1.  Em NPS1, no Gerenciador do servidor, clique em ferramentas e, em seguida, clique em **NPS**. O MMC do servidor de política de rede é aberta.

2.  Clique com botão direito **NPS (Local)**e clique em **servidor Register no Active Directory**. O **NPS** caixa de diálogo é aberta.

3.  Em **NPS**, clique em **Okey**e clique em **Okey** novamente.

Para obter mais informações sobre o servidor de políticas de rede, consulte [servidor de política de rede (NPS)](../technologies/nps/nps-top.md).

#### <a name="BKMK_IIS"></a>Implantando WEB1

A função Web Server (IIS) no Windows Server 2016 fornece uma plataforma segura, fácil de gerenciar, modular e extensível para hospedar confiavelmente sites, serviços e aplicativos. Com o Internet Information Services (IIS), você pode compartilhar informações com usuários na Internet, uma intranet ou uma extranet. IIS é uma plataforma web unificado que se integra IIS, ASP.NET, serviços FTP, PHP e Windows Communication Foundation (WCF).

Além de permitir que você publicar uma CRL para acesso por computadores membros do domínio, a função de servidor de Web Server (IIS) permite que você configure e gerencie vários sites, aplicativos web e sites FTP. O IIS também oferece os seguintes benefícios:

-   Maximize a segurança na web por meio de isolamento de aplicativo de impressão e automática de pés um servidor reduzido.

-   Facilmente implantar e executar ASP.NET, ASP clássica e PHP de aplicativos no mesmo servidor da web.

-   Obter o isolamento de aplicativos fornecendo processos uma identidade exclusiva e configuração em área restrita por padrão, reduzindo os riscos de segurança.

-   Facilmente adicionar, remover e até mesmo substituir componentes internos do IIS com módulos personalizados, adequados às necessidades dos clientes.

-   Acelere seu site por meio de cache dinâmico interno e compactação avançada.

Para implantar WEB1, que é o computador que esteja executando a função de servidor de Web Server (IIS), você deve fazer o seguinte:

-   Siga as etapas na seção [configurando todos os servidores](#BKMK_configuringAll).

-   Siga as etapas na seção [ingressar computadores de servidor para o domínio e o registro em log no](#BKMK_joinlogserver)

-   [Instalar a função de servidor de Web Server (IIS)](#BKMK_install_IIS)

##### <a name="BKMK_install_IIS"></a>Instalar a função de servidor de Web Server (IIS)
Para concluir este procedimento, você deve ser um membro do **administradores** grupo.

> [!NOTE]
> Para executar este procedimento usando o Windows PowerShell, abra o PowerShell e digite o seguinte e pressione ENTER.
>
> `Install-WindowsFeature Web-Server -IncludeManagementTools`

1.  Em **Gerenciador do servidor**, clique em **gerenciar**e clique em **adicionar funções e recursos**. Abre o assistente Adicionar funções e recursos.

2.  Em **Before You Begin**, clique em **próxima**.

    > [!NOTE]
    > O **Before You Begin** página do Assistente de recursos de adição de funções e não é exibida se você tiver selecionado anteriormente **ignorar esta página por padrão** quando a adição de funções e recursos de assistente foi executado.

3.  Sobre o **selecione tipo de instalação** página, clique em **próxima**.

4.  No **servidor de destino Select** página, certifique-se de que o computador local está selecionado e clique em **próxima**.

5.  Sobre o **selecionar funções de servidor** de página, role e selecione **Web Server (IIS)**. O **adicionar recursos que são necessários para Web Server (IIS)** caixa de diálogo é aberta. Clique em **adicionar recursos**e clique em **próxima**.

6.  Clique em **próxima** até que você aceitou todos padrão configurações de servidor web e, em seguida, clique em **instalar**.

7.  Verificar se todas as instalações foram bem-sucedidas e clique em **fechar**.

## <a name="BKMK_resources"></a>Recursos técnicos adicionais
Para obter mais informações sobre as tecnologias neste guia, consulte os seguintes recursos:

 Recursos de biblioteca técnica do Windows Server 2012, Windows Server 2012 R2 e Windows Server 2016

-   [O que há de novo nos serviços de domínio Active Directory (AD DS) no Windows Server 2016](https://technet.microsoft.com/en-us/library/mt163897.aspx)

-   [Active Directory Visão geral dos serviços de domínio](https://technet.microsoft.com/library/hh831484.aspx) em https://technet.microsoft.com/library/hh831484.aspx.

-   [Visão geral do domínio nome DNS (sistema)](https://technet.microsoft.com/library/hh831667.aspx) em https://technet.microsoft.com/library/hh831667.aspx.

-   [Implementando a função de administradores DNS](https://technet.microsoft.com/library/cc756152(WS.10).aspx)

-   [Visão geral de protocolo DHCP (Host Configuration) dinâmica](https://technet.microsoft.com/library/hh831825.aspx) em https://technet.microsoft.com/library/hh831825.aspx.

-   [Visão geral dos serviços de acesso e política de rede](https://technet.microsoft.com/library/hh831683.aspx) em https://technet.microsoft.com/library/hh831683.aspx.

-   [Visão geral da Web Server (IIS)](https://technet.microsoft.com/library/hh831725.aspx) em https://technet.microsoft.com/library/hh831725.aspx.

## <a name="BKMK_appendix"></a>Apêndices A por meio eletrônico
As seções a seguir contêm informações de configuração adicionais para computadores que executam sistemas operacionais que não sejam Windows 8, Windows 10, Windows Server 2012 e Windows Server 2016. Além disso, uma planilha de preparação de rede é fornecida para ajudá-lo com sua implantação.

1.  [Apêndice A - renomeando computadores](#BKMK_A)

2.  [Apêndice B - Configurando IP estático aborda](#BKMK_B)

3.  [Apêndice C - ingressar computadores no domínio](#BKMK_C)

4.  [Apêndice D - logon no domínio](#BKMK_D)

5.  [Apêndice E - rede principal folha de preparação de planejamento](#BKMK_E)

## <a name="BKMK_A"></a>Apêndice A - renomeando computadores
Você pode usar os procedimentos desta seção para fornecer os computadores que executam o Windows Server 2008 R2, Windows 7, Windows Server 2008 e Windows Vista com um nome de outro computador.

-   [Windows Server 2008 R2 e Windows 7](#bkmk_NetFndtn_Pln_rename_R2)

-   [Windows Server 2008 e Windows Vista](#bkmk_NetFndtn_Pln_Renam08)

### <a name="bkmk_NetFndtn_Pln_rename_R2"></a>Windows Server 2008 R2 e Windows 7
A associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para executar esses procedimentos.

##### <a name="to-rename-computers-running-windows-server-2008-r2-and-windows-7"></a>Para renomear computadores que executam o Windows Server 2008 R2 e Windows 7

1.  Clique em **iniciar**, clique com botão direito **computador**e clique em **propriedades**. O **sistema** caixa de diálogo é aberta.

2.  Em **configurações de grupo de trabalho, domínio e nome de computador**, clique em **alterar configurações**. O **propriedades do sistema** caixa de diálogo é aberta.

    > [!NOTE]
    > Em computadores executando o Windows 7, antes do **propriedades do sistema** abre a caixa de diálogo, o **User Account Control** abre a caixa de diálogo, solicitando permissão para continuar. Clique em **continuar** para prosseguir.

3.  Clique em **alteração**. O **alterações de nome/domínio do computador** caixa de diálogo é aberta.

4.  Em **nome do computador**, digite o nome do computador. Por exemplo, se você quiser nomear o computador DC1, digite **DC1**.

5.  Clique em **Okey** duplicadas, clique em **fechar**e clique em **reiniciar agora** para reiniciar o computador.

### <a name="bkmk_NetFndtn_Pln_Renam08"></a>Windows Server 2008 e Windows Vista
A associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para executar esses procedimentos.

##### <a name="to-rename-computers-running-windows-server-2008-and-windows-vista"></a>Para renomear computadores que executam o Windows Server 2008 e Windows Vista

1.  Clique em **iniciar**, clique com botão direito **computador**e clique em **propriedades**. O **sistema** caixa de diálogo é aberta.

2.  Em **configurações de grupo de trabalho, domínio e nome de computador**, clique em **alterar configurações**. O **propriedades do sistema** caixa de diálogo é aberta.

    > [!NOTE]
    > Em computadores que executam o Windows Vista, antes do **propriedades do sistema** abre a caixa de diálogo, o **User Account Control** abre a caixa de diálogo, solicitando permissão para continuar. Clique em **continuar** para prosseguir.

3.  Clique em **alteração**. O **alterações de nome/domínio do computador** caixa de diálogo é aberta.

4.  Em **nome do computador**, digite o nome do computador. Por exemplo, se você quiser nomear o computador DC1, digite **DC1**.

5.  Clique em **Okey** duplicadas, clique em **fechar**e clique em **reiniciar agora** para reiniciar o computador.

## <a name="BKMK_B"></a>Apêndice B - Configurando IP estático aborda
Este tópico contém procedimentos para configurar os endereços IP estáticos em computadores que executam os seguintes sistemas operacionais:

-   [Windows Server 2008 R2](#bkmk_R2Cng_WS08R2IP)

-   [Windows Server 2008](#bkmk_NetFndtn_Pln_CfgStatic08)

### <a name="bkmk_R2Cng_WS08R2IP"></a>Windows Server 2008 R2
A associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para executar este procedimento.

##### <a name="to-configure-a-static-ip-address-on-a-computer-running-windows-server-2008-r2"></a>Para configurar um endereço IP estático em um computador executando o Windows Server 2008 R2

1.  Clique em **iniciar**e clique em **painel de controle**.

2.  Em **painel de controle**, clique em **de rede e Internet**. **Rede e Internet** é aberta.

    Em **de rede e Internet**, clique em **Central de rede e compartilhamento**. **Central de rede e compartilhamento** é aberta.

3.  Em **Central de rede e compartilhamento**, clique em **alterar as configurações do adaptador**. **Conexões de rede** é aberta.

4.  Em **conexões de rede**, clique com botão direito a conexão de rede que você deseja configurar e, em seguida, clique em **propriedades**.

5.  Em **propriedades de Conexão de área Local**, na **esta conexão usa os seguintes itens**, selecione **Internet Protocol versão 4 (TCP/IPv4)**e clique em **propriedades**. O **propriedades de Internet/protocolo IP versão 4 (TCP/IPv4)** caixa de diálogo é aberta.

6.  Em **propriedades da Internet Protocol versão 4 (TCP/IPv4)**diante do **geral**, clique em **usar o seguinte endereço IP**. Em **endereço IP**, digite o endereço IP que você deseja usar.

7.  Pressione tab para colocar o cursor no **máscara de sub-rede**. Um valor padrão de máscara de sub-rede for inserido automaticamente. Aceite a máscara de sub-rede padrão ou digite a máscara de sub-rede que você deseja usar.

8.  Em **gateway padrão**, digite o endereço IP do seu gateway padrão.

9. Em **servidor DNS preferencial**, digite o endereço IP do seu servidor DNS. Se você pretende usar o computador local como o servidor DNS preferencial, digite o endereço IP do computador local.

10. Em **servidor DNS alternativo**, digite o endereço IP do seu servidor DNS alternativo, se houver. Se você pretende usar o computador local como um servidor DNS alternativo, digite o endereço IP do computador local.

11. Clique em **Okey**e clique em **fechar**.

### <a name="bkmk_NetFndtn_Pln_CfgStatic08"></a>Windows Server 2008
A associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para executar esses procedimentos.

##### <a name="to-configure-a-static-ip-address-on-a-computer-running-windows-server-2008"></a>Para configurar um endereço IP estático em um computador executando o Windows Server 2008

1.  Clique em **iniciar**e clique em **painel de controle**.

2.  Em **painel de controle**, verifique **modo de exibição clássico** está selecionado e clique duas vezes em **Central de rede e compartilhamento**.

3.  Em **Central de rede e compartilhamento**, na **tarefas**, clique em **gerenciar conexões de rede**.

4.  Em **conexões de rede**, clique com botão direito a conexão de rede que você deseja configurar e, em seguida, clique em **propriedades**.

5.  Em **propriedades de Conexão de área Local**, na **esta conexão usa os seguintes itens**, selecione **Internet Protocol versão 4 (TCP/IPv4)**e clique em **propriedades**. O **propriedades de Internet/protocolo IP versão 4 (TCP/IPv4)** caixa de diálogo é aberta.

6.  Em **propriedades da Internet Protocol versão 4 (TCP/IPv4)**diante do **geral**, clique em **usar o seguinte endereço IP**. Em **endereço IP**, digite o endereço IP que você deseja usar.

7.  Pressione tab para colocar o cursor no **máscara de sub-rede**. Um valor padrão de máscara de sub-rede for inserido automaticamente. Aceite a máscara de sub-rede padrão ou digite a máscara de sub-rede que você deseja usar.

8.  Em **gateway padrão**, digite o endereço IP do seu gateway padrão.

9. Em **servidor DNS preferencial**, digite o endereço IP do seu servidor DNS. Se você pretende usar o computador local como o servidor DNS preferencial, digite o endereço IP do computador local.

10. Em **servidor DNS alternativo**, digite o endereço IP do seu servidor DNS alternativo, se houver. Se você pretende usar o computador local como um servidor DNS alternativo, digite o endereço IP do computador local.

11. Clique em **Okey**e clique em **fechar**.

## <a name="BKMK_C"></a>Apêndice C - ingressar computadores no domínio
Você pode usar esses procedimentos para ingressar em computadores que executam o Windows Server 2008 R2, Windows 7, Windows Server 2008 e Windows Vista ao domínio.

-   [Windows Server 2008 R2 e Windows 7](#BKMK_c1)

-   [Windows Server 2008 e Windows Vista](#BKMK_c2)

> [!IMPORTANT]
> Para ingressar um computador em um domínio, você deve estar conectado ao computador com a conta de administrador local ou, se você estiver conectado ao computador com uma conta de usuário que não tenha credenciais administrativas do computador local, você deve fornecer as credenciais da conta de administrador local durante o processo de ingressar o computador ao domínio. Além disso, você deve ter uma conta de usuário no domínio ao qual você deseja ingressar o computador. Durante o processo de ingressar o computador ao domínio, você será solicitado para suas credenciais de conta de domínio (nome de usuário e senha).

### <a name="BKMK_c1"></a>Windows Server 2008 R2 e Windows 7
A associação ao grupo **usuários do domínio**, ou equivalente, é o requisito mínimo para executar este procedimento.

##### <a name="to-join-computers-running-windows-server-2008-r2-and-windows-7-to-the-domain"></a>Para participar de computadores que executam o Windows Server 2008 R2 e Windows 7 ao domínio

1.  Faça logon no computador com a conta de administrador local.

2.  Clique em **iniciar**, clique com botão direito **computador**e clique em **propriedades**. O **sistema** caixa de diálogo é aberta.

3.  Em **configurações de grupo de trabalho, domínio e nome de computador**, clique em **alterar configurações**. O **propriedades do sistema** caixa de diálogo é aberta.

    > [!NOTE]
    > Em computadores executando o Windows 7, antes do **propriedades do sistema** abre a caixa de diálogo, o **User Account Control** abre a caixa de diálogo, solicitando permissão para continuar. Clique em **continuar** para prosseguir.

4.  Clique em **alteração**. O **alterações de nome/domínio do computador** caixa de diálogo é aberta.

5.  Em **nome do computador**, na **membro do**, selecione **domínio**e, em seguida, digite o nome do domínio que você deseja ingressar. Por exemplo, se o nome de domínio for corp.contoso.com, digite **corp.contoso.com**.

6.  Clique em **Okey**. O **segurança do Windows** caixa de diálogo é aberta.

7.  Em **alterações de nome/domínio do computador**, na **nome de usuário**, digite o nome de usuário e em **senha**, digite a senha e clique em **Okey**. O **alterações de nome/domínio do computador** abre a caixa de diálogo, boas-vindas ao domínio. Clique em **Okey**.

8.  O **alterações de nome/domínio do computador** caixa de diálogo exibe uma mensagem indicando que você deve reiniciar o computador para aplicar as alterações. Clique em **Okey**.

9. Sobre o **propriedades do sistema** caixa de diálogo, no **nome do computador**, clique em **fechar**. O **Microsoft Windows** caixa de diálogo abre e exibe uma mensagem, novamente, indicando que você deve reiniciar o computador para aplicar as alterações. Clique em **reiniciar agora**.

### <a name="BKMK_c2"></a>Windows Server 2008 e Windows Vista
A associação ao grupo **usuários do domínio**, ou equivalente, é o requisito mínimo para executar este procedimento.

##### <a name="to-join-computers-running-windows-server-2008-and-windows-vista-to-the-domain"></a>Para participar de computadores que executam o Windows Server 2008 e Windows Vista ao domínio

1.  Faça logon no computador com a conta de administrador local.

2.  Clique em **iniciar**, clique com botão direito **computador**e clique em **propriedades**. O **sistema** caixa de diálogo é aberta.

3.  Em **configurações de grupo de trabalho, domínio e nome de computador**, clique em **alterar configurações**. O **propriedades do sistema** caixa de diálogo é aberta.

4.  Clique em **alteração**. O **alterações de nome/domínio do computador** caixa de diálogo é aberta.

5.  Em **nome do computador**, na **membro do**, selecione **domínio**e, em seguida, digite o nome do domínio que você deseja ingressar. Por exemplo, se o nome de domínio for corp.contoso.com, digite **corp.contoso.com**.

6.  Clique em **Okey**. O **segurança do Windows** caixa de diálogo é aberta.

7.  Em **alterações de nome/domínio do computador**, na **nome de usuário**, digite o nome de usuário e em **senha**, digite a senha e clique em **Okey**. O **alterações de nome/domínio do computador** abre a caixa de diálogo, boas-vindas ao domínio. Clique em **Okey**.

8.  O **alterações de nome/domínio do computador** caixa de diálogo exibe uma mensagem indicando que você deve reiniciar o computador para aplicar as alterações. Clique em **Okey**.

9. Sobre o **propriedades do sistema** caixa de diálogo, no **nome do computador**, clique em **fechar**. O **Microsoft Windows** caixa de diálogo abre e exibe uma mensagem, novamente, indicando que você deve reiniciar o computador para aplicar as alterações. Clique em **reiniciar agora**.

## <a name="BKMK_D"></a>Apêndice D - logon no domínio
Você pode usar esses procedimentos para fazer logon no domínio usando computadores que executam o Windows Server 2008 R2, Windows 7, Windows Server 2008 e Windows Vista.

-   [Windows Server 2008 R2 e Windows 7](#BKMK_d1)

-   [Windows Server 2008 e Windows Vista](#BKMK_d2)

### <a name="BKMK_d1"></a>Windows Server 2008 R2 e Windows 7
A associação ao grupo **usuários do domínio**, ou equivalente, é o requisito mínimo para executar este procedimento.

##### <a name="log-on-to-the-domain-using-computers-running-windows-server-2008-r2-and-windows-7"></a>Faça logon no domínio usando computadores que executam o Windows Server 2008 R2 e Windows 7

1.  Faça logoff do computador ou reiniciá-lo.

2.  Pressione CTRL + ALT + DELETE. A tela de logon é exibida.

3.  Clique em **Alternar usuário**e clique em **outro usuário**.

4.  Em **nome de usuário**, digite seu nome de domínio e do usuário no formato *domínio \ usuário*. Por exemplo, para fazer logon no domínio corp.contoso.com com uma conta chamada **usuário-01**, tipo **CORP\User-01**.

5.  Em **senha**, digite sua senha de domínio e, em seguida, clique na seta ou pressione ENTER.

### <a name="BKMK_d2"></a>Windows Server 2008 e Windows Vista
A associação ao grupo **usuários do domínio**, ou equivalente, é o requisito mínimo para executar este procedimento.

##### <a name="log-on-to-the-domain-using-computers-running-windows-server-2008-and-windows-vista"></a>Faça logon no domínio usando computadores que executam o Windows Server 2008 e Windows Vista

1.  Faça logoff do computador ou reiniciá-lo.

2.  Pressione CTRL + ALT + DELETE. A tela de logon é exibida.

3.  Clique em **Alternar usuário**e clique em **outro usuário**.

4.  Em **nome de usuário**, digite seu nome de domínio e do usuário no formato *domínio \ usuário*. Por exemplo, para fazer logon no domínio corp.contoso.com com uma conta chamada **usuário-01**, tipo **CORP\User-01**.

5.  Em **senha**, digite sua senha de domínio e, em seguida, clique na seta ou pressione ENTER.

## <a name="BKMK_E"></a>Apêndice E - rede principal folha de preparação de planejamento
Você pode usar essa folha de preparação de planejamento de rede para coletar as informações necessárias para instalar uma rede principal. Este tópico apresenta tabelas que contêm os itens de configuração individual para cada computador do servidor para o qual você deve fornecer informações ou valores específicos durante o processo de instalação ou configuração. Valores de exemplo são fornecidos para cada item de configuração.

Para planejar e fins de controle, espaços são fornecidos em cada tabela para que você insira os valores usados para a implantação. Se você fizer logon valores relacionados à segurança nessas tabelas, você deve armazenar as informações em um local seguro.

Os links a seguir levam as seções deste tópico que fornecem itens de configuração e valores de exemplo que estão associados com os procedimentos de implantação apresentados neste guia.

1.  [Instalar o DNS e serviços de domínio do Active Directory](#BKMK_FndtnPrep_InstallAD)

    -   [Configurando uma zona de pesquisa inversa DNS](#BKMK_FndtnPrep_DNSRevrsLook)

2.  [Instalar o DHCP](#BKMK_FndtnPrep_InstallDHCP)

    -   [Criando um intervalo de exclusão em DHCP](#BKMK_FndtnPrep_DHCP_Exclusn)

    -   [Criar um novo escopo DHCP](#bkmk_NetFndtn_Pln_DHCP_NewScope)

3.  [Instalar o servidor de política de rede (opcional)](#BKMK_FndtnPrep_InstallNPS)

### <a name="BKMK_FndtnPrep_InstallAD"></a>Instalar o DNS e serviços de domínio do Active Directory
As tabelas desta seção listam os itens de configuração para pré-instalação e a instalação dos serviços de domínio do Active Directory (AD DS) e DNS.

##### <a name="pre-installation-configuration-items-for-ad-ds-and-dns"></a>Itens de configuração de pré-instalação para o AD DS e DNS
As tabelas a seguir itens da lista de configuração de pré-instalação conforme descrito em [configurando todos os servidores](#BKMK_configuringAll):

-   [Configurar um endereço IP estático](#BKMK_ip)

|Itens de configuração|Valores de exemplo|Valores|
|-----------------------|------------------|----------|
|Endereço IP|10.0.0.2||
|Máscara de sub-rede|255.255.255.0||
|Gateway padrão|10.0.0.1||
|Servidor DNS preferencial|127.0.0.1||
|Servidor DNS alternativo|10.0.0.15||

-   [Renomear o computador](#BKMK_rename)

|Item de configuração|Valor de exemplo|Valor|
|----------------------|-----------------|---------|
|Nome do computador|DC1||

##### <a name="ad-ds-and-dns-installation-configuration-items"></a>Itens de configuração de instalação AD DS e DNS
Itens de configuração para obter o procedimento de implantação de rede do Windows Server Core [instalar AD DS e DNS para uma nova floresta](#BKMK_installAD-DNS):

|Itens de configuração|Valores de exemplo|Valores|
|-----------------------|------------------|----------|
|Nome completo do DNS|corp.contoso.com||
|Nível funcional da floresta|Windows Server 2003||
|Local da pasta de banco de dados de serviços de domínio do Active Directory|E:\Configuration\\<br /><br />Ou aceite o local padrão.||
|Serviços de domínio do Active Directory local da pasta arquivos de log|E:\Configuration\\<br /><br />Ou aceite o local padrão.||
|Local da pasta SYSVOL de serviços de domínio do diretório ativo|E:\Configuration\\<br /><br />Ou aceite o local padrão||
|Senha do administrador do modo de restauração de diretório|J * p2leO4$ F||
|Nome do arquivo de resposta (opcional)|AD DS_AnswerFile||

#### <a name="BKMK_FndtnPrep_DNSRevrsLook"></a>Configurando uma zona de pesquisa inversa DNS

|Itens de configuração|Valores de exemplo|Valores|
|-----------------------|------------------|----------|
|Tipo de zona:|-Zona principal<br />-Zona secundária<br />-Zona de stub||
|Tipo de zona<br /><br />**Armazenar a zona no Active Directory**|-Selecionado<br />-Não selecionado||
|Escopo de replicação de zona do Active Directory|-A todos os servidores DNS na floresta<br />-Para todos os servidores DNS neste domínio<br />-Para todos os controladores de domínio nesse domínio<br />-Para todos os controladores de domínio especificados no escopo dessa partição de diretório||
|Nome da zona de pesquisa inversa<br /><br />(Tipo de IP)|-Zona de pesquisa inversa IPv4<br />-Zona de pesquisa inversa IPv6||
|Nome da zona de pesquisa inversa<br /><br />(ID de rede)|10.0.0||

### <a name="BKMK_FndtnPrep_InstallDHCP"></a>Instalar o DHCP
As tabelas desta seção listam os itens de configuração para pré-instalação e a instalação de DHCP.

##### <a name="pre-installation-configuration-items-for-dhcp"></a>Itens de configuração de pré-instalação de DHCP
As tabelas a seguir itens da lista de configuração de pré-instalação conforme descrito em [configurando todos os servidores](#BKMK_configuringAll):

-   [Configurar um endereço IP estático](#BKMK_ip)

|Itens de configuração|Valores de exemplo|Valores|
|-----------------------|------------------|----------|
|Endereço IP|10.0.0.3||
|Máscara de sub-rede|255.255.255.0||
|Gateway padrão|10.0.0.1||
|Servidor DNS preferencial|10.0.0.2||
|Servidor DNS alternativo|10.0.0.15||

-   [Renomear o computador](#BKMK_rename)

|Item de configuração|Valor de exemplo|Valor|
|----------------------|-----------------|---------|
|Nome do computador|DHCP1||

##### <a name="dhcp-installation-configuration-items"></a>Itens de configuração de instalação de DHCP
Itens de configuração para obter o procedimento de implantação de rede do Windows Server Core [instalar DHCP Dynamic Host Configuration Protocol ()](#BKMK_installDHCP):

|Itens de configuração|Valores de exemplo|Valores|
|-----------------------|------------------|----------|
|Associações de conexão de rede|Ethernet||
|Configurações do servidor DNS|DC1||
|Endereço IP do servidor DNS preferencial|10.0.0.2||
|Endereço IP do servidor DNS alternativo|10.0.0.15||
|Nome do escopo|Corp1||
|A partir do endereço IP|10.0.0.1||
|O endereço IP final|10.0.0.254||
|Máscara de sub-rede|255.255.255.0||
|Gateway padrão (opcional)|10.0.0.1||
|Duração da concessão|8 dias||
|Modo de operação do servidor DHCP IPv6|Não habilitado||

#### <a name="BKMK_FndtnPrep_DHCP_Exclusn"></a>Criando um intervalo de exclusão em DHCP
Itens de configuração para criar um intervalo de exclusão durante a criação de um escopo em DHCP.

|Itens de configuração|Valores de exemplo|Valores|
|-----------------------|------------------|----------|
|Nome do escopo|Corp1||
|Descrição do escopo|Sub-rede da matriz 1||
|Endereço IP de inicial de intervalo de exclusão|10.0.0.1||
|Endereço IP de fim de intervalo de exclusão|10.0.0.15||

#### <a name="bkmk_NetFndtn_Pln_DHCP_NewScope"></a>Criar um novo escopo DHCP
Itens de configuração para obter o procedimento de implantação de rede do Windows Server Core [criar e ativar um novo escopo DHCP](#BKMK_newscopeDHCP):

|Itens de configuração|Valores de exemplo|Valores|
|-----------------------|------------------|----------|
|Nome do novo escopo|Corp2||
|Descrição do escopo|Sub-rede de matriz 2||
|(Intervalo de endereços IP)<br /><br />Inicie o endereço IP|10.0.1.1||
|(Intervalo de endereços IP)<br /><br />Endereço IP final|10.0.1.254||
|Comprimento|8||
|Máscara de sub-rede|255.255.255.0||
|(Intervalo de exclusão) Inicie o endereço IP|10.0.1.1||
|Endereço IP de fim de intervalo de exclusão|10.0.1.15||
|Duração da concessão<br /><br />Dias<br /><br />Horas<br /><br />Minutos|-   8<br />-   0<br />-   0||
|Roteador (gateway padrão)<br /><br />Endereço IP|10.0.1.1||
|Domínio pai DNS|corp.contoso.com||
|Servidor DNS<br /><br />Endereço IP|10.0.0.2||

### <a name="BKMK_FndtnPrep_InstallNPS"></a>Instalar o servidor de política de rede (opcional)
As tabelas desta seção listam os itens de configuração para pré-instalação e a instalação do NPS.

##### <a name="pre-installation-configuration-items"></a>Itens de configuração de pré-instalação
As três tabelas a seguir listam os itens de configuração de pré-instalação conforme descrito em [configurando todos os servidores](#BKMK_configuringAll):

-   [Configurar um endereço IP estático](#BKMK_ip)

|Itens de configuração|Valores de exemplo|Valores|
|-----------------------|------------------|----------|
|Endereço IP|10.0.0.4||
|Máscara de sub-rede|255.255.255.0||
|Gateway padrão|10.0.0.1||
|Servidor DNS preferencial|10.0.0.2||
|Servidor DNS alternativo|10.0.0.15||

-   [Renomear o computador](#BKMK_rename)

|Item de configuração|Valor de exemplo|Valor|
|----------------------|-----------------|---------|
|Nome do computador|NPS1||

##### <a name="network-policy-server-installation-configuration-items"></a>Itens de configuração de instalação do servidor de políticas de rede
Itens de configuração para os procedimentos de implantação do Windows Server Core rede NPS [servidor de política de rede (NPS) instalar](#BKMK_installNPS) e [registrar o servidor NPS no domínio padrão](#BKMK_registerNPS).

-   Nenhum item de configuração adicionais é necessárias para instalar e registrar NPS.

