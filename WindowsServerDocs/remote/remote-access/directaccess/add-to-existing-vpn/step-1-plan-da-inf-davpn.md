---
title: Etapa 1 plano a infraestrutura do DirectAccess
description: Este tópico faz parte do guia adicionar DirectAccess a uma implantação de acesso remoto existente (VPN) para Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4ca50ea8-6987-4081-acd5-5bf9ead62acd
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 34f6ec2b50e38042a7530e94915ed6d29d5f76a6
ms.sourcegitcommit: d84dc3d037911ad698f5e3e84348b867c5f46ed8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2019
ms.locfileid: "66266792"
---
# <a name="step-1-plan-directaccess-infrastructure"></a>Etapa 1 plano a infraestrutura do DirectAccess

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

A primeira etapa do planejamento de uma implantação básica do Acesso Remoto em um único servidor é executar o planejamento para infraestrutura necessária para implantação. Este tópico descreve as etapas de planejamento de infraestrutura:  
  
|Tarefa|Descrição|  
|----|--------|  
|Planejar topologia e configurações de rede|Decidir onde colocar o servidor de Acesso Remoto (na borda, atrás de um dispositivo NAT [Conversão de Endereços de Rede] ou de um firewall) e planejar o endereçamento IP e o roteamento.|  
|Planejar requisitos de firewall|Planejar como permitir a passagem do Acesso Remoto através de firewalls de borda.|  
|Planejar requisitos de certificado|O Acesso Remoto pode usar Kerberos ou certificados para autenticação cliente. Nesta implantação básica de Acesso Remoto, o Kerberos é configurado automaticamente e a autenticação é obtida usando um certificado autoassinado emitido automaticamente pelo servidor de Acesso Remoto.|  
|Planejar os requisitos de DNS|Planejar configurações de DNS para o servidor de Acesso Remoto, os servidores de infraestrutura, as opções de resolução de nome local e a conectividade de cliente.|  
|Planejar o Active Directory|Planeje seus controladores de domínio e os requisitos do Active Directory.|  
|Planejar objetos de política de grupo|Decida quais GPOs são necessários em sua organização e como criar ou editar os GPOs.|  
  
As tarefas de planejamento não precisam ser feitas em uma ordem específica.  
  
## <a name="plan-network-topology-and-settings"></a>Planejar topologia e configurações de rede  
  
### <a name="plan-network-adapters-and-ip-addressing"></a>Planejar adaptadores de rede e endereçamento IP  
  
1.  Identificar a topologia de adaptador de rede que será usada. O Acesso Remoto pode ser configurado com um dos seguintes:  
  
    -   Com dois adaptadores de rede-tanto na borda com um adaptador de rede conectado à Internet e o outro à rede interna, ou atrás de um NAT, firewall ou roteador, com um adaptador de rede conectado à rede de perímetro e outro para o interno rede.  
  
    -   Por trás de um dispositivo NAT com um adaptador de rede-servidor de acesso remoto o é instalado atrás de um dispositivo NAT, e o único adaptador de rede está conectado à rede interna.  
  
