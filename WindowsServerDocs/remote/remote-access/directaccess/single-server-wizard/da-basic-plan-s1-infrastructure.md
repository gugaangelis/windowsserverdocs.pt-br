---
title: Etapa 1 de plano a infraestrutura básica do DirectAccess
description: Este tópico faz parte do guia de implantar um único servidor DirectAccess usando o Introdução ao Assistente para Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b86726fd6e9ee37dbfd43357d8040b43b8dfb200
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59853337"
---
#<a name="step-1-plan-the-basic-directaccess-infrastructure"></a>Etapa 1 de plano a infraestrutura básica do DirectAccess
A primeira etapa para uma implantação básica do DirectAccess em um único servidor é executar o planejamento da infraestrutura necessária para a implantação. Este tópico descreve as etapas de planejamento de infraestrutura:  
  
|Tarefa|Descrição|  
|----|--------|  
|Planejar topologia e configurações de rede|Decidir onde colocar o servidor do DirectAccess \(na borda ou atrás de uma conversão de endereços de rede \(NAT\) firewall ou dispositivo\)e planejar o endereçamento IP e roteamento.|  
|Planejar requisitos de firewall|Planejar como permitir o DirectAccess por meio de firewalls de borda.|  
|Planejar requisitos de certificado|O DirectAccess pode usar Kerberos ou certificados para autenticação cliente. Nessa implantação básica do DirectAccess, um Proxy Kerberos é configurado automaticamente e a autenticação é obtida usando credenciais do Active Directory.|  
|Planejar os requisitos de DNS|Planeje as configurações DNS para o servidor DirectAccess, servidores de infraestrutura e conectividade do cliente.|  
|Planejar o Active Directory|Planeje seus controladores de domínio e os requisitos do Active Directory.|  
|Planejar objetos de política de grupo|Decida quais GPOs são necessários em sua organização e como criar ou editar os GPOs.|  
  
As tarefas de planejamento não precisam ser feitas em uma ordem específica.  
  
## <a name="bkmk_1_1_Network_svr_top_settings"></a>Planejar a topologia de rede e configurações  
  
### <a name="plan-network-adapters-and-ip-addressing"></a>Planejar adaptadores de rede e endereçamento IP  
  
1.  Identificar a topologia de adaptador de rede que será usada. O DirectAccess pode ser configurado com um dos seguintes:  
  
    -   Com dois adaptadores de rede - o na borda com um adaptador de rede conectado à Internet e o outro à rede interna, ou atrás de um NAT, firewall ou roteador, com um adaptador de rede conectado à rede de perímetro e outro para o interno rede.  
  
    -   Por trás de um dispositivo NAT com um adaptador de rede - o servidor DirectAccess é instalado atrás de um dispositivo NAT e o único adaptador de rede está conectado à rede interna.  
  
