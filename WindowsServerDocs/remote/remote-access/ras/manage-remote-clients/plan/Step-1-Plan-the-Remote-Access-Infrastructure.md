---
title: Etapa 1 planejar a infraestrutura de acesso remoto
description: Este tópico faz parte do guia de clientes do DirectAccess de gerenciar remotamente no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a1ce7af5-f3fe-4fc9-82e8-926800e37bc1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9a3ee39736fb4ee2eb41162db27fed2299c204e5
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281194"
---
# <a name="step-1-plan-the-remote-access-infrastructure"></a>Etapa 1 planejar a infraestrutura de acesso remoto

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

> [!NOTE]
> Windows Server 2016 combina o DirectAccess e o roteamento e acesso remoto (RRAS) em uma única função de acesso remoto.  
  
Este tópico descreve as etapas para planejar uma infraestrutura que você pode usar para configurar um único servidor de acesso remoto para o gerenciamento remoto de clientes DirectAccess. A tabela a seguir lista as etapas, mas essas tarefas de planejamento não precisam ser feitas em uma ordem específica.  
  
|Tarefa|Descrição|  
|----|--------|  
|[Planejar as configurações de topologia e o servidor de rede](#plan-network-topology-and-settings)|Decida onde colocar o servidor de acesso remoto (na borda ou atrás de um dispositivo de tradução de endereço de rede (NAT) ou firewall) e planejar o endereçamento IP e roteamento.|  
|[Planejar requisitos de firewall](#plan-firewall-requirements)|Planejar como permitir a passagem do Acesso Remoto através de firewalls de borda.|  
|[Planejar requisitos de certificado](#plan-certificate-requirements)|Decida se você usar o protocolo Kerberos ou certificados para autenticação de cliente e planejar seus certificados de site.<br/><br/>O IP-HTTPS é um protocolo de transição usado por clientes do DirectAccess para tráfego IPv6 de túnel em redes IPv4. Decida se deseja autenticar IP-HTTPS para o servidor usando um certificado emitido por uma autoridade de certificação (CA) ou usando um certificado autoassinado emitido automaticamente pelo servidor de acesso remoto.|  
|[Planejar os requisitos de DNS](#plan-dns-requirements)|Planeje as configurações do sistema de nome de domínio (DNS) para o servidor acesso remoto, servidores de infraestrutura, as opções de resolução de nome local e conectividade de cliente.| 
|[Planejar a configuração de servidor de local de rede](#plan-the-network-location-server-configuration)|Decida onde colocar o site de servidor de local de rede em sua organização (no servidor de acesso remoto ou um servidor alternativo) e planejar os requisitos de certificado, se o servidor de local de rede ficarão localizado no servidor de acesso remoto. **Observação:** O servidor de local de rede é usado por clientes DirectAccess para determinar se estão localizados na rede interna.|  
|[Planejar configurações de servidores de gerenciamento](#plan-management-servers-configuration)|Plano para servidores de gerenciamento (como servidores de atualização) que são utilizados durante o gerenciamento de cliente remoto. **Observação:** Os administradores podem gerenciar remotamente os computadores cliente do DirectAccess localizados fora da rede corporativa via Internet.|  
|[Planejar os requisitos do Active Directory](#plan-active-directory-requirements)|Planeje seus controladores de domínio, seus requisitos do Active Directory, autenticação de cliente e estrutura de vários domínios.|  
|[Planejar a criação do objeto de diretiva de grupo](#plan-group-policy-object-creation)|Decida quais GPOs são necessários em sua organização e como criar e editar os GPOs.|  
  
## <a name="plan-network-topology-and-settings"></a>Planejar topologia e configurações de rede  
Ao planejar sua rede, você precisa considerar a topologia de adaptador de rede, as configurações para o endereçamento IP e requisitos para ISATAP.  
  
### <a name="plan-network-adapters-and-ip-addressing"></a>Planejar adaptadores de rede e endereçamento IP  
  
1.  Identifica a topologia de adaptador de rede que você deseja usar. Acesso remoto pode ser configurado com qualquer uma das seguintes topologias:  
  
    -   Com dois adaptadores de rede: O servidor de acesso remoto está instalado na borda com um adaptador de rede conectado à Internet e o outro à rede interna.  
  
    -   Com dois adaptadores de rede: O servidor de acesso remoto é instalado atrás de um dispositivo NAT, firewall ou roteador, com um adaptador de rede conectado a uma rede de perímetro e outro à rede interna.  
  
    -   Com um adaptador de rede: O servidor de acesso remoto é instalado atrás de um dispositivo NAT, e o único adaptador de rede está conectado à rede interna.  
  
2.  Identificar os requisitos de endereçamento IP:  
  
    O DirectAccess usa IPv6 com IPsec para criar uma conexão segura entre os computadores cliente do DirectAccess e a rede interna corporativa. Contudo, o DirectAccess não precisa necessariamente de uma conectividade IPv6 com a Internet ou suporte IPv6 nativo em redes internas. Em vez disso, ele automaticamente configura e usa tecnologias de transição IPv6 para encapsular tráfego IPv6 pela Internet IPv4 (6to4, Teredo ou IP-HTTPS) e pela intranet somente IPv4 (NAT64 ou ISATAP). Para uma visão geral dessas tecnologias de transição, confira os seguintes recursos:  
  
    -   [Tecnologias de transição IPv6](https://technet.microsoft.com/library/bb726951.aspx)  
  
    -   [Especificação do protocolo de túnel IP-HTTPS](https://msdn.microsoft.com/library/dd358571.aspx)  
  
3.  Configure os adaptadores e endereçamento necessários conforme a tabela a seguir. Para implantações que estão atrás de um dispositivo NAT usando um único adaptador de rede, configure seus endereços IP usando apenas o **adaptador de rede interno** coluna.  
  
    ||Adaptador de rede externa|Adaptador de rede interno<sup>1, acima</sup>|Requisitos de roteamento|  
    |-|--------------|------------------------|------------|  
    |Internet IPv4 e intranet IPv4|Configure o seguinte:<br/><br/>-Estático dois endereços IPv4 públicos consecutivos com as máscaras de sub-rede apropriadas (necessárias somente para Teredo).<br/>-Um endereço IPv4 para o seu firewall da Internet ou roteador do ISP (provedor) de serviços de Internet local do gateway padrão. **Observação:** O servidor de acesso remoto requer dois endereços IPv4 públicos consecutivos para que ele pode atuar como um servidor Teredo e os clientes Teredo baseados no Windows podem usar o servidor de acesso remoto para detectar o tipo de dispositivo NAT.|Configure o seguinte:<br/><br/>-Um endereço de intranet IPv4 com a máscara de sub-rede apropriada.<br/>-Um sufixo DNS específico da conexão para seu namespace da intranet. Um servidor DNS também deverá ser configurado na interface interna. **Cuidado:** Não configure um gateway padrão em nenhuma interface da intranet.|Para configurar o servidor de acesso remoto para acessar todas as sub-redes na rede IPv4 interna, faça o seguinte:<br/><br/>-Lista os espaços de endereço IPv4 para todos os locais na sua intranet.<br/>– Use o `route add -p` ou `netsh interface ipv4 add route` espaços de endereço de comandos para adicionar o IPv4 como rotas estáticas na tabela de roteamento IPv4 do servidor de acesso remoto.|  
    |Internet IPv6 e intranet IPv6|Configure o seguinte:<br/><br/>-Use a configuração de endereço configurado automaticamente fornecida pelo seu ISP.<br/>– Use o `route print` comando para garantir a existência de uma rota IPv6 padrão que aponta para o roteador do ISP na tabela de roteamento IPv6.<br/>-Determine se os roteadores de intranet e ISP estão usando preferências de roteador padrão conforme descrito no RFC 4191 e se eles forem usar uma preferência padrão mais alta que os roteadores de intranet local. Se as duas situações forem verdadeiras, nenhuma outra configuração para a rota padrão será necessária. A preferência mais alta para o roteador do ISP assegura que a rota IPv6 padrão ativa do servidor de Acesso Remoto aponte para a Internet IPv6.<br/><br/>Como o servidor de Acesso Remoto é um roteador IPv6, caso você tenha uma infraestrutura IPv6 nativa, a interface de Internet também poderá acessar os controladores de domínio na intranet. Nesse caso, adicione filtros de pacote ao controlador de domínio na rede de perímetro que impedem a conectividade com o endereço IPv6 da interface da Internet do servidor de acesso remoto.|Configure o seguinte:<br/><br/>Se você não estiver usando níveis de preferência padrão, configure suas interfaces da intranet usando o `netsh interface ipv6 set InterfaceIndex ignoredefaultroutes=enabled` comando. Esse comando garante que as rotas padrão adicionais que apontam para os roteadores da intranet não sejam acrescentadas à tabela de roteamento de IPv6. Você pode obter o InterfaceIndex das interfaces da intranet na exibição do `netsh interface show interface` comando.|Se você possui uma intranet IPv6, execute o seguinte procedimento para configurar o servidor de Acesso Remoto para chegar a todos os locais IPv6:<br/><br/>-Lista os espaços de endereço IPv6 para todos os locais na sua intranet.<br/>– Use o `netsh interface ipv6 add route` comando para adicionar os espaços de endereço IPv6 como rotas estáticas na tabela de roteamento IPv6 do servidor de acesso remoto.|  
    |Internet IPv4 e intranet IPv6|O servidor de acesso remoto encaminha o tráfego de rota IPv6 padrão usando a interface do adaptador Microsoft 6to4 para uma retransmissão 6to4 na Internet IPv4. Quando o IPv6 nativo não está implantado na rede corporativa, você pode usar o comando a seguir para configurar um servidor de acesso remoto para o endereço IPv4 da retransmissão do Microsoft 6to4 na Internet IPv4: `netsh interface ipv6 6to4 set relay name=<ipaddress> state=enabled`.|||  
  
    > [!NOTE]  
    > -   Se o cliente DirectAccess tiver sido atribuído um endereço IPv4 público, ele usará a tecnologia de retransmissão 6to4 para se conectar à intranet. Se o cliente é atribuído um endereço IPv4 privado, ele usará Teredo. Se o cliente do DirectAccess não puder se conectar ao servidor do DirectAccess com 6to4 ou Teredo, ele usará IP-HTTPS.  
    > -   Para usar o Teredo, você precisa configurar dois endereços IP consecutivos no adaptador de rede voltado para o exterior.  
    > -   Não é possível usar o Teredo se o servidor de acesso remoto tem apenas um adaptador de rede.  
    > -   Os computadores cliente IPv6 nativos podem conectar-se ao servidor de Acesso Remoto pelo IPv6 nativo, sem a necessidade de tecnologia de transição.  
  
### <a name="plan-isatap-requirements"></a>Planejar os requisitos de ISATAP  
ISATAP é necessária para o gerenciamento remoto de DirectAccessclients, para que os servidores de gerenciamento do DirectAccess podem se conectar a clientes DirectAccess localizados na Internet. ISATAP não é necessário para dar suporte a conexões iniciadas pelos computadores cliente do DirectAccess para recursos IPv4 na rede corporativa. NAT64/DNS64 é usado para esta finalidade. Se sua implantação exigir ISATAP, use a tabela a seguir para identificar seus requisitos.  
  
|Cenário de implantação de ISATAP|Requisitos|  
|---------------|--------|  
|Nativo IPv6 da intranet existente (nenhuma ISATAP é necessária)|Com uma infraestrutura IPv6 nativa existente, você especifica o prefixo da organização durante a implantação do acesso remoto, e não configurará o servidor de acesso remoto como um roteador ISATAP. Faça o seguinte:<br/><br/>1.  Para garantir que os clientes DirectAccess sejam acessíveis a partir da intranet, você deve modificar seu roteamento IPv6 para que o tráfego de rota padrão seja encaminhado ao servidor de acesso remoto. Se o seu espaço de endereço IPv6 de intranet usa um endereço diferente de um único prefixo de endereço de IPv6 de 48 bits, você deve especificar o prefixo IPv6 de organização relevantes durante a implantação.<br/>2.  Se você estiver conectado no momento com a Internet IPv6, você deve configurar seu tráfego de rota padrão para que ele seja encaminhado para o servidor de acesso remoto e, em seguida, configurar as rotas e conexões apropriadas no servidor de acesso remoto, para que a rota padrão o tráfego é encaminhado para o dispositivo que está conectado à Internet IPv6.|  
|Implantação de ISATAP existente|Se você tiver uma infraestrutura ISATAP existente, durante a implantação, você será solicitado para o prefixo de 48 bits da organização e não configurará o servidor de acesso remoto como um roteador ISATAP. Para garantir que os clientes DirectAccess sejam acessíveis a partir da intranet, você deve modificar sua infraestrutura de roteamento IPv6 para que o tráfego de rota padrão seja encaminhado ao servidor de acesso remoto. Essa alteração precisa ser feito no roteador ISATAP existente ao qual os clientes da intranet devem já encaminhando o tráfego padrão.|  
|Não há conectividade IPv6 existente|Quando o Assistente de instalação do acesso remoto detecta que o servidor não tem nenhuma conectividade IPv6 nativa ou baseada em ISATAP, ele automaticamente deriva um prefixo de 48 bits baseado em 6to4 para a intranet e configura o servidor de acesso remoto como um roteador ISATAP para fornecer o IPv6 conectividade para hosts ISATAP na sua intranet. (Um prefixo baseado em 6to4 é usado somente se o servidor tem endereços públicos, caso contrário, o prefixo é gerado automaticamente de um intervalo de endereço local exclusivo).<br/><br/>Para usar ISATAP faça o seguinte:<br/><br/>1.  Registre o nome ISATAP em um servidor DNS para cada domínio no qual você deseja habilitar a conectividade baseada em ISATAP, para que o nome ISATAP pode ser resolvido pelo servidor DNS interno para o endereço IPv4 interno do servidor de acesso remoto.<br/>2.  Por padrão, servidores DNS que executam o Windows Server 2012, Windows Server 2008 R2, Windows Server 2008 ou Windows Server 2003 bloqueiam a resolução do nome ISATAP, usando a lista global de consultas não autorizadas. Para habilitar o ISATAP, você deve remover o nome ISATAP da lista de bloqueios. Para obter mais informações, consulte [Remover o ISATAP da lista global de consultas não autorizadas do DNS](https://go.microsoft.com/fwlink/p/?LinkId=168593).<br/><br/>Hosts ISATAP baseados no Windows que podem resolver o nome ISATAP automaticamente configura um endereço com o servidor de acesso remoto da seguinte maneira:<br/><br/>1.  Um endereço IPv6 baseado em ISATAP em uma interface de encapsulamento ISATAP<br/>2.  Uma rota de 64 bits que fornece conectividade aos outros hosts ISATAP na intranet<br/>3.  Uma rota IPv6 padrão que aponta para o servidor de acesso remoto. A rota padrão garante que os hosts ISATAP da intranet podem acessar os clientes DirectAccess<br/><br/>Quando os hosts ISATAP baseados no Windows obtêm um endereço IPv6 baseado em ISATAP, eles começam a usar o tráfego encapsulado pelo ISATAP para se comunicar se o destino é também um host ISATAP. Como o ISATAP usa uma única sub-rede de 64 bits para toda a intranet, a comunicação vai de um modelo de IPv4 segmentado de comunicação, a um modelo de comunicação de única sub-rede com o IPv6. Isso pode afetar o comportamento de alguns serviços de domínio Active Directory (AD DS) e aplicativos que dependem de sua configuração de serviços e Sites do Active Directory. Por exemplo, se você usou o Sites do Active Directory e snap-in Serviços para configurar sites, sub-redes baseadas em IPv4 e transportes entre sites para encaminhar solicitações aos servidores nos sites, essa configuração não é usada pelos hosts ISATAP.<br/><br/><ol><li>Para configurar Sites do Active Directory e serviços para encaminhamento em sites para hosts ISATAP, para cada objeto de sub-rede IPv4, você deve configurar um objeto de sub-rede IPv6 equivalente, no qual o prefixo de endereço IPv6 para a sub-rede expressa o mesmo host de intervalo de ISATAP endereços que a sub-rede IPv4. Por exemplo, para o IPv4 192.168.99.0/24 sub-rede e o endereço ISATAP de 64 bits do prefixo 2002:836b:1:8000::/ 64, o prefixo de endereço IPv6 equivalente para o objeto de sub-rede IPv6 é 2002:836b:1:8000:0:5efe:192.168.99.0/120. Para um arbitrário IPv4 comprimento do prefixo (definido como 24 no exemplo), você pode determinar o comprimento do prefixo IPv6 correspondente da fórmula 96 + IPv4PrefixLength.</li><li>Para os endereços IPv6 dos clientes DirectAccess, adicione o seguinte:<br/><br/><ul><li>Para clientes DirectAccess baseados em Teredo: Uma sub-rede IPv6 para o intervalo 2001:0:WWXX:YYZZ::/ 64, no qual WWXX: YYZZ é a versão hexadecimal com dois-pontos do primeiro endereço IPv4 voltado para a Internet do servidor de acesso remoto. .</li><li>Para clientes DirectAccess baseados em IP-HTTPS: Uma sub-rede IPv6 para o intervalo 2002:WWXX:YYZZ:8100::/ / 56, no qual WWXX: YYZZ é a versão hexadecimal com dois-pontos do primeiro endereço IPv4 voltado para a Internet (w.x.y. z) do servidor de acesso remoto. .</li><li>Para clientes do DirectAccess baseado em 6to4: Uma série de prefixos IPv6 baseados em 6to4 que iniciam com 2002: e representam os prefixos de endereço IPv4 público e regional que são administrados pela IANA (Autoridade para Atribuição de Números da Internet) e registros regionais. O prefixo baseado em 6to4 para uma w.x.y.z/n de prefixo de endereço IPv4 público é 2002:WWXX:YYZZ::/ / [16 + n], no qual WWXX: YYZZ é a versão hexadecimal com dois-pontos w.x.y.z.<br/><br/>        Por exemplo, o intervalo 7.0.0.0/8 é administrado pelo ARIN (Registro Americano para Números da Internet) para a América do Norte. O prefixo baseado em 6to4 correspondente para esse intervalo de endereço IPv6 público é 2002:700::/ / 24. Para obter informações sobre o espaço de endereço público IPv4, consulte [registro de espaço de endereço de IPv4 IANA](https://www.iana.org/assignments/ipv4-address-space/ipv4-address-space.xml). .</li></ul></li></ol>|  
  
> [!IMPORTANT]  
> Certifique-se de que você não tem endereços IP públicos na interface interna do servidor DirectAccess. Se você tiver um endereço IP público na interface interna, a conectividade por meio de ISATAP pode falhar.  
  
### <a name="plan-firewall-requirements"></a>Planejar requisitos de firewall  
Se o servidor de Acesso Remoto estiver atrás de um firewall de borda, as seguintes exceções serão necessárias para o tráfego do Acesso Remoto quando o servidor de Acesso Remoto estiver na Internet IPv4:  
  
-   Para IP-HTTPS: Porta de destino Transmission Control Protocol (TCP) 443 e a porta de origem TCP 443 de saída.  
  
-   Para tráfego Teredo: Porta de destino de protocolo de datagrama (UDP) do usuário 3544 entrada e a porta de origem UDP 3544 de saída.  
  
-   Para tráfego 6to4: Protocolo IP 41 de entrada e saída.  
  
    > [!NOTE]  
    > Para tráfego Teredo e 6to4, essas exceções devem ser aplicadas para os endereços IPv4 públicos consecutivos voltados para a Internet no servidor de Acesso Remoto.  
    >   
    > Para IP-HTTPS, as exceções precisam ser aplicadas no endereço que está registrado no servidor DNS público.  
  
-   Se você implantar o acesso remoto com um único adaptador de rede e instalar o servidor de local de rede no servidor de acesso remoto, a porta TCP 62000.  
  
    > [!NOTE]  
    > Essa exceção está no servidor de acesso remoto e as exceções anteriores estão no firewall de borda.  
  
As exceções a seguir são necessárias para o tráfego de acesso remoto quando o servidor de acesso remoto estiver na Internet IPv6:  
  
-   Protocolo IP 50  
  
-   Entrada da porta de destino UDP 500 e saída da porta de origem UDP 500.  
  
-   Tráfego ICMPv6 de entrada e saído (somente usando Teredo).  
  
Quando você estiver usando firewalls adicionais, aplique as seguintes exceções de firewall de rede interna para o tráfego de acesso remoto:  
  
-   Para ISATAP: Protocolo 41 de entrada e de saída  
  
-   Para todo o tráfego IPv4/IPv6: TCP/UD  
  
-   Para o Teredo: ICMP para todo o tráfego IPv4/IPv6  
  
### <a name="plan-certificate-requirements"></a>Planejar requisitos de certificado  
Há três cenários que exigem certificados ao implantar um único servidor de acesso remoto.  
  
-   **A autenticação IPsec**: Requisitos de certificado para o IPsec incluem um certificado de computador que é usado pelos computadores cliente do DirectAccess ao estabelecerem a conexão IPsec com o servidor de acesso remoto e um certificado de computador que é usado pelos servidores de acesso remoto para estabelecer Conexões IPsec com clientes do DirectAccess.  
  
    Para o DirectAccess no Windows Server 2012, o uso desses certificados IPsec não é obrigatório. Como alternativa, o servidor de acesso remoto pode agir como um proxy para a autenticação Kerberos, sem a necessidade de certificados. Se a autenticação Kerberos for usada, ele funciona por SSL e o protocolo Kerberos usa o certificado que foi configurado para IP-HTTPS. Alguns cenários empresariais (inclusive implantação multissite e autenticação de cliente de senha de uso único) exigem o uso de autenticação de certificado e não a autenticação Kerberos.  
  
-   **Servidor IP-HTTPS**: Quando você configurar o acesso remoto, o servidor de acesso remoto é automaticamente configurado para atuar como o ouvinte da web IP-HTTPS. O site IP-HTTPS precisa de um certificado de site, e os computadores cliente também devem poder contatar o site da CRL (lista de certificados revogados) do certificado.  
  
-   **Servidor de local de rede**: O servidor de local da rede é um site usado para detectar se os computadores cliente encontram-se na rede corporativa. O servidor local da rede precisa de um certificado de site. Clientes do DirectAccess devem poder contatar o site da CRL para o certificado.  
  
Os requisitos de certificação de autoridade (CA) para cada um desses cenários são resumidos na tabela a seguir.  
  
|Autenticação IPsec|Servidor IP-HTTPS|Servidor de local da rede|  
|------------|----------|--------------|  
|Uma CA interna é necessária para emitir certificados de computador para o servidor de acesso remoto e clientes para a autenticação IPsec quando você não usa o protocolo Kerberos para autenticação.|AC interna: você pode usar uma AC interna para emitir o certificado de IP-HTTPS; contudo, deverá verificar se o ponto de distribuição de CRL está disponível externamente|AC interna: Você pode usar uma AC interna para emitir o certificado de site do servidor de local de rede. Verifique se o ponto de distribuição de CRL está altamente disponível pela rede interna.|  
||Certificado autoassinado: Você pode usar um certificado autoassinado para o servidor IP-HTTPS. Um certificado autoassinado não pode ser usado em uma implantação multissite.|Certificado autoassinado: Você pode usar um certificado autoassinado para o site de servidor de local de rede; No entanto, você não pode usar um certificado autoassinado em implantações multissite.|  
||AC pública: É recomendável que você usar uma autoridade de certificação pública para emitir o certificado IP-HTTPS, isso garante que o ponto de distribuição da CRL esteja disponível externamente.||  
  
#### <a name="plan-computer-certificates-for-ipsec-authentication"></a>Planejar certificados de computador para autenticação IPsec  
Se você estiver usando a autenticação IPsec baseada em certificado, o servidor de acesso remoto e os clientes devem obter um certificado de computador. A maneira mais simples para instalar os certificados é usar a diretiva de grupo para configurar o registro automático para certificados de computador. Isso garante que todos os membros do domínio obtenham um certificado de uma AC corporativa. Se você não tiver uma AC corporativa configurada na sua organização, consulte [serviços de certificados do Active Directory](https://technet.microsoft.com/library/cc770357.aspx).  
  
Esse certificado possui os seguintes requisitos:  
  
-   O certificado deve ter a autenticação de cliente uso estendida de chave (EKU).  
  
-   O cliente e os certificados do servidor devem estar relacionada ao mesmo certificado de raiz. Esse certificado raiz deve ser escolhido nas definições de configuração do DirectAccess.  
  
#### <a name="plan-certificates-for-ip-https"></a>Planejar certificados para IP-HTTPS  
O servidor de Acesso Remoto age como um ouvinte do IP-HTTPS, e você deve instalar manualmente um certificado de site HTTPS no servidor. Leve em conta as seguintes considerações ao planejar:  
  
-   É recomendável usar uma AC pública, para que as CRLs estejam prontamente disponíveis.  
  
-   No campo assunto, especifique o endereço IPv4 do adaptador da Internet do servidor de acesso remoto ou o FQDN da URL IP-HTTPS (o endereço ConnectTo). Se o servidor de Acesso Remoto encontrar-se atrás de um dispositivo NAT, o nome ou endereço público do dispositivo NAT deverá ser especificado.  
  
-   O nome comum do certificado deverá ser corresponder ao nome do site IP-HTTPS.  
  
-   Para o **uso avançado de chave** campo, use o identificador de objeto (OID) de autenticação do servidor.  
  
-   No campo **Pontos de Distribuição da Lista de Certificados Revogados**, especifique um ponto de distribuição da CRL que seja acessível aos clientes do DirectAccess conectados à Internet.  
  
    > [!NOTE]  
    > Isso só é necessário para clientes que executam o Windows 7.  
  
-   O certificado IP-HTTPS deve ter uma chave privada.  
  
-   O certificado IP-HTTPS deve ser importado diretamente para o repositório pessoal.  
  
-   Os certificados IP-HTTPS podem ter curingas no nome.  
  
#### <a name="plan-website-certificates-for-the-network-location-server"></a>Planejar certificados de site para o servidor de local da rede  
Considere o seguinte ao planejar o site de servidor de local de rede:  
  
-   No campo **Assunto**, especifique o endereço IP da interface da intranet do servidor de local de rede do FQDN da URL de rede local.  
  
-   Para o **uso avançado de chave** campo, use o OID de autenticação de servidor.  
  
-   Para o **pontos de distribuição da CRL** campo, use um ponto de distribuição da CRL que seja acessível pelos clientes DirectAccess que estão conectados à intranet. Esse ponto de distribuição da CRL não deverá ser acessível de fora da rede interna.  
  
> [!NOTE]  
> Certifique-se de que os certificados do servidor de local de rede e IP-HTTPS tem um nome de assunto. Se o certificado usa um nome alternativo, ele não será aceito pelo Assistente de acesso remoto.  
  
#### <a name="plan-dns-requirements"></a>Planejar os requisitos de DNS  
Esta seção explica os requisitos de DNS para clientes e servidores em uma implantação de acesso remoto.  
  
##### <a name="directaccess-client-requests"></a>Solicitações de cliente do DirectAccess  
O DNS é usado para resolver solicitações de computadores cliente do DirectAccess não localizados na rede interna. Clientes DirectAccess tentam se conectar ao servidor de local de rede do DirectAccess para determinar se eles estão localizados na Internet ou na rede corporativa.  
  
-   Se a conexão for bem-sucedida, os clientes forem determinados como estando na intranet, DirectAccess não será usado e as solicitações de cliente serão resolvidas usando o servidor DNS que está configurado no adaptador de rede do computador cliente.  
  
-   Se a conexão não for bem-sucedida, presume-se que os clientes estejam na Internet. Clientes do DirectAccess usarão a NRPT (tabela de políticas de resolução de nomes) para determinar qual servidor DNS usar ao resolver solicitações de nome. Você pode especificar que os clientes devem usar o DirectAccess DNS64 para resolver nomes ou um servidor DNS interno alternativo.  
  
Ao realizar a resolução do nome, a NRPT é usada pelos clientes do DirectAccess para identificar como lidar com uma solicitação. Os clientes solicitam um FQDN ou nome de rótulo único, como <https://internal>. Se um nome de rótulo único for solicitado, um sufixo do DNs é agregado para criar um FQDN. Se a consulta DNS corresponde a uma entrada na NRPT e DNS4 ou um servidor DNS da intranet é especificado para a entrada, a consulta é enviada para resolução de nome usando o servidor especificado. Se houver uma correspondência, mas nenhum servidor DNS for especificado, uma regra de isenção e normal é aplicada a resolução de nome.  
  
Quando um novo sufixo for adicionado à NRPT no console de gerenciamento de acesso remoto, os servidores DNS padrão para o sufixo podem ser descobertos automaticamente, clicando na **detectar** botão. A detecção automática funciona da seguinte maneira:  
  
-   Se a rede corporativa for baseada em IPv4 ou então usar IPv4 e IPv6, o endereço padrão é o endereço do DNS64 do adaptador interno no servidor de acesso remoto.  
  
-   Se a rede corporativa for baseada em IPv6, o endereço padrão será o endereço IPv6 dos servidores DNS na rede corporativa.  
  
##### <a name="infrastructure-servers"></a>Servidores de infraestrutura  
  
-   **Servidor de local de rede**  
  
    Os clientes DirectAccess tentam acessar o servidor de local de rede para determinar se estão em uma rede interna. Os clientes na rede interna devem ser capazes de resolver o nome do servidor de local de rede, e eles devem ser impedidos de resolução do nome quando se encontrarem na Internet. Para garantir que isso ocorra, o FQDN do servidor do local de rede é adicionado, por padrão, a uma regra de exceção do NRPT. Quando você configurar o Acesso Remoto, as seguintes regras também serão criadas automaticamente:  
  
    -   Regra de domínio raiz ou o nome de domínio do servidor de acesso remoto e os endereços IPv6 que correspondem aos servidores DNS da intranet configurados no servidor de acesso remoto de sufixo de DNS. Por exemplo, se o servidor de Acesso Remoto for membro do domínio corp.contoso.com, é criada uma regra para o sufixo de DNS corp.contoso.com.  
  
    -   Uma regra de isenção para o FQDN do servidor de local de rede. Por exemplo, se a URL de servidor de local de rede é <https://nls.corp.contoso.com>, uma regra de isenção é criada para nls.corp.contoso.com do FQDN.  
  
-   **Servidor IP-HTTPS**  
  
    O servidor de acesso remoto age como ouvinte do IP-HTTPS e usa seu certificado de servidor para autenticar clientes IP-HTTPS. O nome do IP-HTTPS deve ser resolvível pelos clientes DirectAccess que usam servidores DNS públicos.  
  
##### <a name="connectivity-verifiers"></a>Verificadores de conectividade  
O Acesso Remoto cria uma sonda da Web padrão usada pelos computadores cliente do DirectAccess para verificar a conectividade com a rede interna. Para garantir que a sonda funcione como esperado, os nomes a seguir devem ser registrados manualmente no DNS:  
  
-   **DirectAccess-webprobehost** deve resolver para o endereço IPv4 interno do servidor de acesso remoto, ou o endereço IPv6 em um ambiente somente com IPv6.  
  
-   **DirectAccess-corpconnectivityhost** deve resolver para o endereço do host local (loopback). Você deve criar registros A e AAAA. O valor do registro é 127.0.0.1 e o valor do registro AAAA é construído a partir do prefixo NAT64 com os últimos 32 bits como 127.0.0.1. O prefixo NAT64 pode ser recuperado executando o **Get-netnatTransitionConfiguration** cmdlet do Windows PowerShell.  
  
    > [!NOTE]  
    > Isso é válido somente em ambientes compatíveis somente com IPv4. Em um IPv4 e IPv6 ou um ambiente somente com IPv6, criar somente um registro AAAA com o endereço IP do loopback:: 1.  
  
Você pode criar verificadores de conectividade adicionais usando outros endereços da web via HTTP ou PING. Uma entrada de DNS deverá existir para cada verificador de conectividade.  
  
##### <a name="dns-server-requirements"></a>Requisitos de servidor DNS  
  
-   Para clientes DirectAccess, você deve usar um servidor DNS executando o Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows Server 2003 ou qualquer servidor DNS que dá suporte a IPv6.  
  
-   Você deve usar um servidor DNS que dá suporte a atualizações dinâmicas. Você pode usar servidores DNS que não dão suporte a atualizações dinâmicas, mas, em seguida, as entradas devem ser atualizadas manualmente.  
  
-   O FQDN para pontos de distribuição da CRL deve ser resolvível usando servidores DNS da Internet. Por exemplo, se URL <https://crl.contoso.com/crld/corp-DC1-CA.crl> está no **pontos de distribuição da CRL** campo do certificado IP-HTTPS do servidor de acesso remoto, você deve garantir que o FQDN CRL.contoso.com possa ser resolvido usando servidores DNS da Internet.  
  
#### <a name="plan-for-local-name-resolution"></a>Plano para resolução de nomes locais  
Quando você estiver planejando para resolução de nomes locais, considere o seguinte:  
  
##### <a name="nrpt"></a>NRPT  
Talvez seja necessário criar regras de NRPT (tabela) de política de resolução de nome adicionais nas seguintes situações:  
  
-   Você precisa adicionar mais sufixos DNS para seu namespace da intranet.  
  
-   Se os FQDNs dos pontos de distribuição da CRL baseiam-se no namespace da intranet, você deverá adicionar regras de isenção para os FQDNs dos pontos de distribuição da CRL.  
  
-   Se você tiver um ambiente DNS com partição de rede, você deve adicionar regras de isenção para os nomes de recursos para o qual você deseja que os clientes DirectAccess que estão localizados na Internet para acessar a versão de Internet, em vez da versão da intranet.  
  
-   Se você estiver redirecionando o tráfego para um site externo por meio de seus servidores de proxy da web de intranet, o site externo está disponível somente na intranet. Ele usa os endereços dos seus servidores de proxy da web para permitir solicitações de entrada. Nessa situação, adicione uma regra de isenção para o FQDN do site externo e especificar que a regra usa o servidor de proxy da web da intranet em vez dos endereços IPv6 dos servidores DNS da intranet.  
  
    Por exemplo, digamos que você está testando um site externo chamado test.contoso.com. Esse nome não pode ser resolvido por meio de servidores DNS da Internet, mas o servidor de proxy web Contoso sabe como resolver o nome e direcionar as solicitações para o site para o servidor web externo. Para evitar que os usuários que não estiverem na intranet da Contoso acessem o site, o site externo permitirá solicitações somente do endereço da Internet IPv4 do proxy Web da Contoso. Assim, os usuários da intranet podem acessar o site porque eles estão usando o proxy Web da Contoso, mas os usuários do DirectAccess não é possível porque eles não estão usando o proxy Web da Contoso. Com a configuração de uma regra de isenção de NRPT para test.contoso.com que usar o proxy Web da Contoso, as solicitações de página da Web para test.contoso.com serão roteadas para o servidor proxy Web da intranet na Internet IPv4.  
  
##### <a name="single-label-names"></a>Nomes de rótulo único  
Único de nomes de rótulo, tal como <https://paycheck>, às vezes, são usados para servidores da intranet. Se um nome de rótulo único for solicitado e uma lista de pesquisa de sufixos DNS estiver configurada, os sufixos DNS na lista serão acrescentados ao nome de rótulo único. Por exemplo, quando um usuário em um computador que é um membro do domínio corp.contoso.com digitar <https://paycheck> no navegador da web, o FQDN é criado como o nome é paycheck.corp.contoso.com. Por padrão, o sufixo agregado com base no sufixo DNS primário do computador cliente.  
  
> [!NOTE]  
> Em um cenário de espaço de nomes separados (em que um ou mais computadores de domínio tem um sufixo DNS que não corresponde ao qual os computadores são membros de domínio do Active Directory), você deve garantir que a lista de pesquisa seja personalizada para incluir todos os sufixos necessários. Por padrão, o Assistente de acesso remoto configura o nome DNS do Active Directory como o sufixo DNS primário no cliente. Certifique-se de adicionar o sufixo de DNS usado pelos clientes para a resolução de nome.  
  
Se vários domínios e serviço de nome na Internet do Windows (WINS) são implantados em sua organização, e você está se conectando remotamente, nomes únicos podem ser resolvidos da seguinte maneira:  
  
-   Implantando uma zona de pesquisa direta do WINS no DNS. Ao tentar resolver computername.dns.zone1.corp.contoso.com, a solicitação é direcionada para o servidor WINS que está usando apenas o nome do computador. O cliente pensa que está emitindo uma solicitação de registros DNS A regular, mas é, na verdade, uma solicitação NetBIOS.  
  
    Para obter mais informações, consulte [Gerenciando uma zona de pesquisa direta](https://technet.microsoft.com/library/cc816891(WS.10).aspx).  
  
-   Adicionando um sufixo DNS (por exemplo, o dns.zone1.corp.contoso.com) para o GPO de domínio padrão.  
  
##### <a name="split-brain-dns"></a>DNS com partição de rede  
DNS com partição de rede refere-se ao uso do mesmo domínio DNS para resolução de nome de Internet e intranet.  
  
Para implantações de DNS com partição de rede, você deve listar os FQDNs duplicados na Internet e intranet e decidir quais recursos o cliente DirectAccess deve alcance a intranet ou a versão de Internet. Quando desejar que os clientes DirectAccess acessem a versão de Internet, você deve adicionar o FQDN correspondente como uma regra de isenção à NRPT para cada recurso.  
  
Em um ambiente DNS com partição de rede, se você quiser que as duas versões do recurso esteja disponível, configure os recursos de intranet com nomes que não duplicam os nomes que são usados na Internet. Em seguida, instrua os usuários a usar o nome alternativo quando eles acessarem os recursos na intranet. Por exemplo, configure www.internal.contoso.com para o nome interno de www.contoso.com.  
  
Em um ambiente de DNS sem partição de rede, o namespace da Internet é diferente do namespace da intranet. Por exemplo, a Contoso Corporation usa contoso.com na Internet e corp.contoso.com na intranet. Como todos os recursos da intranet usam o sufixo DNS corp.contoso.com, a regra da NRPT para corp.contoso.com roteia todas as consultas de nome DNS por recursos de intranet para servidores DNS da intranet. Consultas DNS para nomes com o sufixo contoso.com não correspondem a regra de namespace corp.contoso.com da intranet na NRPT, e elas são enviadas para os servidores DNS da Internet. Com uma implantação de DNS com partição de rede, não é necessário fazer configurações adicionais na NRPT, pois não há duplicação dos FQDNs dos recursos da Internet e da intranet. Os clientes DirectAccess podem acessar os recursos da Internet e da intranet da organização.  
  
##### <a name="plan-local-name-resolution-behavior-for-directaccess-clients"></a>Planejar o comportamento de resolução de nomes locais para clientes do DirectAccess  
Se um nome não pode ser resolvido com DNS, o serviço cliente DNS no Windows Server 2012, Windows 8, Windows Server 2008 R2 e Windows 7 pode usar a resolução de nomes locais, com o Link-Local Multicast nome LLMNR (resolução) e o NetBIOS nos protocolos TCP/IP, para resolver o  nome na sub-rede local. Em geral, a resolução de nomes locais é necessária para a conectividade ponto a ponto, quando o computador está localizado em redes privadas, como redes domésticas com uma única sub-rede.  
  
Quando o serviço cliente DNS executa a resolução de nomes locais para os nomes de servidor de intranet e o computador está conectado a uma sub-rede compartilhada na Internet, usuários mal-intencionados podem capturar LLMNR e NetBIOS sobre mensagens de TCP/IP para determinar os nomes de servidor de intranet. Na página do Assistente de instalação do servidor de infraestrutura de DNS, você pode configurar o comportamento de resolução de nomes locais com base nos tipos de respostas recebidas dos servidores DNS da intranet. As seguintes opções estão disponíveis:  
  
-   **Usar a resolução de nome local se o nome não existe no DNS**: Esta opção é a mais segura, pois o cliente do DirectAccess executará a resolução de nomes locais somente para os nomes de servidor que não puderem ser resolvidos pelos servidores DNS da intranet. Se os servidores DNS da intranet puderem ser acessados, os nomes dos servidores da intranet serão resolvidos. Se os servidores DNS da intranet não puderem ser acessados ou se houver outros tipos de erros de DNS, não haverá uma perda dos nomes do servidor da intranet para a sub-rede por meio da resolução de nomes locais.  
  
-   **Usar a resolução de nome local se o nome não existe no DNS ou servidores DNS estiverem inacessíveis quando o computador cliente está em uma rede privada (recomendada)** : Esta opção é recomendada, pois permite o uso da resolução de nomes locais em uma rede privada somente quando os servidores DNS da intranet não forem acessíveis.  
  
-   **Usar a resolução de nome local para qualquer tipo de erro de resolução DNS (menos seguro)** : Esta opção é a menos segura, pois pode haver uma perda dos nomes dos servidores da rede da intranet para a sub-rede local por meio da resolução de nomes locais.  
  
#### <a name="plan-the-network-location-server-configuration"></a>Planejar a configuração de servidor de local de rede  
O servidor de local da rede é um site usado para detectar se os clientes DirectAccess encontram-se na rede corporativa. Os clientes na rede corporativa não usam o DirectAccess para acessar recursos internos; mas, em vez disso, eles se conectam diretamente.  
  
O site de servidor de local de rede pode ser hospedado no servidor de acesso remoto ou em outro servidor na sua organização. Se você hospedar o servidor de local de rede no servidor de acesso remoto, o site será criado automaticamente quando você implanta o acesso remoto. Se você hospedar o servidor de local de rede em outro servidor executando o sistema operacional Windows, certifique-se de que os serviços de informações da Internet (IIS) está instalado no servidor e que o site é criado. Acesso remoto não configura as configurações no servidor de local de rede.  
  
Certifique-se de que o site de servidor de local de rede atende aos seguintes requisitos:  
  
-   Tem um certificado de servidor HTTPS.  
  
-   Tenha alta disponibilidade para computadores na rede interna.  
  
-   Não é acessível para computadores cliente DirectAccess na Internet.  
  
-  
  
Além disso, considere os seguintes requisitos para clientes quando você estiver configurando seu site de servidor de local de rede:  
  
-   Os computadores cliente do DirectAccess devem confiar na AC que emitiu o certificado do servidor para o site do servidor do local de rede.  
  
-   Os computadores cliente do DirectAccess na rede interna devem poder resolver o nome do site do servidor do local de rede.  
  
##### <a name="plan-certificates-for-the-network-location-server"></a>Planejar certificados para o servidor de local de rede  
Quando você obtiver o certificado do site a ser usado para o servidor de local de rede, considere o seguinte:  
  
-   No cmapo **Assunto**, especifique o endereço IP da interface da intranet do servidor de local de rede do FQDN da URL de rede local.  
  
-   Para o **uso avançado de chave** campo, use o OID de autenticação de servidor.  
  
-   O certificado de servidor de local de rede deve ser verificado em relação a uma lista de certificados revogados (CRL). Para o **pontos de distribuição da CRL** campo, use um ponto de distribuição da CRL que seja acessível pelos clientes DirectAccess que estão conectados à intranet. Esse ponto de distribuição da CRL não deverá ser acessível de fora da rede interna.  
  
##### <a name="plan-dns-for-the-network-location-server"></a>Planejar DNS para o servidor de local de rede  
Os clientes DirectAccess tentam acessar o servidor de local de rede para determinar se estão em uma rede interna. Os clientes na rede interna devem poder resolver o nome do servidor de local de rede, mas não deverão resolver o nome quando se encontrarem na Internet. Para garantir que isso ocorra, o FQDN do servidor do local de rede é adicionado, por padrão, a uma regra de exceção da NRPT.  
  
### <a name="plan-management-servers-configuration"></a>Planejar a configuração de servidores de gerenciamento  
Os clientes DirectAccess iniciam a comunicação com servidores de gerenciamento que oferecem serviços como o Windows Update e atualizações de antivírus. Clientes DirectAccess também usam o protocolo Kerberos para autenticar em controladores de domínio antes de acessarem a rede interna. Durante o gerenciamento dos clientes DirectAccess, os servidores de gerenciamento comunicam-se com os computadores cliente para realizar funções de gerenciamento como avaliações de inventário de software ou hardware. O Acesso Remoto pode descobrir alguns servidores de gerenciamento automaticamente, incluindo:  
  
-   Controladores de domínio: A descoberta automática de controladores de domínio é executada para os domínios que contêm os computadores cliente e para todos os domínios na mesma floresta que o servidor de acesso remoto.  
  
-   Servidores do System Center Configuration Manager  
  
Controladores de domínio e o System Center Configuration Manager servidores são detectados automaticamente na primeira vez que o DirectAccess é configurado. Os controladores de domínio detectados não são exibidos no console do, mas as configurações podem ser recuperadas usando cmdlets do Windows PowerShell. Se o controlador de domínio ou servidores do System Center Configuration Manager forem modificados, clicando em **servidores de gerenciamento de atualização** no console do atualiza a lista de servidores de gerenciamento.  
  
**Requisitos do servidor de gerenciamento**  
  
-   Servidores de gerenciamento devem estar acessíveis por meio do túnel de infraestrutura. Quando você configurar o Acesso Remoto, os servidores adicionados à lista de servidores de gerenciamento poderão ser automaticamente acessados por esse túnel.  
  
-   Servidores de gerenciamento que iniciam conexões para os clientes DirectAccess totalmente devem oferecer suporte a IPv6, por meio de um endereço IPv6 nativo ou usando um endereço atribuído por ISATAP.  
  
### <a name="plan-active-directory-requirements"></a>Planejar os requisitos do Active Directory  
Acesso remoto usa o Active Directory da seguinte maneira:  
  
-   **Autenticação**: O túnel de infraestrutura usa a autenticação NTLMv2 para a conta de computador que está se conectando ao servidor de acesso remoto e a conta deve ser em um domínio do Active Directory. O túnel de intranet usa autenticação Kerberos para o usuário para criar o túnel de intranet.  
  
-   **Objetos de diretiva de grupo**: Acesso remoto reúne as definições de configuração nos objetos de diretiva de grupo (GPOs), que são aplicados a servidores de acesso remoto, clientes e servidores de aplicativos internos.  
  
-   **Grupos de segurança**: Acesso remoto usa grupos de segurança para reunir e identificar computadores cliente do DirectAccess. GPOs são aplicados aos grupos de segurança necessárias.  
  
Quando você planejar um ambiente do Active Directory para uma implantação de acesso remoto, considere os seguintes requisitos:  
  
-   Pelo menos um controlador de domínio está instalado no sistema operacional Windows Server 2012, Windows Server 2008 R2 Windows Server 2008 ou Windows Server 2003.  
  
    Se o controlador de domínio estiver em uma rede de perímetro (e, portanto, acessível do adaptador de rede para a Internet do servidor de acesso remoto), impedir que o servidor de acesso remoto acesse-o. Você precisará adicionar filtros de pacote no controlador de domínio para evitar a conectividade com o endereço IP do adaptador da Internet.  
  
-   O servidor de Acesso Remoto deve ser um membro do domínio.  
  
-   Os clientes do DirectAccess devem ser membros do domínio. Os clientes podem pertencer a:  
  
    -   Qualquer domínio na mesma floresta que o servidor de Acesso Remoto.  
  
    -   Qualquer domínio que tenha uma relação de confiança bidirecional com o domínio do servidor de Acesso Remoto.  
  
    -   Qualquer domínio em uma floresta que tenha uma relação de confiança bidirecional com a floresta do domínio do servidor de acesso remoto.  
  
> [!NOTE]  
> -   O servidor de Acesso Remoto não deve ser um controlador de domínio.  
> -   O controlador de domínio do Active Directory é usado para acesso remoto não pode estar acessível do adaptador externo da Internet do servidor de acesso remoto (o adaptador não deve estar no perfil de domínio do Firewall do Windows).  
  
#### <a name="plan-client-authentication"></a>Planejar autenticação de cliente  
No acesso remoto no Windows Server 2012, você pode escolher entre usar autenticação integrada do Kerberos, que usa nomes de usuário e senhas, ou usando certificados para autenticação IPsec de computadores.  
  
**A autenticação Kerberos**: Quando você opta por usar as credenciais do Active Directory para autenticação, primeiro, o DirectAccess usa a autenticação Kerberos para o computador e, em seguida, ele usa a autenticação Kerberos para o usuário. Ao usar esse modo de autenticação, o DirectAccess usa um único túnel de segurança que fornece acesso a qualquer outro servidor, o controlador de domínio e o servidor DNS na rede interna  
  
**A autenticação IPsec**: Quando você opta por usar a autenticação de dois fatores ou proteção de acesso à rede, o DirectAccess usa dois túneis de segurança. O Assistente de configuração de acesso remoto configura as regras de segurança de conexão no Firewall do Windows com segurança avançada. Essas regras especificam as credenciais a seguir durante a negociação de segurança de IPsec para o servidor de acesso remoto:  
  
-   O túnel de infraestrutura usa as credenciais de certificado de computador para a primeira autenticação e credenciais de usuário (NTLMv2) para a segunda autenticação. As credenciais do usuário forçam o uso do protocolo de Internet autenticado (AuthIP), e fornecem acesso a um servidor DNS e controlador de domínio antes que o cliente do DirectAccess pode usar as credenciais do Kerberos para o túnel de intranet.  
  
-   O túnel de intranet usa as credenciais de certificado de computador para a primeira autenticação e credenciais de usuário (Kerberos V5) para a segunda autenticação.  
  
#### <a name="plan-multiple-domains"></a>Planejar múltiplos domínios  
A lista de servidores de gerenciamento deverá incluir controladores de domínio de todos os domínios que contém grupos de segurança que incluem computadores cliente do DirectAccess. Ele deve conter todos os domínios que contêm as contas de usuário que podem usar computadores configurados como clientes DirectAccess. Isso garante que todos os usuários não localizados no mesmo domínio que o computador cliente usado sejam autenticados com um controlador de domínio no domínio do usuário.  
  
Essa autenticação é automática, se os domínios estiverem na mesma floresta. Se houver um grupo de segurança com computadores clientes ou servidores de aplicativos que estiverem em florestas diferentes, os controladores de domínio de tais florestas não são detectados automaticamente. Florestas também não detectadas automaticamente. Você pode executar a tarefa **servidores de gerenciamento de atualização** na **gerenciamento de acesso remoto** para detectar esses controladores de domínio.  
  
Sempre que possível, sufixos de nome de domínio comuns devem ser adicionados à NRPT durante a implantação do acesso remoto. Por exemplo, se houvesse dois domínios, domain1.corp.contoso.com e domain2.corp.contoso.com, em vez de adicionar duas entradas na NRPT, você poderia adicionar uma entrada de sufixo de DNS comum, cujo sufixo de nome de domínio seria corp.contoso.com. Isso ocorre automaticamente para domínios na mesma raiz. Domínios que não estão na mesma raiz devem ser adicionados manualmente.  
  
### <a name="plan-group-policy-object-creation"></a>Planejar a criação do objeto de diretiva de grupo  
Quando você configurar o acesso remoto, as configurações do DirectAccess são coletadas em objetos de diretiva de grupo (GPOs). Dois GPOs são populados com as configurações do DirectAccess, e eles são distribuídos como segue:  
  
-   **GPO do cliente DirectAccess**: Este GPO contém configurações do cliente, incluindo configurações de tecnologia de transição IPv6, entradas da NRPT e regras de segurança de conexão para o Firewall do Windows com segurança avançada. O GPO é aplicado a grupos de segurança específicos para os computadores cliente.  
  
-   **GPO do servidor DirectAccess**: Este GPO contém configurações do DirectAccess são aplicadas a qualquer servidor configurado como um servidor de acesso remoto em sua implantação. Ele também contém regras de segurança de conexão para o Firewall do Windows com segurança avançada.  
  
> [!NOTE]  
> Não há suporte para configuração de servidores de aplicativos no gerenciamento remoto de clientes DirectAccess porque os clientes não podem acessar a rede interna do servidor DirectAccess onde residem os servidores de aplicativos. Etapa 4 na tela de configuração de instalação do acesso remoto não está disponível para esse tipo de configuração.  
  
Você pode configurar GPOs manualmente ou automaticamente.  
  
**Automaticamente**: Quando você especifica que os GPOs são criados automaticamente, um nome padrão é especificado para cada GPO.  
  
**Manualmente**: Você pode usar GPOs predefinidos pelo administrador do Active Directory.  
  
Quando você configura seus GPOs, considere os seguintes avisos:  
  
-   Depois de o DirectAccess ser configurado para usar GPOs específicos, não poderá ser configurado para usar GPOs diferentes.  
  
-   Use o procedimento a seguir para fazer backup de todos os objetos de diretiva de grupo de acesso remoto antes de executar cmdlets do DirectAccess:  
  
    [Fazer backup e restaurar a configuração de acesso remoto](https://go.microsoft.com/fwlink/?LinkID=257928).  
  
-   Se você estiver usando automaticamente ou GPOs configurados manualmente, você precisa adicionar uma política para detecção de vínculo lento se os clientes forem usar 3G. O caminho para **política: Configurar a detecção de vínculo lento de diretiva de grupo** é:  
  
    **Configuração do Computador / Políticas / Modelos Administrativos / Sistema / Política de Grupo**.  
  
-   Se as permissões corretas para vinculação de GPOs não existirem, um aviso será emitido. A operação de acesso remoto continuará, mas não ocorrerá a vinculação. Se esse aviso é emitido, links não serão criados automaticamente, mesmo se as permissões forem adicionadas posteriormente. Em vez disso, o administrador precisará criar os links manualmente.  
  
#### <a name="automatically-created-gpos"></a>GPOs criados automaticamente  
Considere o seguinte ao usar automaticamente criar GPOs:  
  
GPOS criados automaticamente são aplicados de acordo com o local e o destino do link, da seguinte maneira:  
  
-   GPO de servidor do DirectAccess, o local e o destino do link apontam para o domínio que contém o servidor de acesso remoto.  
  
-   Quando o cliente e servidor de aplicativos GPOs são criados, o local é definido em um domínio único. O nome do GPO é pesquisado em cada domínio e o domínio é preenchido com as configurações do DirectAccess, se existir.  
  
-   O destino do link é definido para a raiz do domínio no qual o GPO foi criado. Um GPO é criado para cada domínio que contém computadores cliente ou servidores de aplicativo, e o GPO é vinculado à raiz dos seus respectivos domínios.  
  
Quando usar GPOs criados automaticamente para aplicar as configurações do DirectAccess, o administrador do servidor de acesso remoto requer as seguintes permissões:  
  
-   Permissões para criar GPOs para cada domínio.  
  
-   Permissões para vincular a todas as raízes de domínio do cliente selecionado.  
  
-   Permissões para vincular às raízes de domínio de GPO do servidor.  
  
-   Permissões de segurança para criar, editar, excluir e modificar os GPOs.  
  
-   GPO permissões de leitura para cada domínio necessário. Essa permissão não é necessária, mas é recomendável porque ela permite o acesso remoto verificar se os GPOs com nomes duplicados não existe quando os GPOs estão sendo criados.  
  
#### <a name="manually-created-gpos"></a>GPOs criados manualmente  
Leve em conta as seguintes considerações ao usar GPOs criados manualmente:  
  
-   Os GPOs devem existir antes de o Assistente de Instalação de Acesso Remoto ser executado.  
  
-   Para aplicar as configurações do DirectAccess, o administrador do servidor de acesso remoto requer permissões de segurança completa para criar, editar, excluir e modificar os GPOs criados manualmente.  
  
-   Uma pesquisa é feita para um link para o GPO em todo o domínio. Se o GPO não estiver vinculado ao domínio, um link é criado automaticamente na raiz do domínio. Se as permissões necessárias para criar o link não estiverem disponíveis, será emitido um aviso.  
  
#### <a name="recovering-from-a-deleted-gpo"></a>Recuperando um GPO excluído  
Se um GPO em um servidor de acesso remoto, cliente ou servidor de aplicativos tiver sido excluído por engano, a seguinte mensagem de erro será exibida: **Não é possível encontrar o GPO (nome do GPO)** .  
  
Se um backup estiver disponível, você poderá usá-lo para restaurar o GPO. Se não houver nenhum backup disponível, você deve remover as definições de configuração e configurá-los novamente.  
  
###### <a name="to-remove-configuration-settings"></a>Para remover as definições de configuração  
  
1.  Execute o cmdlet do Windows PowerShell **Uninstall-RemoteAccess**.  
  
2.  Abra **gerenciamento de acesso remoto**.  
  
3.  Você verá uma mensagem de erro indicando que o GPO não foi encontrado. Clique em **Remover definições de configuração**. Após a conclusão, o servidor será restaurado para um estado não configurado, e você pode reconfigurar as configurações.  
  