2.  Identificar seus requisitos de endereçamento IP:  
  
    O DirectAccess usa IPv6 com IPsec para criar uma conexão segura entre os computadores cliente do DirectAccess e a rede interna corporativa. Contudo, o DirectAccess não precisa necessariamente de uma conectividade IPv6 com a Internet ou suporte IPv6 nativo em redes internas. Em vez disso, ele configura e usa automaticamente tecnologias de transição IPv6 para criar um túnel de tráfego IPv6 pela Internet IPv4 (6to4, Teredo ou IP-HTTPS) e pela Intranet somente IPv4 (NAT64 ou ISATAP). Para uma visão geral dessas tecnologias de transição, confira os seguintes recursos:  
  
    -   [Tecnologias de transição IPv6](https://technet.microsoft.com/library/bb726951.aspx)  
  
    -   [Especificação do protocolo de túnel IP-HTTPS](https://msdn.microsoft.com/library/dd358571.aspx)  
  
3.  Configure os adaptadores e endereçamento necessários conforme a tabela a seguir. Para implantações atrás de um dispositivo NAT usando um único adaptador de rede, configure seus endereços IP usando somente a coluna de 'adaptador de rede interno'.  
  
    ||Adaptador de rede externa|Adaptador de rede interna|Requisitos de roteamento|  
    |-|--------------|--------------------|------------|  
    |Intranet IPv4 e Internet IPv4|Configure o seguinte:<br /><br />-Um endereço IPv4 público estático com as máscaras de sub-rede apropriada.<br />-Um endereço IPv4 do seu firewall da Internet ou roteador do ISP (provedor) de serviços de Internet local do gateway padrão.|Configure o seguinte:<br /><br />-Um endereço de intranet IPv4 com a máscara de sub-rede apropriada.<br />-Um sufixo DNS específico da conexão do seu namespace da intranet. O Servidor DNS também deve ser configurado na interface interna.<br />-Não configure um gateway padrão em qualquer interface de intranet.|Para configurar o servidor de Acesso Remoto para acessar todas as sub-redes na rede IPv4 interna, execute este procedimento:<br /><br />1.  Liste os espaços de endereço IPv4 para todos os locais na sua intranet.<br />2.  Use os comandos **route add -p** ou **netsh interface ipv4 add route** para adicionar os espaços de endereços IPv4 como rotas estáticas na tabela de roteamento IPv4 do servidor de Acesso Remoto.|  
    |Internet IPv6 e intranet IPv6|Configure o seguinte:<br /><br />-Use a configuração de endereço configurado automaticamente fornecida pelo seu ISP.<br />– Use o **rotear impressão** comando para garantir a existência de uma rota do IPv6 padrão apontando para o roteador do ISP na tabela de roteamento IPv6.<br />-Determine se os roteadores de intranet e o ISP estão usando preferências de roteador padrão descritas no RFC 4191 e usando uma preferência padrão mais alta que os roteadores de intranet local. Se as duas situações forem verdadeiras, nenhuma outra configuração para a rota padrão será necessária. A preferência mais alta para o roteador do ISP assegura que a rota IPv6 padrão ativa do servidor de Acesso Remoto aponte para a Internet IPv6.<br /><br />Como o servidor de Acesso Remoto é um roteador IPv6, caso você tenha uma infraestrutura IPv6 nativa, a interface de Internet também poderá acessar os controladores de domínio na intranet. Nesse caso, adicione filtros de pacote ao controlador de domínio na rede de perímetro para evitar a conectividade com o endereço IPv6 da interface voltada para Internet do servidor de Acesso Remoto.|Configure o seguinte:<br /><br />– Se você não estiver usando níveis de preferência padrão, configure suas interfaces de intranet com o **netsh interface ipv6 definir InterfaceIndex ignoredefaultroutes = habilitado** comando. Esse comando garante que as rotas padrão adicionais que apontem para os roteadores da intranet não sejam acrescentadas à tabela de roteamento de IPv6. É possível obter o InterfaceIndex das interfaces da intranet na exibição do comando netsh interface show interface.|Se você possui uma intranet IPv6, execute o seguinte procedimento para configurar o servidor de Acesso Remoto para chegar a todos os locais IPv6:<br /><br />1.  Liste os espaços de endereço IPv6 para todos os locais na sua intranet.<br />2.  Use o comando **netsh interface ipv6 add route** para adicionar os espaços de endereço IPv6 como rotas estáticas na tabela de roteamento de IPv6 do servidor de Acesso Remoto.|  
    |Internet IPv6 e intranet IPv4|O servidor de Acesso Remoto encaminha o tráfego de rota padrão IPv6 para a interface do adaptador do Microsoft 6to4 para uma retransmissão 6to4 na Internet IPv4. Você pode configurar um servidor de Acesso Remoto para o endereço IPv4 da retransmissão do Microsoft 6to4 na Internet IPv4 (usado quando o IPv6 nativo não está implantado na rede corporativa) com o seguinte comando: netsh interface ipv6 6to4 set relay name=192.88.99.1 state=enabled.|||  
  
    > [!NOTE]  
    > Observe o seguinte:  
    >   
    > 1.  Se um endereço IPv4 público tiver sido atribuído a um cliente do DirectAccess, ele usará tecnologia de transição 6to4 para se conectar à intranet. Se o cliente do DirectAccess não puder se conectar ao servidor do DirectAccess com 6to4, ele usará IP-HTTPS.  
    > 2.  Os computadores cliente IPv6 nativos podem conectar-se ao servidor de Acesso Remoto pelo IPv6 nativo, sem a necessidade de tecnologia de transição.  
  
### <a name="ConfigFirewalls"></a>Planejar requisitos de firewall  
Se o servidor de Acesso Remoto estiver atrás de um firewall de borda, as seguintes exceções serão necessárias para o tráfego do Acesso Remoto quando o servidor de Acesso Remoto estiver na Internet IPv4:  
  
-   -IP de tráfego 6to4 protocolo 41 de entrada e saída.  
  
-   IP-HTTPS-porta de destino 443 do protocolo TCP (Transmission Control) e a porta de origem TCP 443 de saída.  
  
-   Se você implantar o Acesso Remoto com um único adaptador de rede e instalar o servidor de local de rede no servidor de Acesso Remoto, também deverá haver uma exceção para a porta TCP 62000.  
  
As exceções a seguir serão necessárias para o tráfego de Acesso Remoto quando o servidor de Acesso Remoto estiver na Internet IPv6:  
  
-   Protocolo IP 50  
  
-   Entrada da porta de destino UDP 500 e saída da porta de origem UDP 500.  
  
Ao usar firewalls adicionais, aplique as seguintes exceções do firewall de rede interna no tráfego de Acesso Remoto:  
  
-   ISATAP-protocolo 41 de entrada e saída  
  
-   TCP/UDP para todo o tráfego IPv4/IPv6  
  
### <a name="plan-certificate-requirements"></a>Planejar requisitos de certificado  
Os requisitos de certificado para o IPsec incluem um certificado de computador usado pelos computadores cliente do DirectAccess ao estabelecerem a conexão IPsec entre o cliente e o servidor de Acesso Remoto e um certificado de computador usado pelos servidores de Acesso Remoto para estabelecer conexões IPsec com clientes do DirectAccess. O uso desses certificados IPsec não é obrigatório para o DirectAccess no Windows Server 2012. O Assistente para Habilitar o DirectAccess configura o servidor de Acesso Remoto para agir como um proxy Kerberos para realizar a autenticação IPsec sem a necessidade de certificados.  
  
1.  **Servidor IP-HTTPS** -quando você configurar o acesso remoto, o servidor de acesso remoto é automaticamente configurado para atuar como o ouvinte da web IP-HTTPS. O site IP-HTTPS precisa de um certificado de site, e os computadores cliente também devem poder contatar o site da CRL (lista de certificados revogados) do certificado. O assistente para Habilitar DirectAccess tenta usar o certificado SSTP VPN. Se SSTP não estiver configurado, ele verificará se um certificado para IP-HTTPS está presente no repositório pessoal do computador. Se nenhum certificado estiver disponível, ele criará automaticamente um certificado autoassinado.  
  
2.  **Servidor de local de rede** -o servidor de local de rede é um site usado para detectar se os computadores cliente estão localizados na rede corporativa. O servidor de local da rede precisa de um certificado de site. Os clientes do DirectAccess devem poder contatar o site da CRL para o certificado. O Assistente para Habilitar o DirectAccess verifica se um certificado do Servidor de Local da Rede está presente no repositório pessoal do computador. Se não estiver presente, ele cria automaticamente um certificado autoassinado.  
  
Os requisitos de certificação para cada um deles estão resumidos na tabela a seguir:  
  
|Autenticação IPsec|Servidor IP-HTTPS|Servidor de local da rede|  
|------------|----------|--------------|  
|Uma CA interna é necessária para emitir certificados de computador para o servidor de acesso remoto e clientes para a autenticação IPsec quando você não usa o proxy do Kerberos para autenticação|Autoridade de certificação pública-é recomendável usar uma autoridade de certificação pública para emitir o certificado IP-HTTPS, isso garante que o ponto de distribuição da CRL esteja disponível externamente.|AC interna – você pode usar uma CA interna para emitir o servidor de local de rede certificado de site. Verifique se o ponto de distribuição de CRL está altamente disponível pela rede interna.|  
||AC interna – você pode usar uma CA interna para emitir o certificado IP-HTTPS; No entanto, certifique-se de que o ponto de distribuição da CRL esteja disponível externamente.|Autoassinados de certificado - você pode usar um certificado autoassinado para o site de servidor de local de rede; No entanto, você não pode usar um certificado autoassinado em implantações multissite.|  
||Autoassinados de certificado - você pode usar um certificado autoassinado para o servidor IP-HTTPS; No entanto, certifique-se de que o ponto de distribuição da CRL esteja disponível externamente. Um certificado autoassinado não pode ser usado em uma implantação multissite.||  
  
#### <a name="plan-certificates-for-ip-https"></a>Planejar certificados para IP-HTTPS  
O servidor de Acesso Remoto age como um ouvinte do IP-HTTPS, e você deve instalar manualmente um certificado de site HTTPS no servidor. Observações para o planejamento:  
  
-   É recomendável usar uma AC pública, para que as CRLs estejam prontamente disponíveis.  
  
-   No campo de assunto, especifique o endereço IPv4 no adaptador da Internet do servidor de Acesso Remoto ou o FQDN da URL IP-HTTPS (o endereço ConnectTo). Se o servidor de Acesso Remoto encontrar-se atrás de um dispositivo NAT, o nome ou endereço público do dispositivo NAT deverá ser especificado.  
  
-   O nome comum do certificado deverá ser corresponder ao nome do site IP-HTTPS.  
  
-   No campo Uso Avançado de Chave, use o OID (identificador de objeto) da Autenticação do Servidor.  
  
-   No campo Pontos de Distribuição da Lista de Certificados Revogados, especifique um ponto de distribuição da CRL que seja acessível pelos clientes do DirectAccess conectados à Internet.  
  
-   O certificado IP-HTTPS deve ter uma chave privada.  
  
-   O certificado IP-HTTPS deve ser importado diretamente para o repositório pessoal.  
  
-   Os certificados IP-HTTPS podem conter curingas no nome.  
  
#### <a name="plan-website-certificates-for-the-network-location-server"></a>Planejar certificados de site para o servidor de local da rede  
Ao planejar o site de servidor de local de rede, considere o seguinte:  
  
-   No campo Assunto, especifique o endereço IP da interface da intranet do servidor de local de rede ou o FQDN da URL de local de rede.  
  
-   No campo Uso Avançado de Chave, use o OID da Autenticação do Servidor.  
  
-   No campo Pontos de Distribuição da Lista de Certificados Revogados, um ponto de distribuição da CRL que seja acessível pelos clientes DirectAccess conectados à intranet. Esse ponto de distribuição da CRL não deverá ser acessível de fora da rede interna.  
  
-   Se você estiver planejando configurar posteriormente uma implantação multissite ou em cluster, o nome do certificado não deverá ser igual ao nome interno do servidor de Acesso Remoto.  
  
    > [!NOTE]  
    > Assegure-se de que os certificados IP-HTTPS e do servidor de local de rede tenham um **Nome do Requerente**. Se o certificado não tiver um **Nome do Requerente** , mas sim um **Nome Alternativo**, ele não será aceito pelo Assistente de Acesso Remoto.  
  
#### <a name="plan-dns-requirements"></a>Planejar os requisitos de DNS  
Em uma implantação do Acesso Remoto, o DNS é necessário para o seguinte:  
  
-   **Solicitações de cliente DirectAccess** - DNS é usado para resolver as solicitações dos computadores cliente DirectAccess não localizados na rede interna. Os clientes do DirectAccess tentam se conectar ao servidor de local da rede DirectAccess para determinar se encontram-se na Internet ou na rede corporativa: Se a conexão for efetuada com êxito, os clientes serão determinados como estando na intranet e o DirectAccess não será usado, e as solicitações do cliente serão resolvidas usando o servidor DNS configurado no adaptador de rede do computador cliente. Se a conexão não for bem-sucedida, presume-se que os clientes estejam na Internet. Clientes do DirectAccess usarão a NRPT (tabela de políticas de resolução de nomes) para determinar qual servidor DNS usar ao resolver solicitações de nome. Você pode especificar que os clientes devem usar o DirectAccess DNS64 para resolver nomes ou um servidor DNS interno alternativo. Ao realizar a resolução do nome, a NRPT é usada pelos clientes do DirectAccess para identificar como lidar com uma solicitação. Os clientes solicitam um FQDN ou nome de rótulo único, como https://internal. Se um nome de rótulo único for solicitado, um sufixo do DNS será agregado para criar um FQDN. Se a consulta DNS for correspondente a uma entrada na NRPT, e DNS4 ou um servidor de intranet DNS for especificado para a entrada, então, a consulta é enviada para resolução de nome usando o servidor especificado. Se houver correspondência, mas nenhum servidor DNS foi especificado, isso indicará uma exceção à regra e a resolução de nome normal é aplicada.  
  
    Observe que quando um novo sufixo for adicionado à NRPT no console de Gerenciamento do Acesso Remoto, os servidores DNS padrão do sufixo poderão ser descobertos automaticamente, clicando no botão **Detectar** . A detecção automática funciona da seguinte maneira:  
  
    1.  Se a rede corporativa for baseada em IPv4 ou IPv4 e IPv6, o endereço padrão é o endereço do DNS64 do adaptador interno no servidor de Acesso Remoto.  
  
    2.  Se a rede corporativa for baseada em IPv6, o endereço padrão será o endereço IPv6 dos servidores DNS na rede corporativa.  
  
-   **Servidores de infraestrutura**  
  
    1.  **Servidor de local de rede** clientes - DirectAccess tentam acessar o servidor de local de rede para determinar se eles estão na rede interna. Os clientes na rede interna devem poder resolver o nome do servidor de local de rede, mas não deverão resolver o nome quando se encontrarem na Internet. Para garantir que isso ocorra, o FQDN do servidor do local de rede é adicionado, por padrão, a uma regra de exceção da NRPT. Quando você configurar o Acesso Remoto, as seguintes regras também serão criadas automaticamente:  
  
        1.  Uma regra de sufixo DNS para o domínio raiz ou o nome de domínio do servidor de Acesso Remoto e os endereços IPv6 correspondentes aos servidores DNS da intranet configurados no servidor de Acesso Remoto. Por exemplo, se o servidor de Acesso Remoto for membro do domínio corp.contoso.com, é criada uma regra para o sufixo de DNS corp.contoso.com.  
  
        2.  Uma regra de isenção para o FQDN do servidor de local de rede. Por exemplo, se a URL de servidor de local de rede é https://nls.corp.contoso.com, uma regra de isenção é criada para nls.corp.contoso.com do FQDN.  
  
        **Servidor IP-HTTPS**-server o acesso remoto age como ouvinte do IP-HTTPS e usa seu certificado de servidor para autenticar clientes IP-HTTPS. O nome do IP-HTTPS deve ser resolvível pelos clientes DirectAccess que usam servidores DNS públicos.  
  
        **Verificadores de conectividade**-acesso remoto cria uma investigação web padrão que é usada pelo uso de computadores cliente DirectAccess para verificar a conectividade à rede interna. Para garantir que a sonda funcione conforme esperado, os nomes a seguir devem ser registrados manualmente no DNS:  
  
        1.  DirectAccess-webprobehost deve resolver para o endereço IPv4 interno do servidor de acesso remoto, ou o endereço IPv6 em um ambiente somente IPv6.  
  
        2.  DirectAccess-corpconnectivityhost deve resolver para endereço localhost (loopback). O registro A e AAAA deve ser criado. Registro A com um valor 127.0.0.1 e registro AAAA com valor construído a partir do prefixo NAT64 com os últimos 32 bits como 127.0.0.1. O prefixo NAT64 pode ser recuperado executando o cmdlet get-netnattransitionconfiguration.  
  
            > [!NOTE]  
            > Isso é válido somente em um ambiente com IPv4 apenas. Em um ambiente IPv4 com IPv6, ou somente com IPv6, somente o registro AAAA deve ser criado com o endereço IP do loopback ::1.  
  
        Você pode criar verificadores de conectividade adicional usando outros endereços da Web via HTTP ou PING. Uma entrada de DNS deverá existir para cada verificador de conectividade.  
  
#### <a name="dns-server-requirements"></a>Requisitos de servidor DNS  
  
-   Para clientes DirectAccess, você deve usar um servidor DNS executando o Windows Server 2003, Windows Server 2008, Windows Server 2008 R2, Windows Server 2012 ou qualquer servidor DNS que dá suporte a IPv6.  
  
### <a name="plan-active-directory"></a>Planejar o Active Directory  
Acesso remoto usa o Active Directory e objetos de diretiva de grupo do Active Directory da seguinte maneira:  
  
-   **Autenticação** -Active Directory é usado para autenticação. O túnel de intranet usa autenticação Kerberos para o usuário acessar recursos internos.  
  
-   **Objetos de diretiva de grupo** -acesso remoto reúne as definições de configuração nos objetos de diretiva de grupo que são aplicados a servidores de acesso remoto, clientes e servidores de aplicativos internos.  
  
-   **Grupos de segurança** -acesso remoto usa grupos de segurança para reunir e identificar computadores cliente DirectAccess e servidores de acesso remoto. As políticas de grupo são aplicadas ao grupo de segurança necessário.  
  
-   **Políticas IPsec estendidas** -acesso remoto pode usar a autenticação IPsec e criptografia entre clientes e o servidor de acesso remoto. Você pode estender a autenticação e criptografia IPsec para os servidores de aplicativo internos especificados.   
  
**Requisitos do Active Directory**  
  
Ao planejar o Active Directory para uma implantação de Acesso Remoto, é necessário ter o seguinte:  
  
-   Pelo menos um controlador de domínio instalado nos sistemas operacionais Windows Server 2012, Windows Server 2008 R2 Windows Server 2008 ou Windows Server 2003.  
  
    Se o controlador de domínio estiver em uma rede de perímetro (estando, portanto, acessível do adaptador de rede voltado para a Internet do servidor de Acesso Remoto), evite que o servidor de Acesso Remoto acesse-o adicionando filtros de pacote no controlador de domínio, impedindo a conectividade com o endereço IP do adaptador de Internet.  
  
-   O servidor de Acesso Remoto deve ser um membro do domínio.  
  
-   Os clientes do DirectAccess devem ser membros do domínio. Os clientes podem pertencer a:  
  
    -   Qualquer domínio na mesma floresta que o servidor de Acesso Remoto.  
  
    -   Qualquer domínio que tenha uma relação de confiança bidirecional com o domínio do servidor de Acesso Remoto.  
  
    -   Qualquer domínio em uma floresta que tenha uma relação de confiança bidirecional com a floresta à qual o domínio do Acesso Remoto pertence.  
  
> [!NOTE]  
> -   O servidor de Acesso Remoto não deve ser um controlador de domínio.  
> -   O controlador de domínio Active Directory usado para o Acesso Remoto não pode estar acessível do adaptador externo da Internet do servidor de Acesso Remoto (o adaptador não deve estar no perfil de domínio do Firewall do Windows).  
  
### <a name="plan-group-policy-objects"></a>Planejar objetos de política de grupo  
As definições do DirectAccess configuradas ao definir o Acesso Remoto são coletadas no GPO (Objeto de Política de Grupo). Três GPOs diferentes são populados com definições do DirectAccess e distribuídos como segue:  
  
-   **GPO do cliente DirectAccess** -este GPO contém configurações do cliente, incluindo configurações de tecnologia de transição IPv6, entradas da NRPT e Firewall do Windows com regras de segurança de conexão de segurança avançada. O GPO é aplicado a grupos de segurança especificados para os computadores cliente.  
  
-   **GPO do servidor DirectAccess** -este GPO contém configurações do DirectAccess aplicadas a qualquer servidor configurado como um servidor de acesso remoto em sua implantação. Ele contém também as regras de segurança de conexão do Firewall do Windows com Segurança Avançada.  
  
-   **GPO de servidores de aplicativo** -este GPO contém configurações para servidores de aplicativo selecionado ao qual você estende opcionalmente a autenticação e criptografia dos clientes do DirectAccess. Se a autenticação e a criptografia não forem estendidas, esse GPO não será usado.  
  
Os GPOs são criados automaticamente pelo Assistente para Habilitar o DirectAccess e um nome padrão é especificado para cada GPO.  
  
> [!CAUTION]  
> Use o procedimento a seguir para fazer backup de todos os Objetos da Política de Grupo do Acesso Remoto antes de executar cmdlets do DirectAccess: [Fazer backup e restaurar a configuração do acesso remoto](https://go.microsoft.com/fwlink/?LinkID=257928)  
  
Os GPOs podem ser configurados de duas maneiras:  
  
1.  **Automaticamente** -você pode especificar que eles são criados automaticamente. Um nome padrão é especificado para cada GPO.  
  
2.  **Manualmente** -você pode usar GPOs predefinidos pelo administrador do Active Directory.  
  
Observe que depois de o DirectAccess ser configurado para usar GPOs específicos, ele não poderá ser configurado para usar GPOs diferentes.  
  
#### <a name="automatically-created-gpos"></a>GPOs criados automaticamente  
Observe o seguinte quando usar GPOs criados automaticamente:  
  
GPOs criados automaticamente são aplicados de acordo com o local e o parâmetro de destino do link, da seguinte maneira:  
  
-   Para GPO do servidor do DirectAccess, os parâmetros local e de link apontam para o domínio que contém o servidor de Acesso Remoto.  
  
-   Quando os GPOs de cliente são criados, o local é definido para um único domínio no qual o GPO será criado. O nome do GPO é pesquisado em cada domínio e é preenchido nas configurações do DirectAccess, se existir. O destino do link é definido para a raiz do domínio no qual o GPO foi criado. Um GPO é criado para cada domínio que contém computadores cliente ou servidores de aplicativo, e o GPO é vinculado à raiz dos seus respectivos domínios.  
  
Ao usar GPOs criados automaticamente, para aplicar as configurações do DirectAccess, o administrador do servidor de Acesso Remoto precisará das seguintes permissões:  
  
-   Permissões de criação de GPO para cada domínio.  
  
-   Permissões de links para todas as raízes de domínio de cliente selecionadas.  
  
-   Permissões de links para as raízes de domínio de GPO do servidor.  
  
-   As permissões de segurança para criar, editar, excluir e modificar são necessárias para os GPOs.  
  
-   É recomendado que o administrador do Acesso Remoto possua permissões de leitura do GPO para cada domínio necessário. Isso permite que o Acesso Remoto confirme que não existam GPOs com nomes duplicados ao criá-los.  
  
Observe que se as permissões corretas às quais vincular GPOs não existirem, um aviso será emitido. A operação de Acesso Remoto continuará, porém não ocorrerá a vinculação. Se o aviso for emitido, não será possível criar links automaticamente, mesmo depois que as permissões forem aplicadas. Em vez disso, o administrador precisará criar os links manualmente.  
  
#### <a name="manually-created-gpos"></a>GPOs criados manualmente  
Observe o seguinte quando usar GPOs criados manualmente:  
  
-   Os GPOs devem existir antes de o Assistente de Instalação de Acesso Remoto ser executado.  
  
-   Quando usar GPOs criados manualmente, para aplicar as configurações do DirectAccess, o administrador do Acesso Remoto precisa ter permissões integrais do GPO (Editar, Excluir, Modificar segurança) nos GPOs criados manualmente.  
  
-   Ao usar GPOs criados manualmente, uma pesquisa é feita para um link para o GPO no domínio inteiro. Se o GPO não estiver vinculado ao domínio, um link é criado automaticamente na raiz do domínio. Se as permissões necessárias para criar o link não estiverem disponíveis, será emitido um aviso.  
  
Observe que se as permissões corretas às quais vincular GPOs não existirem, um aviso será emitido. A operação de Acesso Remoto continuará, porém não ocorrerá a vinculação. Se o aviso for emitido, não será possível criar links automaticamente, mesmo se as permissões forem adicionadas posteriormente. Em vez disso, o administrador precisará criar os links manualmente.  
  
#### <a name="recovering-from-a-deleted-gpo"></a>Recuperando um GPO excluído  
Se um GPO de servidor de Acesso Remoto, cliente ou de servidor de aplicativos for excluído por acidente e não houver backup disponível, você deverá remover as definições da configuração e configurá-las novamente. Se um backup estiver disponível, você poderá usá-lo para restaurar o GPO.  
  
O **Gerenciamento do Acesso Remoto** exibirá a seguinte mensagem de erro: **GPO <GPO name> não é possível encontrar**. Para remover as definições de configuração, siga essas etapas:  
  
1.  Execute o cmdlet PowerShell **Uninstall-remoteaccess**.  
  
2.  Abra novamente o **Gerenciamento de Acesso Remoto**.  
  
3.  Você verá uma mensagem de erro indicando que o GPO não foi encontrado. Clique em **Remover definições de configuração**. Depois de concluído, o servidor será restaurado a um estado não configurado.  