2.  Identificar seus requisitos de endereçamento IP:  
  
    O DirectAccess usa IPv6 com IPsec para criar uma conexão segura entre os computadores cliente do DirectAccess e a rede interna corporativa. Contudo, o DirectAccess não precisa necessariamente de uma conectividade IPv6 com a Internet ou suporte IPv6 nativo em redes internas. Em vez disso, ele automaticamente configura e usa tecnologias de transição IPv6 para encapsular tráfego IPv6 pela Internet IPv4 \(6to4, Teredo, IP\-HTTPS\) e entre sua IPv4\-somente intranet \( NAT64 ou ISATAP\). Para uma visão geral dessas tecnologias de transição, confira os seguintes recursos:  
  
    -   [Tecnologias de transição IPv6](https://technet.microsoft.com/library/bb726951.aspx)  
  
    -   [Especificação do protocolo de túnel IP-HTTPS](https://msdn.microsoft.com/library/dd358571(PROT.10).aspx)  
  
3.  Configure os adaptadores e endereçamento necessários conforme a tabela a seguir. Para implantações atrás de um dispositivo NAT usando um único adaptador de rede, configure seus endereços IP usando somente o **adaptador de rede interno** coluna.  
  
    ||Adaptador de rede externa|Adaptador de rede interna<sup>1</sup>|Requisitos de roteamento|  
    |-|--------------|--------------------|------------|  
    |Intranet IPv4 e Internet IPv4|Configure o seguinte:<br /><br />-Um endereço IPv4 público estático com a máscara de sub-rede apropriada.<br />-Um gateway padrão endereço IPv4 do seu firewall da Internet ou o provedor de serviços de Internet local \(ISP\) roteador.|Configure o seguinte:<br /><br />-Um endereço de intranet IPv4 com a máscara de sub-rede apropriada.<br />-Uma conexão\-sufixo DNS específico do seu namespace da intranet. Um servidor DNS também deve ser configurado na interface interna.<br />-Não configure um gateway padrão em qualquer interface de intranet.|Para configurar o servidor do DirectAccess para acessar todas as sub-redes na rede IPv4 interna, execute este procedimento:<br /><br />1.  Liste os espaços de endereço IPv4 para todos os locais na sua intranet.<br />2.  Use o **Adicionar rota \-p** ou **netsh interface ipv4 Adicionar rota** espaços de endereço de comandos para adicionar o IPv4 como rotas estáticas na tabela de roteamento IPv4 do servidor DirectAccess.|  
    |Internet IPv6 e intranet IPv6|Configure o seguinte:<br /><br />-Use a configuração de endereço configurado automaticamente fornecida pelo seu ISP.<br />– Use o **rotear impressão** comando para garantir a existência de uma rota do IPv6 padrão apontando para o roteador do ISP na tabela de roteamento IPv6.<br />-Determine se os roteadores de intranet e o ISP estão usando preferências de roteador padrão descritas no RFC 4191 e usando uma preferência padrão mais alta que os roteadores de intranet local. Se as duas situações forem verdadeiras, nenhuma outra configuração para a rota padrão será necessária. A preferência mais alta para o roteador do ISP assegura que a rota IPv6 padrão ativa do servidor DirectAccess aponte para a Internet IPv6.<br /><br />Como o servidor do DirectAccess é um roteador IPv6, caso você tenha uma infraestrutura IPv6 nativa, a interface de Internet também poderá acessar os controladores de domínio na intranet. Nesse caso, adicione filtros de pacote ao controlador de domínio na rede de perímetro que impedem a conectividade com o endereço IPv6 da Internet\-voltado para a interface do servidor DirectAccess.|Configure o seguinte:<br /><br />– Se você não estiver usando níveis de preferência padrão, configure suas interfaces de intranet com o **netsh interface ipv6 definir InterfaceIndex ignoredefaultroutes\=habilitado** comando. Esse comando garante que as rotas padrão adicionais que apontem para os roteadores da intranet não sejam acrescentadas à tabela de roteamento de IPv6. É possível obter o InterfaceIndex das interfaces da intranet na exibição do comando netsh interface show interface.|Se você possui uma intranet IPv6, execute o seguinte procedimento para configurar o servidor do DirectAccess para chegar a todos os locais IPv6:<br /><br />1.  Liste os espaços de endereço IPv6 para todos os locais na sua intranet.<br />2.  Use o comando **netsh interface ipv6 add route** para adicionar os espaços de endereço IPv6 como rotas estáticas na tabela de roteamento de IPv6 do servidor do DirectAccess.|  
    |Internet IPv4 e intranet IPv6|O servidor do DirectAccess encaminha o tráfego de rota padrão IPv6 para a interface do adaptador Microsoft 6to4 para uma retransmissão 6to4 na Internet IPv4. Você pode configurar um servidor DirectAccess para o endereço IPv4 da retransmissão do Microsoft 6to4 na Internet IPv4 \(usado quando o IPv6 nativo não está implantado na rede corporativa\) com o seguinte comando: netsh interface ipv6 6to4 set relay nome da\=Name=192.88.99.1 estado\=habilitado o comando.|||  
  
    > [!NOTE]  
    > Observe o seguinte:  
    >   
    > 1.  Se um endereço IPv4 público tiver sido atribuído a um cliente do DirectAccess, ele usará tecnologia de transição 6to4 para se conectar à intranet. Se o cliente DirectAccess não pode se conectar ao servidor DirectAccess com 6to4, ele usará IP\-HTTPS.  
    > 2.  Os computadores cliente IPv6 nativos podem se conectar ao servidor do DirectAccess pelo IPv6 nativo, sem a necessidade de tecnologia de transição.  
  
### <a name="ConfigFirewalls"></a>Planejar requisitos de firewall  
Se o servidor DirectAccess estiver atrás de um firewall de borda, as seguintes exceções serão necessárias para o tráfego DirectAccess quando o servidor DirectAccess estiver na Internet IPv4:  
  
-   tráfego 6to4 - protocolo IP 41 de entrada e saída.  
  
-   IP\-HTTPS Transmission Control Protocol \(TCP\) a porta 443 do destino e porta de origem TCP 443 de saída.  
  
-   Se você implantar o DirectAccess com um único adaptador de rede e instalar o servidor de local de rede no servidor do DirectAccess, também deverá haver uma exceção para a porta TCP 62000.  
  
    > [!NOTE]  
    > Essa exceção está no servidor DirectAccess. Todas as outras exceções estão no firewall de borda.  
  
As exceções a seguir serão necessárias para o tráfego DirectAccess quando o servidor DirectAccess estiver na Internet IPv6:  
  
-   Protocolo IP 50  
  
-   Entrada da porta de destino UDP 500 e saída da porta de origem UDP 500.  
  
Ao usar firewalls adicionais, aplique as seguintes exceções do firewall de rede interna no tráfego de DirectAcess:  
  
-   ISATAP - protocolo 41 de entrada e saída  
  
-   TCP\/UDP para todos os IPv4\/tráfego IPv6  
  
### <a name="bkmk_1_2_CAs_and_certs"></a>Planejar requisitos de certificado  
Os requisitos de certificado para o IPsec incluem um certificado de computador usado pelos computadores cliente do DirectAccess ao estabelecerem a conexão IPsec entre o cliente e o servidor do DirectAccess e um certificado de computador usado pelos servidores do DirectAccess para estabelecer conexões IPsec com clientes do DirectAccess. Para o DirectAccess no Windows Server 2012 R2 e Windows Server 2012, o uso desses certificados IPsec não é obrigatório. O Assistente de Introdução configura o servidor do DirectAccess para agir como um proxy Kerberos para realizar a autenticação IPsec sem a necessidade de certificados.
  
1.  **IP\-servidor HTTPS**. Quando você configura o DirectAccess, o servidor DirectAccess é automaticamente configurado para atuar como o IP\-Ouvinte HTTPS da web. O IP\-site HTTPS requer um certificado de site e computadores cliente devem ser capazes de entrar em contato com a lista de certificados revogados \(CRL\) site para o certificado. O assistente para Habilitar DirectAccess tenta usar o certificado SSTP VPN. Se SSTP não estiver configurado, ele verifica se um certificado para IP\-HTTPS está presente no repositório pessoal do computador. Se nenhum estiver disponível, ele cria automaticamente um self\-certificado autoassinado.
  
2.  **Servidor de local de rede**. O servidor de local de rede é um site usado para detectar se os computadores cliente estão localizados na rede corporativa. O servidor de local de rede requer um certificado de site da web. Clientes do DirectAccess devem poder contatar o site da CRL para o certificado. O assistente habilitar o acesso remoto verifica se um certificado de servidor de local de rede está presente no repositório pessoal do computador. Se não estiver presente, ele cria automaticamente um self\-certificado autoassinado.
  
Os requisitos de certificação para cada um deles estão resumidos na tabela a seguir:  
  
|Autenticação IPsec|IP\-servidor HTTPS|Servidor de local da rede|  
|------------|----------|--------------|  
|Uma CA interna é necessária para emitir certificados de computador para o servidor do DirectAccess e clientes para a autenticação IPsec quando você não usa o proxy do Kerberos para autenticação|Autoridade de certificação pública - é recomendável usar uma autoridade de certificação pública para emitir o IP\-certificado HTTPS, isso garante que o ponto de distribuição da CRL esteja disponível externamente.|AC interna – você pode usar uma CA interna para emitir o certificado de site do servidor de local de rede. Verifique se o ponto de distribuição de CRL está altamente disponível pela rede interna.|  
||AC interna – você pode usar uma CA interna para emitir o IP\-certificado HTTPS; no entanto, certifique-se de que o ponto de distribuição da CRL esteja disponível externamente.|Self\-assinou o certificado - você pode usar um self\-certificado autoassinado para o site de servidor de local de rede; no entanto, você não pode usar um self\-assinou o certificado em implantações multissite.|  
||Self\-assinou o certificado - você pode usar um self\-certificado autoassinado para o IP\-servidor HTTPS; no entanto, certifique-se de que o ponto de distribuição da CRL esteja disponível externamente. Um self\-certificado autoassinado não pode ser usado em uma implantação multissite.||  
  
#### <a name="bkmk_website_cert_IPHTTPS"></a>Planejar certificados para IP\-servidor de local de rede e HTTPS  
Se desejar provisionar um certificado para esses fins, consulte [Implantar um único servidor DirectAccess com configurações avançadas](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md). Se nenhum certificado estiver disponível, o Assistente de Introdução criará automaticamente self\-certificados autoassinados para esses fins.
  
> [!NOTE]
> Se você provisionar certificados para IP\-HTTPS e o servidor de local de rede manualmente, certifique-se de que os certificados tenham um nome de assunto. Se o certificado não tiver um nome de assunto, mas tiver um nome alternativo, ele não será aceito pelo assistente DirectAccess.  
  
#### <a name="plan-dns-requirements"></a>Planejar os requisitos de DNS  
Em uma implantação do DirectAccess, é necessário DNS para o seguinte:  
  
-   **Solicitações de cliente do DirectAccess**. O DNS é usado para resolver solicitações de computadores cliente do DirectAccess não localizados na rede interna. Os clientes do DirectAccess tentam se conectar ao servidor de local da rede DirectAccess para determinar se encontram-se na Internet ou na rede corporativa: Se a conexão for efetuada com êxito, os clientes serão determinados como estando na intranet e o DirectAccess não será usado, e as solicitações do cliente serão resolvidas usando o servidor DNS configurado no adaptador de rede do computador cliente. Se a conexão não for bem-sucedida, presume-se que os clientes estejam na Internet. Os clientes DirectAccess usará a tabela de diretivas de resolução de nome \(NRPT\) para determinar qual servidor DNS a ser usado ao resolver solicitações de nome. Você pode especificar que os clientes devem usar o DirectAccess DNS64 para resolver nomes ou um servidor DNS interno alternativo. Ao realizar a resolução do nome, a NRPT é usada pelos clientes do DirectAccess para identificar como lidar com uma solicitação. Os clientes solicitam um FQDN ou single\-nome de rótulo como http:\/\/interno. Se um único\-solicitações do nome do rótulo é, um sufixo DNS é acrescentado para criar um FQDN. Se a consulta DNS for correspondente a uma entrada na NRPT, e DNS4 ou um servidor de intranet DNS for especificado para a entrada, então, a consulta é enviada para resolução de nome usando o servidor especificado. Se houver correspondência, mas nenhum servidor DNS foi especificado, isso indicará uma exceção à regra e a resolução de nome normal é aplicada.  
  
    Observe que quando um novo sufixo é adicionado à NRPT no console de Gerenciamento do DirectAccess, os servidores DNS padrão do sufixo podem ser descobertos automaticamente, clicando em **Detectar**. A detecção automática funciona da seguinte maneira:  
  
    1.  Se a rede corporativa for IPv4\-baseado, ou IPv4 e IPv6, o endereço padrão é o endereço do DNS64 do adaptador interno no servidor DirectAccess.  
  
    2.  Se a rede corporativa for IPv6\-com base, o endereço padrão é o endereço IPv6 dos servidores DNS na rede corporativa.  
  
-   **Servidores de infraestrutura**
  
    1.  **Servidor de local de rede**. Os clientes DirectAccess tentam acessar o servidor de local de rede para determinar se estão em uma rede interna. Os clientes na rede interna devem poder resolver o nome do servidor de local de rede, mas não deverão resolver o nome quando se encontrarem na Internet. Para garantir que isso ocorra, o FQDN do servidor do local de rede é adicionado, por padrão, a uma regra de exceção da NRPT. Ao configurar o DirectAccess, as seguintes regras também serão criadas automaticamente:  
  
        1.  Uma regra de sufixo DNS para o nome de domínio do servidor DirectAccess e os endereços IPv6 correspondentes aos servidores DNS da intranet configurados no servidor DirectAccess. Por exemplo, se o servidor do DirectAccess for membro do domínio corp.contoso.com, é criada uma regra para o sufixo de DNS corp.contoso.com.  
  
        2.  Uma regra de isenção para o FQDN do servidor de local de rede. Por exemplo, se a URL de servidor de local de rede é https:\/\/nls.corp.contoso.com, uma regra de isenção é criada para nls.corp.contoso.com do FQDN.  
  
        **IP\-servidor HTTPS**. O servidor do DirectAccess age como um IP\-Ouvinte HTTPS e usa seu servidor de certificado para autenticar para o IP\-clientes HTTPS. O IP\-nome HTTPS deve ser resolvível pelos clientes DirectAccess que usam servidores DNS públicos.  
  
        **Verificadores de conectividade**. O DirectAccess cria uma investigação Web padrão usada pelos computadores cliente do DirectAccess para verificar a conectividade com a rede interna. Para garantir que a sonda funcione conforme esperado, os nomes a seguir devem ser registrados manualmente no DNS:  
  
        1.  DirectAccess\-webprobehost - deve resolver para o endereço IPv4 interno do servidor DirectAccess, ou o endereço IPv6 em um IPv6\-apenas ambiente.  
  
        2.  DirectAccess\-corpconnectivityhost - deve resolver para localhost \(loopback\) endereço. O registro A e AAAA deve ser criado. Registro A com um valor 127.0.0.1 e registro AAAA com valor construído a partir do prefixo NAT64 com os últimos 32 bits como 127.0.0.1. O prefixo NAT64 pode ser recuperado executando o cmdlet get\-netnattransitionconfiguration.  
  
        Você pode criar verificadores de conectividade adicional usando outros endereços da Web via HTTP ou PING. Uma entrada de DNS deverá existir para cada verificador de conectividade.  
  
#### <a name="dns-server-requirements"></a>Requisitos de servidor DNS  
  
-   Para clientes DirectAccess, você deve usar um servidor DNS que está executando o Windows Server 2008, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016 ou qualquer servidor DNS que dá suporte a IPv6.  
  
> [!NOTE]  
> Não é recomendável que você use servidores DNS que estejam executando o Windows Server 2003, quando estiver implantando o DirectAccess. Embora os servidores DNS do Windows Server 2003 ofereçam suporte a registros IPv6, o Windows Server 2003 não tem mais o suporte da Microsoft. Além disso, você não deve implantar o DirectAccess se os controladores de domínio estiverem executando o Windows Server 2003 devido a um problema com o Serviço de replicação de arquivos. Para obter mais informações, consulte [configurações sem suporte do DirectAccess](../DirectAccess-Unsupported-Configurations.md).  
  
### <a name="bkmk_1_4_NLS"></a>Planejar o servidor de local de rede  
O servidor de local da rede é um site da Web usado para detectar se os clientes DirectAccess se encontram na rede corporativa. Os clientes na rede corporativa não usam o DirectAccess para acessar recursos internos, mas, em vez disso, se conectam diretamente.  
  
O Assistente de Introdução define automaticamente o servidor de local de rede no servidor DirectAccess e o site da Web é criado automaticamente quando você implanta o DirectAccess. Isso permite uma instalação simples sem o uso de uma infraestrutura de certificado.
  
Se você deseja implantar um servidor de local de rede e não usar self\-certificados autoassinados, consulte [implantar um único servidor DirectAccess com configurações avançadas](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).
  
### <a name="bkmk_1_6_AD"></a>Planejar o Active Directory  
O DirectAccess usa o Active Directory e objetos de diretiva de grupo do Active Directory da seguinte maneira:
  
-   **Autenticação**. O Active Directory é usado para autenticação. O túnel do DirectAccess usa autenticação Kerberos para o usuário acessar recursos internos.
  
-   **Objetos de política de grupo**. O DirectAccess reúne as definições de configuração nos objetos da política de grupo que são aplicados aos servidores e clientes do DirectAccess.
  
-   **Grupos de segurança**. O DirectAccess usa grupos de segurança para reunir e identificar computadores cliente do DirectAccess e servidores do DirectAccess. As políticas de grupo são aplicadas ao grupo de segurança necessário.

**Requisitos do Active Directory**  
  
Ao planejar o Active Directory para uma implantação do DirectAccess, os procedimentos a seguir serão necessários:  
  
-   Pelo menos um controlador de domínio instalado no Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008. 
  
    Se o controlador de domínio estiver em uma rede de perímetro \(e, portanto, acessível da Internet\-voltados para o adaptador de rede do servidor DirectAccess\) impedir que o servidor do DirectAccess acesse-o adicionando filtros de pacote no controlador de domínio, para evitar a conectividade com o endereço IP do adaptador da Internet.  
  
-   O servidor do DirectAccess deve ser um membro do domínio.  
  
-   Os clientes do DirectAccess devem ser membros do domínio. Os clientes podem pertencer a:  
  
    -   Qualquer domínio na mesma floresta que o servidor do DirectAccess.  
  
    -   Qualquer domínio que tenha uma\-relação de confiança bidirecional com o domínio do servidor DirectAccess.  
  
    -   Qualquer domínio em uma floresta que tenha uma\-relação de confiança bidirecional com a floresta à qual pertence o domínio do DirectAccess.  
  
> [!NOTE]
> - O servidor DirectAccess não pode ser um controlador de domínio.  
> - O controlador de domínio do Active Directory usado para o DirectAccess não pode estar acessível do adaptador externo da Internet do servidor do DirectAccess \(o adaptador não deve estar no perfil de domínio do Windows Firewall\).  
  
### <a name="bkmk_1_7_GPOs"></a>Objetos de diretiva de grupo do plano  
Configurações do DirectAccess configuradas quando você configura o DirectAccess são coletadas nos objetos de diretiva de grupo \(GPO\). Dois GPOs diferentes são preenchidos com definições do DirectAccess e distribuídos como segue:  
  
-   **GPO de cliente do DirectAccess**. Este GPO contém configurações do cliente, inclusive configurações da tecnologia de transição IPv6, entradas da NRPT e regras de segurança de conexão do Firewall do Windows com Segurança Avançada. O GPO é aplicado a grupos de segurança especificados para os computadores cliente.  
  
-   **GPO de servidor do DirectAccess**. Este GPO contém definições de configurações do DirectAccess aplicadas a qualquer servidor configurado como servidor do DirectAccess na implantação. Ele contém também as regras de segurança de conexão do Firewall do Windows com Segurança Avançada.  
  
Os GPOs podem ser configurados de duas maneiras:  
  
1.  **Automaticamente**. Você pode especificar para que sejam criados automaticamente. Um nome padrão é especificado para cada GPO. Os GPOs são criados automaticamente pelo Assistente de Introdução.
  
2.  **Manualmente**. Você pode usar GPOs predefinidos pelo administrador do Active Directory.
  
Observe que depois de o DirectAccess ser configurado para usar GPOs específicos, ele não poderá ser configurado para usar GPOs diferentes.
  
> [!IMPORTANT]
> Seja em GPOs configurados automática ou manualmente, será necessário adicionar uma política para detecção de links lentos se os clientes forem usar 3G. O caminho de diretiva de grupo para **política: Configurar a detecção de vínculo lento de diretiva de grupo** é: **Configuração do computador \/ políticas \/ modelos administrativos \/ sistema \/ diretiva de grupo**.  
  
> [!CAUTION]  
> Use o procedimento a seguir para fazer backup de todos os Objetos da política de grupo do DirectAccess antes de executar os cmdlets do DirectAccess: [Fazer backup e restaurar a configuração do DirectAccess](https://go.microsoft.com/fwlink/?LinkID=257928)  
  
#### <a name="automatically-created-gpos"></a>Automaticamente\-GPOs criados  
Observe o seguinte ao usar automaticamente\-GPOs criados:  
  
GPOs criados automaticamente são aplicados de acordo com o local e o parâmetro de destino do link, da seguinte maneira:  
  
-   Para GPO do servidor do DirectAccess, os parâmetros local e de link apontam para o domínio que contém o servidor DirectAccess.  
  
-   Quando os GPOs de cliente são criados, o local é definido para um único domínio no qual o GPO será criado. O nome do GPO é pesquisado em cada domínio e é preenchido nas configurações do DirectAccess, se existir. O destino do link é definido para a raiz do domínio no qual o GPO foi criado. Um GPO é criado para cada domínio que contém computadores cliente, e o GPO é vinculado à raiz dos seus respectivos domínios.  
  
Ao usar GPOs criados automaticamente, para aplicar as configurações do DirectAccess, o administrador do servidor do DirectAccess precisará das seguintes permissões:  
  
-   Permissões de criação de GPO para cada domínio.  
  
-   Permissões de links para todas as raízes de domínio de cliente selecionadas.  
  
-   Permissões de links para as raízes de domínio de GPO do servidor.  
  
-   As permissões de segurança para criar, editar, excluir e modificar são necessárias para os GPOs.  
  
-   É recomendado que o administrador do DirectAccess possua permissões de leitura do GPO para cada domínio necessário. Isso permite que o DirectAccess confirme que não existem GPOs com nomes duplicados ao criá-los.  
  
Observe que se as permissões corretas às quais vincular GPOs não existirem, um aviso será emitido. A operação de DirectAccess continuará, porém não ocorrerá a vinculação. Se o aviso for emitido, não será possível criar links automaticamente, mesmo depois que as permissões forem adicionadas posteriormente. Em vez disso, o administrador precisará criar os links manualmente.  
  
#### <a name="manually-created-gpos"></a>Manualmente\-GPOs criados  
Observe o seguinte ao usar manualmente\-GPOs criados:  
  
-   Os GPOs devem existir antes de o Assistente de Introdução de Acesso Remoto ser executado.  
  
-   Ao usar manualmente\-GPOs criados, para aplicar as configurações do DirectAccess do DirectAccess administrador requer permissões completas de GPO \(editar, excluir, modificar segurança\) sobre o manualmente\-GPOs criados.  
  
-   Ao usar GPOs criados manualmente, uma pesquisa é feita para um link para o GPO no domínio inteiro. Se o GPO não estiver vinculado ao domínio, um link é criado automaticamente na raiz do domínio. Se as permissões necessárias para criar o link não estiverem disponíveis, será emitido um aviso.  
  
Observe que se as permissões corretas às quais vincular GPOs não existirem, um aviso será emitido. A operação de DirectAccess continuará, porém não ocorrerá a vinculação. Se o aviso for emitido, não será possível criar links automaticamente, mesmo se as permissões forem adicionadas posteriormente. Em vez disso, o administrador precisará criar os links manualmente.  
  
#### <a name="recovering-from-a-deleted-gpo"></a>Recuperando um GPO excluído  
Se um servidor do DirectAccess, cliente ou GPO do servidor de aplicativo foi excluído por acidente e não houver nenhum backup disponível, você deve remover as definições de configuração e o re\-configurar novamente. Se um backup estiver disponível, você poderá usá-lo para restaurar o GPO.  
  
**Gerenciamento do DirectAccess** exibirá a seguinte mensagem de erro: **GPO <GPO name> não é possível encontrar**. Para remover as definições de configuração, siga essas etapas:  
  
1.  Execute o cmdlet do PowerShell **Uninstall\-remoteaccess**.  
  
2.  Re\-abra **gerenciamento do DirectAccess**.  
  
3.  Você verá uma mensagem de erro indicando que o GPO não foi encontrado. Clique em **Remover definições de configuração**. Após a conclusão, o servidor será restaurado para um Cancelar\-configurado de estado.  
  
### <a name="BKMK_Links"></a>Próxima etapa  
  
-   [Etapa 2: Planejar a implantação básica do DirectAccess](da-basic-plan-s2-deployment.md)  
  
