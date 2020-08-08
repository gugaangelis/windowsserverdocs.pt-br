---
title: Etapa 1 planejar a infraestrutura de acesso remoto
description: Este tópico faz parte do guia gerenciar clientes DirectAccess remotamente no Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: a1ce7af5-f3fe-4fc9-82e8-926800e37bc1
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 88bc666b516d00b4c132b5b67ed702f071847fb0
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87989755"
---
# <a name="step-1-plan-the-remote-access-infrastructure"></a>Etapa 1 planejar a infraestrutura de acesso remoto

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

> [!NOTE]
> O Windows Server 2016 combina o DirectAccess e o serviço de roteamento e acesso remoto (RRAS) em uma única função de acesso remoto.

Este tópico descreve as etapas para planejar uma infraestrutura que você pode usar para configurar um único servidor de acesso remoto para o gerenciamento remoto de clientes DirectAccess. A tabela a seguir lista as etapas, mas essas tarefas de planejamento não precisam ser feitas em uma ordem específica.

|Tarefa|Descrição|
|----|--------|
|[Planejar a topologia de rede e as configurações do servidor](#plan-network-topology-and-settings)|Decida onde posicionar o servidor de acesso remoto (na borda ou atrás de um dispositivo NAT) ou de um firewall (conversão de endereços de rede) e o roteamento e endereçamento IP do plano.|
|[Planejar requisitos de firewall](#plan-firewall-requirements)|Planejar como permitir a passagem do Acesso Remoto através de firewalls de borda.|
|[Planejar requisitos de certificado](#plan-certificate-requirements)|Decida se você usará o protocolo Kerberos ou certificados para autenticação de cliente e planejará os certificados do site.<br/><br/>O IP-HTTPS é um protocolo de transição usado por clientes do DirectAccess para tráfego IPv6 de túnel em redes IPv4. Decida se deseja autenticar o IP-HTTPS para o servidor usando um certificado emitido por uma autoridade de certificação (CA) ou usando um certificado autoassinado que é emitido automaticamente pelo servidor de acesso remoto.|
|[Planejar os requisitos de DNS](#plan-dns-requirements)|Planeje as configurações de DNS (sistema de nomes de domínio) para o servidor de acesso remoto, servidores de infraestrutura, opções de resolução de nome local e conectividade de cliente.|
|[Planejar a configuração do servidor do local de rede](#plan-the-network-location-server-configuration)|Decida onde posicionar o site do servidor de local de rede em sua organização (no servidor de acesso remoto ou em um servidor alternativo) e planeje os requisitos de certificado se o servidor do local de rede estiver localizado no servidor de acesso remoto. **Observação:** O servidor de local de rede é usado pelos clientes DirectAccess para determinar se eles estão localizados na rede interna.|
|[Planejar configurações de servidores de gerenciamento](#plan-management-servers-configuration)|Plano para servidores de gerenciamento (como servidores de atualização) que são utilizados durante o gerenciamento de cliente remoto. **Observação:** Os administradores podem gerenciar remotamente os computadores cliente do DirectAccess localizados fora da rede corporativa usando a Internet.|
|[Planejar requisitos de Active Directory](#plan-active-directory-requirements)|Planeje seus controladores de domínio, seus requisitos de Active Directory, autenticação de cliente e várias estruturas de domínio.|
|[Planejar a criação de Política de Grupo objeto](#plan-group-policy-object-creation)|Decida quais GPOs são necessários em sua organização e como criar e editar os GPOs.|

## <a name="plan-network-topology-and-settings"></a>Planejar topologia e configurações de rede
Ao planejar sua rede, você precisa considerar a topologia do adaptador de rede, as configurações de endereçamento IP e os requisitos para o ISATAP.

### <a name="plan-network-adapters-and-ip-addressing"></a>Planejar adaptadores de rede e endereçamento IP

1.  Identifique a topologia do adaptador de rede que você deseja usar. O acesso remoto pode ser configurado com qualquer uma das seguintes topologias:

    -   Com dois adaptadores de rede: o servidor de acesso remoto é instalado na borda com um adaptador de rede conectado à Internet e o outro à rede interna.

    -   Com dois adaptadores de rede: o servidor de acesso remoto é instalado atrás de um dispositivo NAT, firewall ou roteador, com um adaptador de rede conectado a uma rede de perímetro e o outro à rede interna.

    -   Com um adaptador de rede: o servidor de acesso remoto é instalado atrás de um dispositivo NAT e o adaptador de rede único é conectado à rede interna.

2.  Identificar os requisitos de endereçamento IP:

    O DirectAccess usa IPv6 com IPsec para criar uma conexão segura entre os computadores cliente do DirectAccess e a rede interna corporativa. Contudo, o DirectAccess não precisa necessariamente de uma conectividade IPv6 com a Internet ou suporte IPv6 nativo em redes internas. Em vez disso, ele configura automaticamente e usa tecnologias de transição IPv6 para encapsular o tráfego IPv6 na Internet IPv4 (6to4, Teredo ou IP-HTTPS) e em sua intranet somente IPv4 (NAT64 ou ISATAP). Para uma visão geral dessas tecnologias de transição, confira os seguintes recursos:

    -   [Tecnologias de transição IPv6](/previous-versions/bb726951(v=technet.10))

    -   [Especificação do protocolo de túnel IP-HTTPS](/previous-versions/bb726951(v=technet.10))

3.  Configure os adaptadores e endereçamento necessários conforme a tabela a seguir. Para implantações que estão atrás de um dispositivo NAT usando um único adaptador de rede, configure seus endereços IP usando apenas a coluna **adaptador de rede interno** .

    |Descrição|Adaptador de rede externa|Adaptador de rede interno<sup>1, acima</sup>|Requisitos de roteamento|
    |-|--------------|------------------------|------------|
    |Internet IPv4 e intranet IPv4|Configure o seguinte:<br/><br/>-Dois endereços IPv4 públicos consecutivos estáticos com as máscaras de sub-rede apropriadas (necessárias somente para Teredo).<br/>-Um endereço IPv4 de gateway padrão para seu firewall da Internet ou para o roteador do provedor de serviços de Internet local (ISP). **Observação:** O servidor de acesso remoto requer dois endereços IPv4 públicos consecutivos para que ele possa atuar como um servidor Teredo e clientes Teredo baseados no Windows podem usar o servidor de acesso remoto para detectar o tipo de dispositivo NAT.|Configure o seguinte:<br/><br/>-Um endereço de intranet IPv4 com a máscara de sub-rede apropriada.<br/>-Um sufixo DNS específico da conexão para seu namespace de intranet. Um servidor DNS também deverá ser configurado na interface interna. **Cuidado:** Não configure um gateway padrão em nenhuma interface de intranet.|Para configurar o servidor de acesso remoto para alcançar todas as sub-redes na rede IPv4 interna, faça o seguinte:<br/><br/>-Lista os espaços de endereço IPv4 para todos os locais na sua intranet.<br/>-Use os `route add -p` `netsh interface ipv4 add route` comandos ou para adicionar os espaços de endereço IPv4 como rotas estáticas na tabela de roteamento IPv4 do servidor de acesso remoto.|
    |Internet IPv6 e intranet IPv6|Configure o seguinte:<br/><br/>-Use a configuração de endereço autoconfigurado fornecida pelo seu ISP.<br/>-Use o `route print` comando para garantir que uma rota IPv6 padrão que aponta para o roteador do ISP exista na tabela de roteamento IPv6.<br/>-Determine se o ISP e os roteadores de intranet estão usando as preferências de roteador padrão, conforme descrito em RFC 4191, e se eles usam uma preferência padrão mais alta do que os roteadores de intranet local. Se as duas situações forem verdadeiras, nenhuma outra configuração para a rota padrão será necessária. A preferência mais alta para o roteador do ISP assegura que a rota IPv6 padrão ativa do servidor de Acesso Remoto aponte para a Internet IPv6.<br/><br/>Como o servidor de Acesso Remoto é um roteador IPv6, caso você tenha uma infraestrutura IPv6 nativa, a interface de Internet também poderá acessar os controladores de domínio na intranet. Nesse caso, adicione filtros de pacote ao controlador de domínio na rede de perímetro que impeçam a conectividade com o endereço IPv6 da interface de Internet do servidor de acesso remoto.|Configure o seguinte:<br/><br/>Se você não estiver usando níveis de preferência padrão, configure suas interfaces de intranet usando o `netsh interface ipv6 set InterfaceIndex ignoredefaultroutes=enabled` comando. Esse comando garante que as rotas padrão adicionais que apontam para os roteadores da intranet não sejam acrescentadas à tabela de roteamento de IPv6. Você pode obter o InterfaceIndex de suas interfaces de intranet na exibição do `netsh interface show interface` comando.|Se você possui uma intranet IPv6, execute o seguinte procedimento para configurar o servidor de Acesso Remoto para chegar a todos os locais IPv6:<br/><br/>-Lista os espaços de endereço IPv6 para todos os locais na sua intranet.<br/>-Use o `netsh interface ipv6 add route` comando para adicionar os espaços de endereço IPv6 como rotas estáticas na tabela de roteamento IPv6 do servidor de acesso remoto.|
    |Internet IPv4 e intranet IPv6|O servidor de acesso remoto encaminha o tráfego de rota IPv6 padrão usando a interface de adaptador 6to4 da Microsoft para uma retransmissão de 6to4 na Internet IPv4. Quando o IPv6 nativo não está implantado na rede corporativa, você pode usar o seguinte comando para configurar um servidor de acesso remoto para o endereço IPv4 da retransmissão de 6to4 da Microsoft na Internet IPv4: `netsh interface ipv6 6to4 set relay name=<ipaddress> state=enabled` .|||

    > [!NOTE]
    > -   Se o cliente DirectAccess tiver recebido um endereço IPv4 público, ele usará a tecnologia de retransmissão 6to4 para se conectar à intranet. Se o cliente receber um endereço IPv4 privado, ele usará o Teredo. Se o cliente do DirectAccess não puder se conectar ao servidor do DirectAccess com 6to4 ou Teredo, ele usará IP-HTTPS.
    > -   Para usar o Teredo, você precisa configurar dois endereços IP consecutivos no adaptador de rede voltado para o exterior.
    > -   Você não poderá usar o Teredo se o servidor de acesso remoto tiver apenas um adaptador de rede.
    > -   Os computadores cliente IPv6 nativos podem conectar-se ao servidor de Acesso Remoto pelo IPv6 nativo, sem a necessidade de tecnologia de transição.

### <a name="plan-isatap-requirements"></a>Planejar requisitos de ISATAP
O ISATAP é necessário para o gerenciamento remoto do DirectAccessclients, para que os servidores de gerenciamento do DirectAccess possam se conectar aos clientes DirectAccess localizados na Internet. O ISATAP não é necessário para dar suporte a conexões iniciadas por computadores cliente do DirectAccess para recursos de IPv4 na rede corporativa. NAT64/DNS64 é usado para esta finalidade. Se sua implantação exigir ISATAP, use a tabela a seguir para identificar seus requisitos.

|Cenário de implantação ISATAP|Requisitos|
|---------------|--------|
|Intranet nativa IPv6 existente (não é necessário ISATAP)|Com uma infraestrutura IPv6 nativa existente, você especifica o prefixo da organização durante a implantação de acesso remoto e o servidor de acesso remoto não se configura como um roteador ISATAP. Faça o seguinte:<br/><br/>1. para garantir que os clientes DirectAccess possam ser acessados pela intranet, você deve modificar o roteamento IPv6 para que o tráfego de rota padrão seja encaminhado para o servidor de acesso remoto. Se o espaço de endereço IPv6 da intranet usar um endereço diferente de um único prefixo de endereço IPv6 de 48 bits, você deverá especificar o prefixo IPv6 da organização relevante durante a implantação.<br/>2. se você estiver conectado à Internet IPv6, deverá configurar o tráfego de rota padrão para que ele seja encaminhado ao servidor de acesso remoto e, em seguida, configurar as conexões e rotas apropriadas no servidor de acesso remoto, para que o tráfego de rota padrão seja encaminhado ao dispositivo que está conectado à Internet IPv6.|
|Implantação ISATAP existente|Se você tiver uma infraestrutura ISATAP existente, durante a implantação, será solicitado o prefixo de 48 bits da organização e o servidor de acesso remoto não se configurará como um roteador ISATAP. Para garantir que os clientes DirectAccess possam ser acessados pela intranet, você deve modificar sua infraestrutura de roteamento IPv6 para que o tráfego de rota padrão seja encaminhado para o servidor de acesso remoto. Essa alteração precisa ser feita no roteador ISATAP existente para o qual os clientes de intranet já devem estar encaminhando o tráfego padrão.|
|Nenhuma conectividade IPv6 existente|Quando o assistente de instalação de acesso remoto detecta que o servidor não tem conectividade IPv6 baseada em ISATAP ou nativa, ele deriva automaticamente um prefixo de 48 bits baseado em 6to4 para a intranet e configura o servidor de acesso remoto como um roteador ISATAP para fornecer conectividade IPv6 para hosts ISATAP em toda a sua intranet. (Um prefixo baseado em 6to4 será usado somente se o servidor tiver endereços públicos; caso contrário, o prefixo será gerado automaticamente a partir de um intervalo de endereços local exclusivo.)<br/><br/>Para usar o ISATAP, faça o seguinte:<br/><br/>1. Registre o nome ISATAP em um servidor DNS para cada domínio no qual você deseja habilitar a conectividade baseada em ISATAP, para que o nome ISATAP possa ser resolvido pelo servidor DNS interno para o endereço IPv4 interno do servidor de acesso remoto.<br/>2. por padrão, os servidores DNS que executam o Windows Server 2012, o Windows Server 2008 R2, o Windows Server 2008 ou o Windows Server 2003 bloqueiam a resolução do nome ISATAP usando a lista de blocos de consulta global. Para habilitar o ISATAP, você deve remover o nome ISATAP da lista de blocos. Para obter mais informações, consulte [Remover o ISATAP da lista global de consultas não autorizadas do DNS](https://go.microsoft.com/fwlink/p/?LinkId=168593).<br/><br/>Os hosts ISATAP baseados no Windows que podem resolver o nome ISATAP automaticamente configuram um endereço com o servidor de acesso remoto da seguinte maneira:<br/><br/>1. um endereço IPv6 baseado em ISATAP em uma interface de encapsulamento ISATAP<br/>2. uma rota de 64 bits que fornece conectividade com os outros hosts ISATAP na intranet<br/>3. uma rota IPv6 padrão que aponta para o servidor de acesso remoto. A rota padrão garante que os hosts ISATAP de intranet possam acessar clientes DirectAccess<br/><br/>Quando seus hosts ISATAP baseados no Windows obtêm um endereço IPv6 baseado em ISATAP, eles começam a usar o tráfego encapsulado ISATAP para se comunicarem se o destino também for um host ISATAP. Como o ISATAP usa uma única sub-rede de 64 bits para toda a intranet, sua comunicação passa de um modelo de comunicação IPv4 segmentado para um modelo de comunicação de sub-rede única com IPv6. Isso pode afetar o comportamento de alguns Active Directory Domain Services (AD DS) e aplicativos que dependem de sua configuração de sites e serviços do Active Directory. Por exemplo, se você usou o snap-in Active Directory sites e serviços para configurar sites, sub-redes baseadas em IPv4 e transportes entre sites para encaminhar solicitações para servidores dentro de sites, essa configuração não será usada por hosts ISATAP.<br/><br/><ol><li>Para configurar sites e serviços do Active Directory para encaminhamento em sites para hosts ISATAP, para cada objeto de sub-rede IPv4, você deve configurar um objeto de sub-rede IPv6 equivalente, no qual o prefixo de endereço IPv6 para a sub-rede expressa o mesmo intervalo de endereços de host ISATAP que a sub-rede IPv4. Por exemplo, para a sub-rede IPv4 192.168.99.0/24 e o prefixo de endereço ISATAP de 64 bits 2002:836b: 1:8000::/64, o prefixo de endereço IPv6 equivalente para o objeto de sub-rede IPv6 é 2002:836b: 1:8000:0: 5efe: 192.168.99.0/120. Para um comprimento de prefixo IPv4 arbitrário (definido como 24 no exemplo), você pode determinar o comprimento do prefixo IPv6 correspondente da fórmula 96 + IPv4PrefixLength.</li><li>Para os endereços IPv6 dos clientes DirectAccess, adicione o seguinte:<br/><br/><ul><li>Para clientes DirectAccess baseados em Teredo: uma sub-rede IPv6 para o intervalo de 2001:0: WWXX: YYZZ::/64, em que WWXX: YYZZ é a versão hexadecimal de dois-pontos do primeiro endereço IPv4 voltado para a Internet do servidor de acesso remoto. .</li><li>Para clientes DirectAccess baseados em IP-HTTPS: uma sub-rede IPv6 para o intervalo de 2002: WWXX: YYZZ: 8100::/56, em que WWXX: YYZZ é a versão hexadecimal de dois-pontos do primeiro endereço IPv4 voltado para a Internet (w.x.y. z) do servidor de acesso remoto. .</li><li>Para clientes DirectAccess baseados em 6to4: uma série de prefixos IPv6 baseados em 6to4 que começam com 2002: e representam os prefixos de endereço IPv4 públicos e regionais administrados pela IANA (Internet Assigned Numbers Authority) e registros regionais. O prefixo baseado em 6to4 para um prefixo de endereço IPv4 público w.x.y.z/n é 2002:WWXX:YYZZ::/[16+n], em que WWXX:YYZZ é a versão hexadecimal com dois-pontos de w.x.y.z.<br/><br/>        Por exemplo, o intervalo 7.0.0.0/8 é administrado pelo ARIN (Registro Americano para Números da Internet) para a América do Norte. O prefixo baseado em 6to4 correspondente para esse intervalo de endereços IPv6 públicos é 2002:700::/24. Para obter informações sobre o espaço de endereço IPv4 público, consulte [registro de espaço de endereço IPv4 da IANA](https://www.iana.org/assignments/ipv4-address-space/ipv4-address-space.xml). .</li></ul></li></ol>|

> [!IMPORTANT]
> Verifique se você não tem endereços IP públicos na interface interna do servidor DirectAccess. Se você tiver um endereço IP público na interface interna, a conectividade por meio de ISATAP poderá falhar.

### <a name="plan-firewall-requirements"></a>Planejar requisitos de firewall
Se o servidor de Acesso Remoto estiver atrás de um firewall de borda, as seguintes exceções serão necessárias para o tráfego do Acesso Remoto quando o servidor de Acesso Remoto estiver na Internet IPv4:

-   Para IP-HTTPS: TCP (protocolo de controle de transmissão) porta de destino 443 e saída da porta de origem TCP 443.

-   Para o tráfego Teredo: porta de destino UDP (User Datagram Protocol) 3544 de entrada e porta de origem UDP 3544 de saída.

-   Para o tráfego 6to4: protocolo IP 41 de entrada e saída.

    > [!NOTE]
    > Para tráfego Teredo e 6to4, essas exceções devem ser aplicadas para os endereços IPv4 públicos consecutivos voltados para a Internet no servidor de Acesso Remoto.
    >
    > Para IP-HTTPS, as exceções precisam ser aplicadas no endereço registrado no servidor DNS público.

-   Se você estiver implantando o acesso remoto com um único adaptador de rede e instalando o servidor de local de rede no servidor de acesso remoto, a porta TCP 62000.

    > [!NOTE]
    > Essa isenção está no servidor de acesso remoto e as isenções anteriores estão no firewall do Edge.

As seguintes exceções são necessárias para o tráfego de acesso remoto quando o servidor de acesso remoto está na Internet IPv6:

-   Protocolo IP 50

-   Entrada da porta de destino UDP 500 e saída da porta de origem UDP 500.

-   Entrada de tráfego ICMPv6 e saída (somente ao usar Teredo).

Quando você estiver usando firewalls adicionais, aplique as seguintes exceções de firewall de rede interna para o tráfego de acesso remoto:

-   Para ISATAP: protocolo 41 de entrada e saída

-   Para todo o tráfego IPv4/IPv6: TCP/UD

-   Para Teredo: ICMP para todo o tráfego IPv4/IPv6

### <a name="plan-certificate-requirements"></a>Planejar requisitos de certificado
Há três cenários que exigem certificados quando você implanta um único servidor de acesso remoto.

-   **Autenticação IPsec**: os requisitos de certificado para IPsec incluem um certificado de computador que é usado pelos computadores cliente do DirectAccess quando eles estabelecem a conexão IPsec com o servidor de acesso remoto e um certificado de computador usado pelos servidores de acesso remoto para estabelecer conexões IPSec com clientes DirectAccess.

    Para o DirectAccess no Windows Server 2012, o uso desses certificados IPsec não é obrigatório. Como alternativa, o servidor de acesso remoto pode atuar como um proxy para autenticação Kerberos sem a necessidade de certificados. Se a autenticação Kerberos for usada, ela funcionará em SSL e o protocolo Kerberos usará o certificado que foi configurado para IP-HTTPS. Alguns cenários empresariais (incluindo a implantação multissite e a autenticação de cliente de senha única) exigem o uso da autenticação de certificado e não da autenticação Kerberos.

-   **Servidor IP-HTTPS**: quando você configura o acesso remoto, o servidor de acesso remoto é configurado automaticamente para atuar como ouvinte da Web IP-HTTPS. O site IP-HTTPS precisa de um certificado de site, e os computadores cliente também devem poder contatar o site da CRL (lista de certificados revogados) do certificado.

-   **Servidor de local de rede**: o servidor de local de rede é um site que é usado para detectar se os computadores cliente estão localizados na rede corporativa. O servidor local da rede precisa de um certificado de site. Clientes do DirectAccess devem poder contatar o site da CRL para o certificado.

Os requisitos de AC (autoridade de certificação) para cada um desses cenários são resumidos na tabela a seguir.

|Autenticação IPsec|Servidor IP-HTTPS|Servidor de local da rede|
|------------|----------|--------------|
|Uma AC interna é necessária para emitir certificados de computador para o servidor de acesso remoto e clientes para autenticação IPsec quando você não usa o protocolo Kerberos para autenticação.|AC interna: você pode usar uma AC interna para emitir o certificado IP-HTTPS; no entanto, você deve verificar se o ponto de distribuição da CRL está disponível externamente.|AC interna: você pode usar uma AC interna para emitir o certificado de site do servidor de local de rede. Verifique se o ponto de distribuição de CRL está altamente disponível pela rede interna.|
||Certificado autoassinado: você pode usar um certificado autoassinado para o servidor IP-HTTPS. Um certificado autoassinado não pode ser usado em uma implantação multissite.|Certificado autoassinado: você pode usar um certificado autoassinado para o site do servidor do local de rede; no entanto, você não pode usar um certificado autoassinado em implantações multissite.|
||AC pública: Recomendamos que você use uma autoridade de certificação pública para emitir o certificado IP-HTTPS, isso garante que o ponto de distribuição da CRL esteja disponível externamente.||

#### <a name="plan-computer-certificates-for-ipsec-authentication"></a>Planejar certificados de computador para autenticação IPsec
Se você estiver usando a autenticação IPsec baseada em certificado, o servidor de acesso remoto e os clientes serão necessários para obter um certificado de computador. A maneira mais simples de instalar os certificados é usar Política de Grupo para configurar o registro automático de certificados de computador. Isso garante que todos os membros do domínio obtenham um certificado de uma AC corporativa. Se você não tiver uma AC corporativa configurada na organização, consulte [Serviços de Certificados do Active Directory](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770357(v=ws.10)).

Esse certificado possui os seguintes requisitos:

-   O certificado deve ter EKU (uso estendido de chave) de autenticação de cliente.

-   O cliente e os certificados de servidor devem estar relacionados ao mesmo certificado raiz. Esse certificado raiz deve ser escolhido nas definições de configuração do DirectAccess.

#### <a name="plan-certificates-for-ip-https"></a>Planejar certificados para IP-HTTPS
O servidor de Acesso Remoto age como um ouvinte do IP-HTTPS, e você deve instalar manualmente um certificado de site HTTPS no servidor. Leve em conta as seguintes considerações ao planejar:

-   É recomendável usar uma AC pública, para que as CRLs estejam prontamente disponíveis.

-   No campo assunto, especifique o endereço IPv4 do adaptador de Internet do servidor de acesso remoto ou o FQDN da URL IP-HTTPS (o endereço connectto). Se o servidor de Acesso Remoto encontrar-se atrás de um dispositivo NAT, o nome ou endereço público do dispositivo NAT deverá ser especificado.

-   O nome comum do certificado deverá ser corresponder ao nome do site IP-HTTPS.

-   Para o campo **uso avançado de chave** , use o identificador de objeto de autenticação de servidor (OID).

-   No campo **Pontos de Distribuição da Lista de Certificados Revogados**, especifique um ponto de distribuição da CRL que seja acessível aos clientes do DirectAccess conectados à Internet.

    > [!NOTE]
    > Isso só é necessário para clientes que executam o Windows 7.

-   O certificado IP-HTTPS deve ter uma chave privada.

-   O certificado IP-HTTPS deve ser importado diretamente para o repositório pessoal.

-   Os certificados IP-HTTPS podem ter curingas no nome.

#### <a name="plan-website-certificates-for-the-network-location-server"></a>Planejar certificados de site para o servidor de local da rede
Considere o seguinte ao planejar o site do servidor do local de rede:

-   No campo **Assunto**, especifique o endereço IP da interface da intranet do servidor de local de rede do FQDN da URL de rede local.

-   Para o campo **uso avançado de chave** , use o OID de autenticação do servidor.

-   Para o campo **pontos de distribuição da CRL** , use um ponto de distribuição de CRL acessível por clientes DirectAccess que estão conectados à intranet. Esse ponto de distribuição da CRL não deverá ser acessível de fora da rede interna.

> [!NOTE]
> Verifique se os certificados para IP-HTTPS e servidor de local de rede têm um nome de entidade. Se o certificado usar um nome alternativo, ele não será aceito pelo assistente de acesso remoto.

#### <a name="plan-dns-requirements"></a>Planejar os requisitos de DNS
Esta seção explica os requisitos de DNS para clientes e servidores em uma implantação de acesso remoto.

##### <a name="directaccess-client-requests"></a>Solicitações de cliente do DirectAccess
O DNS é usado para resolver solicitações de computadores cliente do DirectAccess não localizados na rede interna. Os clientes do DirectAccess tentam se conectar ao servidor de local de rede do DirectAccess para determinar se estão localizados na Internet ou na rede corporativa.

-   Se a conexão for bem-sucedida, os clientes serão determinados na intranet, o DirectAccess não será usado e as solicitações do cliente serão resolvidas usando o servidor DNS configurado no adaptador de rede do computador cliente.

-   Se a conexão não for bem-sucedida, presume-se que os clientes estejam na Internet. Clientes do DirectAccess usarão a NRPT (tabela de políticas de resolução de nomes) para determinar qual servidor DNS usar ao resolver solicitações de nome. Você pode especificar que os clientes devem usar o DirectAccess DNS64 para resolver nomes ou um servidor DNS interno alternativo.

Ao realizar a resolução do nome, a NRPT é usada pelos clientes do DirectAccess para identificar como lidar com uma solicitação. Os clientes solicitam um FQDN ou um nome de rótulo único, como <https://internal> . Se um nome de rótulo único for solicitado, um sufixo do DNs é agregado para criar um FQDN. Se a consulta DNS corresponder a uma entrada no NRPT e DNS4 ou um servidor DNS da intranet for especificado para a entrada, a consulta será enviada para resolução de nomes usando o servidor especificado. Se houver uma correspondência, mas nenhum servidor DNS for especificado, uma regra de isenção e a resolução de nome normal serão aplicadas.

Quando um novo sufixo é adicionado ao NRPT no console de gerenciamento de acesso remoto, os servidores DNS padrão para o sufixo podem ser descobertos automaticamente clicando no botão **detectar** . A detecção automática funciona da seguinte maneira:

-   Se a rede corporativa for baseada em IPv4 ou usar IPv4 e IPv6, o endereço padrão será o endereço DNS64 do adaptador interno no servidor de acesso remoto.

-   Se a rede corporativa for baseada em IPv6, o endereço padrão será o endereço IPv6 dos servidores DNS na rede corporativa.

##### <a name="infrastructure-servers"></a>Servidores de infraestrutura

-   **Servidor de local da rede**

    Os clientes DirectAccess tentam acessar o servidor de local de rede para determinar se estão em uma rede interna. Os clientes na rede interna devem ser capazes de resolver o nome do servidor de local de rede, e eles devem ser impedidos de solucionar o nome quando estiverem localizados na Internet. Para garantir que isso ocorra, o FQDN do servidor do local de rede é adicionado, por padrão, a uma regra de exceção do NRPT. Quando você configurar o Acesso Remoto, as seguintes regras também serão criadas automaticamente:

    -   Uma regra de sufixo DNS para o domínio raiz ou o nome de domínio do servidor de acesso remoto e os endereços IPv6 que correspondem aos servidores DNS da intranet configurados no servidor de acesso remoto. Por exemplo, se o servidor de Acesso Remoto for membro do domínio corp.contoso.com, é criada uma regra para o sufixo de DNS corp.contoso.com.

    -   Uma regra de isenção para o FQDN do servidor de local de rede. Por exemplo, se a URL do servidor de local de rede for <https://nls.corp.contoso.com> , uma regra de isenção será criada para o FQDN NLS.Corp.contoso.com.

-   **Servidor IP-HTTPS**

    O servidor de acesso remoto atua como um ouvinte IP-HTTPS e usa seu certificado de servidor para autenticar em clientes IP-HTTPS. O nome IP-HTTPS deve ser resolvido pelos clientes DirectAccess que usam servidores DNS públicos.

##### <a name="connectivity-verifiers"></a>Verificadores de conectividade
O Acesso Remoto cria uma sonda da Web padrão usada pelos computadores cliente do DirectAccess para verificar a conectividade com a rede interna. Para garantir que a sonda funcione como esperado, os nomes a seguir devem ser registrados manualmente no DNS:

-   o **DirectAccess-webprobehost** deve ser resolvido para o endereço IPv4 interno do servidor de acesso remoto ou para o endereço IPv6 em um ambiente somente IPv6.

-   o **DirectAccess-corpconnectivityhost** deve ser resolvido para o endereço do host local (loopback). Você deve criar registros A e AAAA. O valor do registro a é 127.0.0.1 e o valor do registro AAAA é construído a partir do prefixo NAT64 com os últimos 32 bits como 127.0.0.1. O prefixo NAT64 pode ser recuperado executando o cmdlet **Get-netnatTransitionConfiguration** do Windows PowerShell.

    > [!NOTE]
    > Isso é válido somente em ambientes somente IPv4. Em um ambiente IPv4 mais IPv6 ou somente IPv6, crie apenas um registro AAAA com o endereço IP de loopback:: 1.

Você pode criar verificadores de conectividade adicionais usando outros endereços da Web via HTTP ou PING. Uma entrada de DNS deverá existir para cada verificador de conectividade.

##### <a name="dns-server-requirements"></a>Requisitos de servidor DNS

-   Para clientes do DirectAccess, você deve usar um servidor DNS que executa o Windows Server 2012, o Windows Server 2008 R2, o Windows Server 2008, o Windows Server 2003 ou qualquer servidor DNS que ofereça suporte a IPv6.

-   Você deve usar um servidor DNS que ofereça suporte a atualizações dinâmicas. Você pode usar servidores DNS que não dão suporte a atualizações dinâmicas, mas as entradas devem ser atualizadas manualmente.

-   O FQDN para os pontos de distribuição da CRL deve ser resolvido usando servidores DNS da Internet. Por exemplo, se <https://crl.contoso.com/crld/corp-DC1-CA.crl> a URL estiver no campo **pontos de distribuição da CRL** do certificado IP-HTTPS do servidor de acesso remoto, você deverá verificar se o FQDN crld.contoso.com pode ser resolvido usando servidores DNS da Internet.

#### <a name="plan-for-local-name-resolution"></a>Planejar a resolução de nomes locais
Considere o seguinte ao planejar a resolução de nome local:

##### <a name="nrpt"></a>NRPT
Talvez seja necessário criar regras de NRPT (tabela de políticas de resolução de nome) adicionais nas seguintes situações:

-   Você precisa adicionar mais sufixos DNS para seu namespace de intranet.

-   Se os FQDNs dos pontos de distribuição da CRL forem baseados em seu namespace de intranet, você deverá adicionar regras de isenção para os FQDNs dos pontos de distribuição da CRL.

-   Se você tiver um ambiente DNS de divisão-Brain, deverá adicionar regras de isenção para os nomes de recursos para os quais você deseja que os clientes do DirectAccess que estão localizados na Internet acessem a versão da Internet, em vez da versão da intranet.

-   Se você estiver redirecionando o tráfego para um site externo por meio de seus servidores proxy da Web Intranet, o site externo estará disponível apenas na intranet. Ele usa os endereços dos seus servidores proxy da Web para permitir as solicitações de entrada. Nessa situação, adicione uma regra de isenção para o FQDN do site externo e especifique que a regra usa o servidor proxy da Web de intranet em vez dos endereços IPv6 dos servidores DNS da intranet.

    Por exemplo, digamos que você esteja testando um site externo chamado test.contoso.com. Esse nome não é resolvível por meio de servidores DNS da Internet, mas o servidor de proxy Web da Contoso sabe como resolver o nome e como direcionar solicitações para o site para o servidor Web externo. Para evitar que os usuários que não estiverem na intranet da Contoso acessem o site, o site externo permitirá solicitações somente do endereço da Internet IPv4 do proxy Web da Contoso. Assim, os usuários da intranet podem acessar o site porque estão usando o proxy Web da Contoso, mas os usuários do DirectAccess não podem porque não estão usando o proxy Web da contoso. Com a configuração de uma regra de isenção de NRPT para test.contoso.com que usar o proxy Web da Contoso, as solicitações de página da Web para test.contoso.com serão roteadas para o servidor proxy Web da intranet na Internet IPv4.

##### <a name="single-label-names"></a>Nomes de rótulo único
Nomes de rótulo único, como <https://paycheck> , às vezes são usados para servidores de intranet. Se um único nome de rótulo for solicitado e uma lista de pesquisa de sufixo DNS estiver configurada, os sufixos DNS na lista serão anexados ao nome de rótulo único. Por exemplo, quando um usuário em um computador que é membro dos tipos de domínio corp.contoso.com <https://paycheck> no navegador da Web, o FQDN que é construído como o nome é Paycheck.Corp.contoso.com. Por padrão, o sufixo anexado é baseado no sufixo DNS primário do computador cliente.

> [!NOTE]
> Em um cenário de espaço de nome não contíguo (em que um ou mais computadores de domínio têm um sufixo DNS que não corresponde ao domínio de Active Directory ao qual os computadores são membros), você deve garantir que a lista de pesquisa seja personalizada para incluir todos os sufixos necessários. Por padrão, o assistente de acesso remoto configura o Active Directory nome DNS como o sufixo DNS primário no cliente. Certifique-se de adicionar o sufixo de DNS usado pelos clientes para a resolução de nome.

Se vários domínios e o WINS (serviço de cadastramento na Internet do Windows) estiverem implantados em sua organização e você estiver se conectando remotamente, os nomes únicos poderão ser resolvidos da seguinte maneira:

-   Implantando uma zona de pesquisa direta WINS no DNS. Ao tentar resolver computername.dns.zone1.corp.contoso.com, a solicitação é direcionada para o servidor WINS que está usando apenas o nome do computador. O cliente pensa que está emitindo uma solicitação de registros DNS A normal, mas é, na verdade, uma solicitação NetBIOS.

    Para obter mais informações, consulte [Gerenciando uma zona de pesquisa direta](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc816891(v=ws.10)).

-   Adicionando um sufixo DNS (por exemplo, dns.zone1.corp.contoso.com) ao GPO de domínio padrão.

##### <a name="split-brain-dns"></a>DNS com partição de rede
O DNS de divisão-Brain refere-se ao uso do mesmo domínio DNS para a resolução de nomes de Internet e intranet.

Para implantações de DNS de divisão-Brain, você deve listar os FQDNs que estão duplicados na Internet e na intranet e decidir quais recursos o cliente DirectAccess deve alcançar – a intranet ou a versão da Internet. Quando você deseja que os clientes do DirectAccess acessem a versão da Internet, você deve adicionar o FQDN correspondente como uma regra de isenção à NRPT para cada recurso.

Em um ambiente de DNS de divisão-cérebro, se você quiser que ambas as versões do recurso estejam disponíveis, configure seus recursos de intranet com nomes que não duplicam os nomes que são usados na Internet. Em seguida, instrua os usuários a usar o nome alternativo quando acessarem o recurso na intranet. Por exemplo, configure www \. Internal.contoso.com para o nome interno de www \. contoso.com.

Em um ambiente de DNS sem partição de rede, o namespace da Internet é diferente do namespace da intranet. Por exemplo, a Contoso Corporation usa contoso.com na Internet e corp.contoso.com na intranet. Como todos os recursos da intranet usam o sufixo DNS corp.contoso.com, a regra da NRPT para corp.contoso.com roteia todas as consultas de nome DNS por recursos de intranet para servidores DNS da intranet. As consultas DNS para nomes com o sufixo contoso.com não correspondem à regra de namespace de intranet corp.contoso.com na NRPT e são enviadas aos servidores DNS da Internet. Com uma implantação de DNS com partição de rede, não é necessário fazer configurações adicionais na NRPT, pois não há duplicação dos FQDNs dos recursos da Internet e da intranet. Os clientes DirectAccess podem acessar os recursos da Internet e da intranet da organização.

##### <a name="plan-local-name-resolution-behavior-for-directaccess-clients"></a>Planejar o comportamento de resolução de nome local para clientes do DirectAccess
Se um nome não puder ser resolvido com o DNS, o serviço cliente DNS no Windows Server 2012, Windows 8, Windows Server 2008 R2 e Windows 7 poderá usar a resolução de nomes locais, com o LLMNR (link-local multicast Name Resolution) e o NetBIOS sobre protocolos TCP/IP, para resolver o nome na sub-rede local. Em geral, a resolução de nomes locais é necessária para a conectividade ponto a ponto, quando o computador está localizado em redes privadas, como redes domésticas com uma única sub-rede.

Quando o serviço cliente DNS executa a resolução de nomes locais para nomes de servidores de intranet e o computador está conectado a uma sub-rede compartilhada na Internet, os usuários mal-intencionados podem capturar as mensagens de LLMNR e NetBIOS sobre TCP/IP para determinar os nomes dos servidores de intranet. Na página DNS do assistente de instalação do servidor de infraestrutura, você pode configurar o comportamento de resolução de nome local com base nos tipos de respostas recebidas de servidores DNS da intranet. As seguintes opções estão disponíveis:

-   **Usar resolução de nome local se o nome não existir no DNS**: essa opção é a mais segura porque o cliente DirectAccess executa a resolução de nomes locais somente para nomes de servidor que não podem ser resolvidos pelos servidores DNS da intranet. Se os servidores DNS da intranet puderem ser acessados, os nomes dos servidores da intranet serão resolvidos. Se os servidores DNS da intranet não puderem ser acessados ou se houver outros tipos de erros de DNS, não haverá uma perda dos nomes do servidor da intranet para a sub-rede por meio da resolução de nomes locais.

-   **Usar resolução de nomes locais se o nome não existir em servidores DNS ou DNS inacessíveis quando o computador cliente estiver em uma rede privada (recomendado)**: essa opção é recomendada porque permite o uso da resolução de nomes locais em uma rede privada somente quando os servidores DNS da intranet estiverem inacessíveis.

-   **Usar a resolução de nomes locais para qualquer tipo de erro de resolução de DNS (menos seguro)**: essa é a opção menos segura porque os nomes de servidores de rede de intranet podem ser vazados para a sub-rede local por meio da resolução de nomes locais.

#### <a name="plan-the-network-location-server-configuration"></a>Planejar a configuração do servidor do local de rede
O servidor de local da rede é um site usado para detectar se os clientes DirectAccess encontram-se na rede corporativa. Os clientes na rede corporativa não usam o DirectAccess para acessar recursos internos; Mas, em vez disso, eles se conectam diretamente.

O site do servidor do local de rede pode ser hospedado no servidor de acesso remoto ou em outro servidor em sua organização. Se você hospedar o servidor de local de rede no servidor de acesso remoto, o site será criado automaticamente quando você implantar o acesso remoto. Se você hospedar o servidor de local de rede em outro servidor que executa um sistema operacional Windows, verifique se Serviços de Informações da Internet (IIS) está instalado no servidor e se o site foi criado. O acesso remoto não define as configurações no servidor do local de rede.

Verifique se o site do servidor do local de rede atende aos seguintes requisitos:

-   Tem um certificado de servidor HTTPS.

-   Tem alta disponibilidade para computadores na rede interna.

-   Não está acessível para computadores cliente do DirectAccess na Internet.

-

Além disso, considere os seguintes requisitos para clientes quando você estiver configurando o site do servidor do local de rede:

-   Os computadores cliente do DirectAccess devem confiar na AC que emitiu o certificado do servidor para o site do servidor do local de rede.

-   Os computadores cliente do DirectAccess na rede interna devem poder resolver o nome do site do servidor do local de rede.

##### <a name="plan-certificates-for-the-network-location-server"></a>Planejar certificados para o servidor de local de rede
Ao obter o certificado do site a ser usado para o servidor de local de rede, considere o seguinte:

-   No cmapo **Assunto**, especifique o endereço IP da interface da intranet do servidor de local de rede do FQDN da URL de rede local.

-   Para o campo **uso avançado de chave** , use o OID de autenticação do servidor.

-   O certificado do servidor do local de rede deve ser verificado em relação a uma CRL (lista de certificados revogados). Para o campo **pontos de distribuição da CRL** , use um ponto de distribuição de CRL acessível por clientes DirectAccess que estão conectados à intranet. Esse ponto de distribuição da CRL não deverá ser acessível de fora da rede interna.

##### <a name="plan-dns-for-the-network-location-server"></a>Planejar o DNS para o servidor de local de rede
Os clientes DirectAccess tentam acessar o servidor de local de rede para determinar se estão em uma rede interna. Os clientes na rede interna devem poder resolver o nome do servidor de local de rede, mas não deverão resolver o nome quando se encontrarem na Internet. Para garantir que isso ocorra, o FQDN do servidor do local de rede é adicionado, por padrão, a uma regra de exceção da NRPT.

### <a name="plan-management-servers-configuration"></a>Planejar configuração de servidores de gerenciamento
Os clientes do DirectAccess iniciam a comunicação com servidores de gerenciamento que fornecem serviços como atualizações de Windows Update e antivírus. Os clientes DirectAccess também usam o protocolo Kerberos para autenticar os controladores de domínio antes de acessarem a rede interna. Durante o gerenciamento dos clientes DirectAccess, os servidores de gerenciamento comunicam-se com os computadores cliente para realizar funções de gerenciamento como avaliações de inventário de software ou hardware. O Acesso Remoto pode descobrir alguns servidores de gerenciamento automaticamente, incluindo:

-   Controladores de domínio: a descoberta automática de controladores de domínio é executada para os domínios que contêm computadores cliente e para todos os domínios na mesma floresta que o servidor de acesso remoto.

-   Microsoft Endpoint Configuration Manager Servers

Os controladores de domínio e os servidores de Configuration Manager são detectados automaticamente na primeira vez que o DirectAccess é configurado. Os controladores de domínio detectados não são exibidos no console do, mas as configurações podem ser recuperadas usando cmdlets do Windows PowerShell. Se os servidores do controlador de domínio ou do Configuration Manager forem modificados, clicar em **Gerenciamento de atualizações servidores** no console do atualizará a lista de servidores de gerenciamento.

**Requisitos do Management Server**

-   Os servidores de gerenciamento devem ser acessíveis no túnel de infraestrutura. Quando você configurar o Acesso Remoto, os servidores adicionados à lista de servidores de gerenciamento poderão ser automaticamente acessados por esse túnel.

-   Os servidores de gerenciamento que iniciam conexões com clientes DirectAccess devem oferecer suporte total ao IPv6, por meio de um endereço IPv6 nativo ou usando um endereço atribuído pelo ISATAP.

### <a name="plan-active-directory-requirements"></a>Planejar requisitos de Active Directory
O acesso remoto usa Active Directory da seguinte maneira:

-   **Autenticação**: o túnel de infraestrutura usa a autenticação NTLMv2 para a conta de computador que está se conectando ao servidor de acesso remoto e a conta deve estar em um domínio Active Directory. O túnel de intranet usa a autenticação Kerberos para que o usuário crie o túnel de intranet.

-   **Objetos de política de grupo**: o acesso remoto reúne definições de configuração em objetos política de grupo (GPOs), que são aplicados a servidores de acesso remoto, clientes e servidores de aplicativos internos.

-   **Grupos de segurança**: o acesso remoto usa grupos de segurança para coletar e identificar os computadores cliente do DirectAccess. Os GPOs são aplicados aos grupos de segurança necessários.

Ao planejar um ambiente de Active Directory para uma implantação de acesso remoto, considere os seguintes requisitos:

-   Pelo menos um controlador de domínio é instalado no sistema operacional Windows Server 2012, Windows Server 2008 R2 Windows Server 2008 ou Windows Server 2003.

    Se o controlador de domínio estiver em uma rede de perímetro (e, portanto, acessível a partir do adaptador de rede voltado para a Internet do servidor de acesso remoto), impeça que o servidor de acesso remoto chegue a ele. Você precisa adicionar filtros de pacote no controlador de domínio para evitar a conectividade com o endereço IP do adaptador de Internet.

-   O servidor de Acesso Remoto deve ser um membro do domínio.

-   Os clientes do DirectAccess devem ser membros do domínio. Os clientes podem pertencer a:

    -   Qualquer domínio na mesma floresta que o servidor de Acesso Remoto.

    -   Qualquer domínio que tenha uma relação de confiança bidirecional com o domínio do servidor de Acesso Remoto.

    -   Qualquer domínio em uma floresta que tenha uma relação de confiança bidirecional com a floresta do domínio do servidor de acesso remoto.

> [!NOTE]
> -   O servidor de Acesso Remoto não deve ser um controlador de domínio.
> -   O controlador de domínio Active Directory usado para acesso remoto não deve ser acessado do adaptador de Internet externo do servidor de acesso remoto (o adaptador não deve estar no perfil de domínio do firewall do Windows).

#### <a name="plan-client-authentication"></a>Planejar autenticação de cliente
No acesso remoto no Windows Server 2012, você pode escolher entre usar a autenticação Kerberos interna, que usa nomes de usuário e senhas, ou usando certificados para autenticação de computador IPsec.

**Autenticação Kerberos**: quando você opta por usar as credenciais de Active Directory para autenticação, o DirectAccess primeiro usa a autenticação Kerberos para o computador e, em seguida, usa a autenticação Kerberos para o usuário. Ao usar esse modo de autenticação, o DirectAccess usa um único túnel de segurança que fornece acesso ao servidor DNS, ao controlador de domínio e a qualquer outro servidor na rede interna

**Autenticação IPsec**: quando você opta por usar a autenticação de dois fatores ou a proteção de acesso à rede, o DirectAccess usa dois túneis de segurança. O assistente de instalação de acesso remoto configura as regras de segurança de conexão no firewall do Windows com segurança avançada. Essas regras especificam as seguintes credenciais ao negociar a segurança IPsec para o servidor de acesso remoto:

-   O túnel de infraestrutura usa credenciais de certificado de computador para a primeira credencial de autenticação e usuário (NTLMv2) para a segunda autenticação. As credenciais de usuário forçam o uso de protocolo Authenticated IP (AuthIP) e fornecem acesso a um servidor DNS e ao controlador de domínio antes que o cliente DirectAccess possa usar credenciais Kerberos para o túnel de intranet.

-   O túnel de intranet usa credenciais de certificado de computador para a primeira autenticação e as credenciais de usuário (Kerberos V5) para a segunda autenticação.

#### <a name="plan-multiple-domains"></a>Planejar múltiplos domínios
A lista de servidores de gerenciamento deverá incluir controladores de domínio de todos os domínios que contém grupos de segurança que incluem computadores cliente do DirectAccess. Ele deve conter todos os domínios que contêm contas de usuário que podem usar computadores configurados como clientes DirectAccess. Isso garante que todos os usuários não localizados no mesmo domínio que o computador cliente usado sejam autenticados com um controlador de domínio no domínio do usuário.

Essa autenticação será automática se os domínios estiverem na mesma floresta. Se houver um grupo de segurança com computadores cliente ou servidores de aplicativos que estejam em florestas diferentes, os controladores de domínio dessas florestas não serão detectados automaticamente. As florestas também não são detectadas automaticamente. Você pode executar os **servidores de gerenciamento de atualizações** de tarefas no **Gerenciamento de acesso remoto** para detectar esses controladores de domínio.

Sempre que possível, os sufixos de nome de domínio comuns devem ser adicionados à NRPT durante a implantação de acesso remoto. Por exemplo, se houvesse dois domínios, domain1.corp.contoso.com e domain2.corp.contoso.com, em vez de adicionar duas entradas na NRPT, você poderia adicionar uma entrada de sufixo de DNS comum, cujo sufixo de nome de domínio seria corp.contoso.com. Isso ocorre automaticamente para domínios na mesma raiz. Os domínios que não estão na mesma raiz devem ser adicionados manualmente.

### <a name="plan-group-policy-object-creation"></a>Planejar a criação de Política de Grupo objeto
Quando você configura o acesso remoto, as configurações do DirectAccess são coletadas em objetos de Política de Grupo (GPOs). Dois GPOs são populados com as configurações do DirectAccess e são distribuídos da seguinte maneira:

-   **GPO do cliente DirectAccess**: esse GPO contém configurações do cliente, incluindo configurações de tecnologia de transição IPv6, entradas NRPT e regras de segurança de conexão para o Firewall do Windows com segurança avançada. O GPO é aplicado a grupos de segurança específicos para os computadores cliente.

-   **GPO do servidor DirectAccess**: esse GPO contém as definições de configuração do DirectAccess que são aplicadas a qualquer servidor configurado como um servidor de acesso remoto em sua implantação. Ele também contém regras de segurança de conexão para o Firewall do Windows com segurança avançada.

> [!NOTE]
> Não há suporte para a configuração de servidores de aplicativos no gerenciamento remoto de clientes DirectAccess porque os clientes não podem acessar a rede interna do servidor DirectAccess onde residem os servidores de aplicativos. A etapa 4 na tela Configuração da instalação de acesso remoto não está disponível para esse tipo de configuração.

Você pode configurar GPOs automaticamente ou manualmente.

**Automaticamente**: quando você especifica que os GPOs são criados automaticamente, um nome padrão é especificado para cada GPO.

**Manualmente**: você pode usar GPOs que foram predefinidos pelo administrador do Active Directory.

Ao configurar seus GPOs, considere os seguintes avisos:

-   Depois de o DirectAccess ser configurado para usar GPOs específicos, não poderá ser configurado para usar GPOs diferentes.

-   Use o procedimento a seguir para fazer backup de todos os objetos de Política de Grupo de acesso remoto antes de executar os cmdlets do DirectAccess:

    [Fazer backup e restaurar a configuração de acesso remoto](https://go.microsoft.com/fwlink/?LinkID=257928).

-   Se você estiver usando GPOs configurados automaticamente ou manualmente, será necessário adicionar uma política para detecção de vínculo lento se os clientes usarem 3G. O caminho para a **política: configurar política de grupo detecção de vínculo lento** é:

    **Configuração do Computador / Políticas / Modelos Administrativos / Sistema / Política de Grupo**.

-   Se as permissões corretas para vincular GPOs não existirem, um aviso será emitido. A operação de acesso remoto continuará, mas a vinculação não ocorrerá. Se esse aviso for emitido, os links não serão criados automaticamente, mesmo que as permissões sejam adicionadas posteriormente. Em vez disso, o administrador precisará criar os links manualmente.

#### <a name="automatically-created-gpos"></a>GPOs criados automaticamente
Considere o seguinte ao usar GPOs criados automaticamente:

Os GPOS criados automaticamente são aplicados de acordo com o local e o destino do link, da seguinte maneira:

-   Para o GPO do servidor DirectAccess, o local e o destino do link apontam para o domínio que contém o servidor de acesso remoto.

-   Quando os GPOs do cliente e do servidor de aplicativos são criados, o local é definido como um único domínio. O nome do GPO é pesquisado em cada domínio e o domínio é preenchido com as configurações do DirectAccess, se existir.

-   O destino do link é definido para a raiz do domínio no qual o GPO foi criado. Um GPO é criado para cada domínio que contém computadores cliente ou servidores de aplicativo, e o GPO é vinculado à raiz dos seus respectivos domínios.

Ao usar GPOs criados automaticamente para aplicar as configurações do DirectAccess, o administrador do servidor de acesso remoto requer as seguintes permissões:

-   Permissões para criar GPOs para cada domínio.

-   Permissões para vincular a todas as raízes de domínio de cliente selecionadas.

-   Permissões para vincular às raízes de domínio do GPO do servidor.

-   Permissões de segurança para criar, editar, excluir e modificar os GPOs.

-   Permissões de leitura de GPO para cada domínio necessário. Essa permissão não é necessária, mas é recomendável porque habilita o acesso remoto para verificar se os GPOs com nomes duplicados não existem quando os GPOs estão sendo criados.

#### <a name="manually-created-gpos"></a>GPOs criados manualmente
Leve em conta as seguintes considerações ao usar GPOs criados manualmente:

-   Os GPOs devem existir antes de o Assistente de Instalação de Acesso Remoto ser executado.

-   Para aplicar as configurações do DirectAccess, o administrador do servidor de acesso remoto requer permissões de segurança completas para criar, editar, excluir e modificar os GPOs criados manualmente.

-   É feita uma pesquisa para um link para o GPO em todo o domínio. Se o GPO não estiver vinculado ao domínio, um link é criado automaticamente na raiz do domínio. Se as permissões necessárias para criar o link não estiverem disponíveis, será emitido um aviso.

#### <a name="recovering-from-a-deleted-gpo"></a>Recuperando um GPO excluído
Se um GPO em um servidor de acesso remoto, cliente ou servidor de aplicativos tiver sido excluído por acidente, a seguinte mensagem de erro será exibida: o **GPO (nome do GPO) não pode ser encontrado**.

Se um backup estiver disponível, você poderá usá-lo para restaurar o GPO. Se não houver nenhum backup disponível, você deverá remover os parâmetros de configuração e configurá-los novamente.

###### <a name="to-remove-configuration-settings"></a>Para remover as definições de configuração

1.  Execute o cmdlet do Windows PowerShell **Uninstall-RemoteAccess**.

2.  Abra o **Gerenciamento de acesso remoto**.

3.  Você verá uma mensagem de erro indicando que o GPO não foi encontrado. Clique em **Remover definições de configuração**. Após a conclusão, o servidor será restaurado para um estado não configurado e você poderá reconfigurar as definições.

