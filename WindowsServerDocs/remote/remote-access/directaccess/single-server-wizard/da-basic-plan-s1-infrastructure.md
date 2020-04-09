---
title: Etapa 1 planejar a infraestrutura básica do DirectAccess
description: Este tópico faz parte do guia implantar um único servidor DirectAccess usando o assistente de Introdução para Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 6bab4adbcf006556c73c96f403afe7538079e967
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80819339"
---
# <a name="step-1-plan-the-basic-directaccess-infrastructure"></a>Etapa 1 planejar a infraestrutura básica do DirectAccess
A primeira etapa para uma implantação básica do DirectAccess em um único servidor é executar o planejamento para a infraestrutura necessária para a implantação. Este tópico descreve as etapas de planejamento de infraestrutura:  
  
|{1&gt;Tarefa&lt;1}|Descrição|  
|----|--------|  
|Planejar topologia e configurações de rede|Decida onde posicionar o servidor DirectAccess \(na borda ou por trás de uma conversão de endereços de rede \(NAT\) dispositivo ou firewall\)e planeje o endereçamento e roteamento de IP.|  
|Planejar requisitos de firewall|Planejar como permitir o DirectAccess por meio de firewalls de borda.|  
|Planejar requisitos de certificado|O DirectAccess pode usar Kerberos ou certificados para autenticação cliente. Nessa implantação básica do DirectAccess, um Proxy Kerberos é configurado automaticamente e a autenticação é obtida usando credenciais do Active Directory.|  
|Planejar os requisitos de DNS|Planeje as configurações DNS para o servidor DirectAccess, servidores de infraestrutura e conectividade do cliente.|  
|Planejar o Active Directory|Planeje seus controladores de domínio e os requisitos do Active Directory.|  
|Planejar objetos de política de grupo|Decida quais GPOs são necessários em sua organização e como criar ou editar os GPOs.|  
  
As tarefas de planejamento não precisam ser feitas em uma ordem específica.  
  
## <a name="plan-network-topology-and-settings"></a><a name="bkmk_1_1_Network_svr_top_settings"></a>Planejar configurações e topologia de rede  
  
### <a name="plan-network-adapters-and-ip-addressing"></a>Planejar adaptadores de rede e endereçamento IP  
  
1.  Identificar a topologia de adaptador de rede que será usada. O DirectAccess pode ser configurado com um dos seguintes:  
  
    -   Com dois adaptadores de rede – na borda com um adaptador de rede conectado à Internet e o outro à rede interna ou por trás de um NAT, firewall ou dispositivo de roteador, com um adaptador de rede conectado a uma rede de perímetro e o outro à parte interna rede.  
  
    -   Atrás de um dispositivo NAT com um adaptador de rede-o servidor DirectAccess é instalado atrás de um dispositivo NAT e o adaptador de rede único está conectado à rede interna.  
  
2.  Identificar seus requisitos de endereçamento IP:  
  
    O DirectAccess usa IPv6 com IPsec para criar uma conexão segura entre os computadores cliente do DirectAccess e a rede interna corporativa. Contudo, o DirectAccess não precisa necessariamente de uma conectividade IPv6 com a Internet ou suporte IPv6 nativo em redes internas. Em vez disso, ele configura automaticamente e usa tecnologias de transição IPv6 para encapsular o tráfego IPv6 na Internet IPv4 \(6to4, Teredo, IP\-HTTPS\) e em seu IPv4\-somente intranet \(NAT64 ou ISATAP\). Para uma visão geral dessas tecnologias de transição, confira os seguintes recursos:  
  
    -   [Tecnologias de transição IPv6](https://technet.microsoft.com/library/bb726951.aspx)  
  
    -   [Especificação de protocolo de túnel IP-HTTPS](https://msdn.microsoft.com/library/dd358571(PROT.10).aspx)  
  
3.  Configure os adaptadores e endereçamento necessários conforme a tabela a seguir. Para implantações por trás de um dispositivo NAT usando um único adaptador de rede, configure seus endereços IP usando apenas a coluna **adaptador de rede interna** .  
  
    ||Adaptador de rede externa|Adaptador de rede interna<sup>1</sup>|Requisitos de roteamento|  
    |-|--------------|--------------------|------------|  
    |Intranet IPv4 e Internet IPv4|Configure o seguinte:<p>-Um endereço IPv4 público estático com a máscara de sub-rede apropriada.<br />-Um endereço IPv4 de gateway padrão do seu firewall de Internet ou provedor de serviços de Internet local \(\) roteador do ISP.|Configure o seguinte:<p>-Um endereço de intranet IPv4 com a máscara de sub-rede apropriada.<br />-Uma conexão\-sufixo DNS específico do seu namespace de intranet. Um servidor DNS também deve ser configurado na interface interna.<br />-Não configure um gateway padrão em nenhuma interface de intranet.|Para configurar o servidor do DirectAccess para acessar todas as sub-redes na rede IPv4 interna, execute este procedimento:<p>1. liste os espaços de endereço IPv4 para todos os locais na sua intranet.<br />2. Use a **rota adicionar \-p** ou **netsh interface IPv4 adicionar** comandos de rota para adicionar os espaços de endereço IPv4 como rotas estáticas na tabela de roteamento IPv4 do servidor DirectAccess.|  
    |Internet IPv6 e intranet IPv6|Configure o seguinte:<p>-Use a configuração de endereço autoconfigurado fornecida pelo seu ISP.<br />-Use o comando **Route Print** para garantir que uma rota IPv6 padrão apontando para o roteador do ISP exista na tabela de roteamento IPv6.<br />-Determine se o ISP e os roteadores de intranet estão usando as preferências de roteador padrão descritas na RFC 4191 e usando uma preferência padrão mais alta do que os roteadores de intranet local. Se as duas situações forem verdadeiras, nenhuma outra configuração para a rota padrão será necessária. A preferência mais alta para o roteador do ISP assegura que a rota IPv6 padrão ativa do servidor DirectAccess aponte para a Internet IPv6.<p>Como o servidor do DirectAccess é um roteador IPv6, caso você tenha uma infraestrutura IPv6 nativa, a interface de Internet também poderá acessar os controladores de domínio na intranet. Nesse caso, adicione filtros de pacote ao controlador de domínio na rede de perímetro que impeçam a conectividade com o endereço IPv6 da interface\-voltada para a Internet do servidor DirectAccess.|Configure o seguinte:<p>-Se você não estiver usando níveis de preferência padrão, configure suas interfaces de intranet com o comando **netsh interface ipv6 set InterfaceIndex ignoredefaultroutes\=Enabled** . Esse comando garante que as rotas padrão adicionais que apontem para os roteadores da intranet não sejam acrescentadas à tabela de roteamento de IPv6. É possível obter o InterfaceIndex das interfaces da intranet na exibição do comando netsh interface show interface.|Se você possui uma intranet IPv6, execute o seguinte procedimento para configurar o servidor do DirectAccess para chegar a todos os locais IPv6:<p>1. liste os espaços de endereço IPv6 para todos os locais na sua intranet.<br />2. Use o comando **netsh interface ipv6 add route** para adicionar os espaços de endereço IPv6 como rotas estáticas na tabela de roteamento IPv6 do servidor DirectAccess.|  
    |Internet IPv4 e intranet IPv6|O servidor do DirectAccess encaminha o tráfego de rota padrão IPv6 para a interface do adaptador Microsoft 6to4 para uma retransmissão 6to4 na Internet IPv4. Você pode configurar um servidor DirectAccess para o endereço IPv4 da retransmissão 6to4 da Microsoft na Internet IPv4 \(usado quando o IPv6 nativo não está implantado na rede corporativa\) com o seguinte comando: netsh interface IPv6 6to4 set relay name\=comando 192.88.99.1 State\=habilitado.|||  
  
    > [!NOTE]  
    > Observe o seguinte:  
    >   
    > 1.  Se um endereço IPv4 público tiver sido atribuído a um cliente do DirectAccess, ele usará tecnologia de transição 6to4 para se conectar à intranet. Se o cliente DirectAccess não puder se conectar ao servidor DirectAccess com o 6to4, ele usará IP\-HTTPS.  
    > 2.  Os computadores cliente IPv6 nativos podem se conectar ao servidor do DirectAccess pelo IPv6 nativo, sem a necessidade de tecnologia de transição.  
  
### <a name="plan-firewall-requirements"></a><a name="ConfigFirewalls"></a>Planejar requisitos de firewall  
Se o servidor DirectAccess estiver atrás de um firewall de borda, as seguintes exceções serão necessárias para o tráfego DirectAccess quando o servidor DirectAccess estiver na Internet IPv4:  
  
-   tráfego 6to4-protocolo IP 41 de entrada e saída.  
  
-   IP\-HTTPS-protocolo de controle de transmissão \(TCP\) porta de destino 443 e a porta de origem TCP 443 de saída.  
  
-   Se você implantar o DirectAccess com um único adaptador de rede e instalar o servidor de local de rede no servidor do DirectAccess, também deverá haver uma exceção para a porta TCP 62000.  
  
    > [!NOTE]  
    > Essa exceção está no servidor DirectAccess. Todas as outras exceções estão no firewall de borda.  
  
As exceções a seguir serão necessárias para o tráfego DirectAccess quando o servidor DirectAccess estiver na Internet IPv6:  
  
-   Protocolo IP 50  
  
-   Entrada da porta de destino UDP 500 e saída da porta de origem UDP 500.  
  
Ao usar firewalls adicionais, aplique as seguintes exceções do firewall de rede interna no tráfego de DirectAcess:  
  
-   ISATAP-protocolo 41 de entrada e saída  
  
-   TCP\/UDP para todo o tráfego IPv4\/IPv6  
  
### <a name="plan-certificate-requirements"></a><a name="bkmk_1_2_CAs_and_certs"></a>Planejar requisitos de certificado  
Os requisitos de certificado para o IPsec incluem um certificado de computador usado pelos computadores cliente do DirectAccess ao estabelecerem a conexão IPsec entre o cliente e o servidor do DirectAccess e um certificado de computador usado pelos servidores do DirectAccess para estabelecer conexões IPsec com clientes do DirectAccess. Para o DirectAccess no Windows Server 2012 R2 e no Windows Server 2012, o uso desses certificados IPsec não é obrigatório. O Assistente de Introdução configura o servidor do DirectAccess para agir como um proxy Kerberos para realizar a autenticação IPsec sem a necessidade de certificados.
  
1.  **Servidor IP\-https**. Quando você configura o DirectAccess, o servidor DirectAccess é configurado automaticamente para atuar como o ouvinte da Web IP\-HTTPS. O site IP\-HTTPS requer um certificado de site e os computadores cliente devem ser capazes de contatar a lista de certificados revogados \(site CRL\) para o certificado. O assistente para Habilitar DirectAccess tenta usar o certificado SSTP VPN. Se o SSTP não estiver configurado, ele verificará se um certificado para IP\-HTTPS está presente no repositório pessoal do computador. Se nenhum estiver disponível, ele criará automaticamente um certificado auto\-assinado.
  
2.  **Servidor de local de rede**. O servidor de local de rede é um site usado para detectar se os computadores cliente estão localizados na rede corporativa. O servidor de local de rede requer um certificado de site. Clientes do DirectAccess devem poder contatar o site da CRL para o certificado. O assistente para habilitar acesso remoto verifica se um certificado para o servidor de local de rede está presente no repositório pessoal do computador. Se não estiver presente, ele criará automaticamente um certificado auto\-assinado.
  
Os requisitos de certificação para cada um deles estão resumidos na tabela a seguir:  
  
|Autenticação IPsec|Servidor IP\-HTTPS|Servidor de local da rede|  
|------------|----------|--------------|  
|Uma AC interna é necessária para emitir certificados de computador para o servidor DirectAccess e clientes para autenticação IPsec quando você não usa o proxy Kerberos para autenticação|AC pública-é recomendável usar uma AC pública para emitir o certificado IP\-HTTPS, isso garante que o ponto de distribuição da CRL esteja disponível externamente.|CA interna-você pode usar uma AC interna para emitir o certificado do site do servidor do local de rede. Verifique se o ponto de distribuição de CRL está altamente disponível pela rede interna.|  
||CA interna-você pode usar uma AC interna para emitir o certificado IP\-HTTPS; no entanto, você deve verificar se o ponto de distribuição da CRL está disponível externamente.|Certificado auto\-assinado-você pode usar um certificado auto\-assinado para o site do servidor do local de rede; no entanto, você não pode usar um certificado auto\-assinado em implantações multissite.|  
||Certificado auto\-assinado-você pode usar um certificado auto\-assinado para o servidor IP\-HTTPS; no entanto, você deve verificar se o ponto de distribuição da CRL está disponível externamente. Um certificado auto\-assinado não pode ser usado em uma implantação multissite.||  
  
#### <a name="plan-certificates-for-ip-https-and-network-location-server"></a><a name="bkmk_website_cert_IPHTTPS"></a>Planejar certificados para IP\-HTTPS e servidor de local de rede  
Se desejar provisionar um certificado para esses fins, consulte [Implantar um único servidor DirectAccess com configurações avançadas](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md). Se nenhum certificado estiver disponível, o assistente de Introdução criará automaticamente\-certificados assinados para essas finalidades.
  
> [!NOTE]
> Se você provisionar certificados para IP\-HTTPS e o servidor de local de rede manualmente, verifique se os certificados têm um nome de entidade. Se o certificado não tiver um nome de assunto, mas tiver um nome alternativo, ele não será aceito pelo assistente DirectAccess.  
  
#### <a name="plan-dns-requirements"></a>Planejar os requisitos de DNS  
Em uma implantação do DirectAccess, é necessário DNS para o seguinte:  
  
-   **Solicitações de cliente do DirectAccess**. O DNS é usado para resolver solicitações de computadores cliente do DirectAccess não localizados na rede interna. Os clientes do DirectAccess tentam se conectar ao servidor de local da rede DirectAccess para determinar se encontram-se na Internet ou na rede corporativa: Se a conexão for efetuada com êxito, os clientes serão determinados como estando na intranet e o DirectAccess não será usado, e as solicitações do cliente serão resolvidas usando o servidor DNS configurado no adaptador de rede do computador cliente. Se a conexão não for bem-sucedida, presume-se que os clientes estejam na Internet. Os clientes do DirectAccess usarão a tabela de política de resolução de nome \(\) NRPT para determinar qual servidor DNS usar ao resolver solicitações de nome. Você pode especificar que os clientes devem usar o DirectAccess DNS64 para resolver nomes ou um servidor DNS interno alternativo. Ao realizar a resolução do nome, a NRPT é usada pelos clientes do DirectAccess para identificar como lidar com uma solicitação. Os clientes solicitam um FQDN ou um único nome de rótulo de\-, como http:\/\/interno. Se um único nome de rótulo de\-for solicitações, um sufixo DNS será anexado para criar um FQDN. Se a consulta DNS for correspondente a uma entrada na NRPT, e DNS4 ou um servidor de intranet DNS for especificado para a entrada, então, a consulta é enviada para resolução de nome usando o servidor especificado. Se houver correspondência, mas nenhum servidor DNS foi especificado, isso indicará uma exceção à regra e a resolução de nome normal é aplicada.  
  
    Observe que quando um novo sufixo é adicionado à NRPT no console de Gerenciamento do DirectAccess, os servidores DNS padrão do sufixo podem ser descobertos automaticamente, clicando em **Detectar**. A detecção automática funciona da seguinte maneira:  
  
    1.  Se a rede corporativa for IPv4\-baseado, ou IPv4 e IPv6, o endereço padrão será o endereço DNS64 do adaptador interno no servidor DirectAccess.  
  
    2.  Se a rede corporativa for baseada em\-IPv6, o endereço padrão será o endereço IPv6 dos servidores DNS na rede corporativa.  
  
-   **Servidores de infraestrutura**
  
    1.  **Servidor de local de rede**. Os clientes DirectAccess tentam acessar o servidor de local de rede para determinar se estão em uma rede interna. Os clientes na rede interna devem poder resolver o nome do servidor de local de rede, mas não deverão resolver o nome quando se encontrarem na Internet. Para garantir que isso ocorra, o FQDN do servidor do local de rede é adicionado, por padrão, a uma regra de exceção da NRPT. Ao configurar o DirectAccess, as seguintes regras também serão criadas automaticamente:  
  
        1.  Uma regra de sufixo DNS para o nome de domínio do servidor DirectAccess e os endereços IPv6 correspondentes aos servidores DNS da intranet configurados no servidor DirectAccess. Por exemplo, se o servidor do DirectAccess for membro do domínio corp.contoso.com, é criada uma regra para o sufixo de DNS corp.contoso.com.  
  
        2.  Uma regra de isenção para o FQDN do servidor de local de rede. Por exemplo, se a URL do servidor de local de rede for https:\/\/nls.corp.contoso.com, uma regra de isenção será criada para o FQDN nls.corp.contoso.com.  
  
        **Servidor IP\-https**. O servidor DirectAccess atua como um ouvinte de IP\-HTTPS e usa seu certificado de servidor para autenticar para clientes de IP\-HTTPS. O nome HTTPS do IP\-deve ser resolvido pelos clientes do DirectAccess usando servidores DNS públicos.  
  
        **Verificadores de conectividade**. O DirectAccess cria uma investigação Web padrão usada pelos computadores cliente do DirectAccess para verificar a conectividade com a rede interna. Para garantir que a sonda funcione conforme esperado, os nomes a seguir devem ser registrados manualmente no DNS:  
  
        1.  DirectAccess\-webprobehost-devem ser resolvidos para o endereço IPv4 interno do servidor DirectAccess ou para o endereço IPv6 em um único ambiente de\-IPv6.  
  
        2.  o DirectAccess\-corpconnectivityhost-deve resolver para o endereço de\) de loopback do localhost \(. O registro A e AAAA deve ser criado. Registro A com um valor 127.0.0.1 e registro AAAA com valor construído a partir do prefixo NAT64 com os últimos 32 bits como 127.0.0.1. O prefixo NAT64 pode ser recuperado executando o cmdlet Get\-netnattransitionconfiguration.  
  
        Você pode criar verificadores de conectividade adicional usando outros endereços da Web via HTTP ou PING. Uma entrada de DNS deverá existir para cada verificador de conectividade.  
  
#### <a name="dns-server-requirements"></a>Requisitos de servidor DNS  
  
-   Para clientes DirectAccess, você deve usar um servidor DNS que esteja executando o Windows Server 2008, o Windows Server 2008 R2, o Windows Server 2012, o Windows Server 2012 R2, o Windows Server 2016 ou qualquer servidor DNS que ofereça suporte a IPv6.  
  
> [!NOTE]  
> Não é recomendável que você use servidores DNS que estejam executando o Windows Server 2003, quando estiver implantando o DirectAccess. Embora os servidores DNS do Windows Server 2003 ofereçam suporte a registros IPv6, o Windows Server 2003 não tem mais o suporte da Microsoft. Além disso, você não deve implantar o DirectAccess se os controladores de domínio estiverem executando o Windows Server 2003 devido a um problema com o Serviço de replicação de arquivos. Para obter mais informações, consulte [configurações sem suporte do DirectAccess](../DirectAccess-Unsupported-Configurations.md).  
  
### <a name="plan-the-network-location-server"></a><a name="bkmk_1_4_NLS"></a>Planejar o servidor do local de rede  
O servidor de local da rede é um site da Web usado para detectar se os clientes DirectAccess se encontram na rede corporativa. Os clientes na rede corporativa não usam o DirectAccess para acessar recursos internos, mas, em vez disso, se conectam diretamente.  
  
O Assistente de Introdução define automaticamente o servidor de local de rede no servidor DirectAccess e o site da Web é criado automaticamente quando você implanta o DirectAccess. Isso permite uma instalação simples sem o uso de uma infraestrutura de certificado.
  
Se você quiser implantar um servidor de local de rede e não usar\-certificados assinados automaticamente, consulte [implantar um único servidor DirectAccess com configurações avançadas](../single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md).
  
### <a name="plan-active-directory"></a><a name="bkmk_1_6_AD"></a>Active Directory de plano  
O DirectAccess usa Active Directory e Active Directory objetos de política de grupo da seguinte maneira:
  
-   **Autenticação**. O Active Directory é usado para autenticação. O túnel do DirectAccess usa autenticação Kerberos para o usuário acessar recursos internos.
  
-   **Objetos de política de grupo**. O DirectAccess reúne as definições de configuração nos objetos da política de grupo que são aplicados aos servidores e clientes do DirectAccess.
  
-   **Grupos de segurança**. O DirectAccess usa grupos de segurança para reunir e identificar computadores cliente do DirectAccess e servidores do DirectAccess. As políticas de grupo são aplicadas ao grupo de segurança necessário.

**Requisitos de Active Directory**  
  
Ao planejar o Active Directory para uma implantação do DirectAccess, os procedimentos a seguir serão necessários:  
  
-   Pelo menos um controlador de domínio instalado no Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008. 
  
    Se o controlador de domínio estiver em uma rede de perímetro \(e, portanto, acessível do adaptador de rede\-voltado para a Internet do servidor DirectAccess\) impedir que o servidor DirectAccess chegue a ele adicionando filtros de pacote no controlador de domínio, para evitar a conectividade com o endereço IP do adaptador da Internet.  
  
-   O servidor do DirectAccess deve ser um membro do domínio.  
  
-   Os clientes do DirectAccess devem ser membros do domínio. Os clientes podem pertencer a:  
  
    -   Qualquer domínio na mesma floresta que o servidor do DirectAccess.  
  
    -   Qualquer domínio que tenha duas\-forma confiável com o domínio do servidor DirectAccess.  
  
    -   Qualquer domínio em uma floresta que tenha duas\-maneira de confiar com a floresta à qual o domínio do DirectAccess pertence.  
  
> [!NOTE]
> - O servidor DirectAccess não pode ser um controlador de domínio.  
> - O controlador de domínio Active Directory usado para o DirectAccess não deve estar acessível a partir do adaptador de Internet externo do servidor DirectAccess \(o adaptador não deve estar no perfil de domínio do\)de firewall do Windows.  
  
### <a name="plan-group-policy-objects"></a><a name="bkmk_1_7_GPOs"></a>Planejar Política de Grupo objetos  
As configurações do DirectAccess definidas quando você configura o DirectAccess são coletadas em objetos de diretiva de grupo \(\)de GPO. Dois GPOs diferentes são preenchidos com definições do DirectAccess e distribuídos como segue:  
  
-   **GPO de cliente do DirectAccess**. Este GPO contém configurações do cliente, inclusive configurações da tecnologia de transição IPv6, entradas da NRPT e regras de segurança de conexão do Firewall do Windows com Segurança Avançada. O GPO é aplicado a grupos de segurança especificados para os computadores cliente.  
  
-   **GPO de servidor do DirectAccess**. Este GPO contém definições de configurações do DirectAccess aplicadas a qualquer servidor configurado como servidor do DirectAccess na implantação. Ele contém também as regras de segurança de conexão do Firewall do Windows com Segurança Avançada.  
  
Os GPOs podem ser configurados de duas maneiras:  
  
1.  **Automaticamente**. Você pode especificar para que sejam criados automaticamente. Um nome padrão é especificado para cada GPO. Os GPOs são criados automaticamente pelo Assistente de Introdução.
  
2.  **Manualmente**. Você pode usar GPOs predefinidos pelo administrador do Active Directory.
  
Observe que depois de o DirectAccess ser configurado para usar GPOs específicos, ele não poderá ser configurado para usar GPOs diferentes.
  
> [!IMPORTANT]
> Seja em GPOs configurados automática ou manualmente, será necessário adicionar uma política para detecção de links lentos se os clientes forem usar 3G. O caminho de Política de Grupo para a **política: configurar política de grupo detecção de vínculo lento** é: **configuração do computador \/ políticas \/ Modelos Administrativos \/ sistema \/** política de grupo.  
  
> [!CAUTION]  
> Use o seguinte procedimento para fazer backup de todos os objetos do DirectAccess Política de Grupo antes de executar os cmdlets do DirectAccess: [fazer backup e restaurar a configuração do DirectAccess](https://go.microsoft.com/fwlink/?LinkID=257928)  
  
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
  
#### <a name="manually-created-gpos"></a>\-manualmente os GPOs criados  
Observe o seguinte ao usar manualmente\-GPOs criados:  
  
-   Os GPOs devem existir antes de o Assistente de Introdução de Acesso Remoto ser executado.  
  
-   Ao usar manualmente\-GPOs criados, para aplicar as configurações do DirectAccess, o administrador do DirectAccess requer permissões totais de GPO \(editar, excluir, modificar a segurança\) nos GPOs\-criados manualmente.  
  
-   Ao usar GPOs criados manualmente, uma pesquisa é feita para um link para o GPO no domínio inteiro. Se o GPO não estiver vinculado ao domínio, um link é criado automaticamente na raiz do domínio. Se as permissões necessárias para criar o link não estiverem disponíveis, será emitido um aviso.  
  
Observe que se as permissões corretas às quais vincular GPOs não existirem, um aviso será emitido. A operação de DirectAccess continuará, porém não ocorrerá a vinculação. Se o aviso for emitido, não será possível criar links automaticamente, mesmo se as permissões forem adicionadas posteriormente. Em vez disso, o administrador precisará criar os links manualmente.  
  
#### <a name="recovering-from-a-deleted-gpo"></a>Recuperando um GPO excluído  
Se um GPO do servidor DirectAccess, cliente ou servidor de aplicativos tiver sido excluído por acidente e não houver nenhum backup disponível, você deverá remover os parâmetros de configuração e re\-configurar novamente. Se um backup estiver disponível, você poderá usá-lo para restaurar o GPO.  
  
O **Gerenciamento do DirectAccess** exibirá a seguinte mensagem de erro: **não é possível encontrar o GPO <GPO name>** . Para remover as definições de configuração, siga essas etapas:  
  
1.  Execute o cmdlet do PowerShell **Uninstall\-RemoteAccess**.  
  
2.  \-Abra o **Gerenciamento do DirectAccess**.  
  
3.  Você verá uma mensagem de erro indicando que o GPO não foi encontrado. Clique em **Remover definições de configuração**. Após a conclusão, o servidor será restaurado para um estado de\-configurado.  
  
### <a name="next-step"></a><a name="BKMK_Links"></a>Próxima etapa  
  
-   [Etapa 2: planejar a implantação básica do DirectAccess](da-basic-plan-s2-deployment.md)  
  
