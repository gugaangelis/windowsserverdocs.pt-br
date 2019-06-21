---
title: Etapa 1 de plano a infraestrutura do DirectAccess avançado
description: Este tópico faz parte do guia de implantar um único servidor DirectAccess com avançadas configurações para o Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: aa3174f3-42af-4511-ac2d-d8968b66da87
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 339189928d3ce5403d0fca4a06efc36b867e2a50
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281781"
---
# <a name="step-1-plan-the-advanced-directaccess-infrastructure"></a>Etapa 1 de plano a infraestrutura do DirectAccess avançado

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

O primeiro passo do planejamento da implantação avançada do DirectAccess em um único servidor é planejar a infraestrutura necessária para a implantação. Este tópico descreve as etapas de planejamento da infraestrutura. Essas tarefas de planejamento não precisam ser concluídas em uma ordem específica.  
  
|Tarefa|Descrição|
|----|--------|  
|[1.1 planejar a topologia de rede e configurações](#11-plan-network-topology-and-settings)|Decidir onde colocar o servidor do DirectAccess (na borda, atrás de um dispositivo NAT [conversão de endereços de rede] ou de um firewall) e planejar o endereçamento IP, roteamento e criação de túneis à força.|  
|[1.2 planejar requisitos de firewall](#12-plan-firewall-requirements)|Planejar como permitir o tráfego do DirectAccess através de firewalls de borda.|  
|[1.3 planejar requisitos de certificado](#13-plan-certificate-requirements)|Decidir entre usar Kerberos ou certificados para autenticação do cliente e planejar seus certificados de site. O IP-HTTPS é um protocolo de transição usado por clientes do DirectAccess para tráfego IPv6 de túnel em redes IPv4. Decidir entre autenticar no servidor IP-HTTPS com um certificado emitido por uma AC (autoridade de certificação) ou com um certificado autoassinado emitido automaticamente pelo servidor do DirectAccess.|  
|[1.4 planejar os requisitos de DNS](#14-plan-dns-requirements)|Planejar as configurações do DNS (Domain Name System) para o servidor do DirectAccess, servidores de infraestrutura, opções de resolução de nome local e conectividade de cliente.|  
|[1.5 planejar o servidor de local de rede](#15-plan-the-network-location-server)|O servidor de local de rede é usado por clientes DirectAccess para determinar se estão localizados na rede interna. Decidir onde colocar o site do servidor de local de rede na sua organização (no servidor do DirectAccess ou em um servidor alternativo) e planejar os requisitos de certificado caso o servido de local de rede localize-se no servidor do DirectAccess.|  
|[1.6 planejar os servidores de gerenciamento](#16-plan-management-servers)|Você pode gerenciar os computadores cliente do DirectAccess localizados fora da rede corporativa na Internet. Plano para servidores de gerenciamento (como servidores de atualização) que são utilizados durante o gerenciamento de cliente remoto.|  
|[1.7 planejar serviços de domínio do Active Directory](#17-plan-active-directory-domain-services)|Planejar os controladores de domínio, seus requisitos de Active Directory, autenticação de cliente e múltiplos domínios.|  
|[1.8 objetos de diretiva de grupo do plano de](#18-plan-group-policy-objects)|Decida quais GPOs são necessários em sua organização e como criar ou editar os GPOs.|  
  
## <a name="11-plan-network-topology-and-settings"></a>1.1 Planejar topologia e configurações de rede

Esta seção explica como planejar sua rede, incluindo:  
  
- [1.1.1 planejar adaptadores de rede e endereçamento IP](#111-plan-network-adapters-and-ip-addressing)  
  
- [1.1.2 planejar conectividade da intranet IPv6](#112-plan-ipv6-intranet-connectivity)  
  
- [1.1.3 planejar de túneis à força](#113-plan-for-force-tunneling)  
  
### <a name="111-plan-network-adapters-and-ip-addressing"></a>1.1.1 Planejar adaptadores de rede e endereçamento IP  
  
1. Identificar a topologia de adaptador de rede que será usada. O DirectAccess pode ser configurado usando uma das topologias a seguir:  
  
    - **Dois adaptadores de rede**. O servidor do DirectAccess pode ser instalado na borda com um adaptador de rede conectado à Internet e outro à rede interna, ou então instalado atrás de um NAT, firewall ou roteador, com um adaptador de rede conectado à rede de perímetro e outro à rede interna.  
  
    - **Um adaptador de rede**. O servidor do DirectAccess é instalado atrás de um dispositivo NAT e o adaptador de rede único fica conectado à rede interna.  
  
2. Identificar os requisitos de endereçamento IP:  
  
    O DirectAccess usa IPv6 com IPsec para criar uma conexão segura entre os computadores cliente do DirectAccess e a rede interna corporativa. Contudo, o DirectAccess não precisa necessariamente de uma conectividade IPv6 com a Internet ou suporte IPv6 nativo em redes internas. Em vez disso, ele configura e usa automaticamente tecnologias de transição IPv6 para criar um túnel de tráfego IPv6 pela Internet IPv4 (usando 6to4, Teredo ou IP-HTTPS) e pela intranet somente IPv4 (usando NAT64 ou ISATAP). Para uma visão geral dessas tecnologias de transição, confira os seguintes recursos:  
  
    - [Tecnologias de transição IPv6](https://technet.microsoft.com/library/bb726951.aspx)  
  
    - [Especificação do protocolo de túnel IP-HTTPS](https://msdn.microsoft.com/library/dd358571(PROT.10).aspx)  
  
3. Configure os adaptadores e endereços necessários conforme a tabela a seguir. Para implantações que usam um único adaptador de rede e são configurados atrás de um dispositivo NAT, configure os endereços IP usando somente a coluna **Adaptador de rede interna**.  
  
    ||Adaptador de rede externa|Adaptador de rede interna|Requisitos de roteamento|  
    |-|--------------|--------------|------------|  
    |Internet IPv4 e intranet IPv4|Configure dois endereços IPv4 públicos, consecutivos e estáticos com as máscaras de sub-rede apropriadas (necessário somente para Teredo).<br/><br/>Configure também o endereço padrão IPv4 de gateway do firewall da Internet ou roteador do ISP (provedor de serviços de Internet) local. **Observação:** O servidor do DirectAccess precisa de dois endereços IPv4 públicos e consecutivos para que possa agir como um servidor Teredo e para que os clientes baseados no Windows possam usar o servidor do DirectAccess para detectar o tipo de dispositivo NAT atrás do qual eles se encontram.|Configure o seguinte:<br/><br/>-Um endereço de intranet IPv4 com a máscara de sub-rede apropriada.<br/>-O sufixo DNS específico da conexão do seu namespace da intranet. Um servidor DNS também deverá ser configurado na interface interna. **Cuidado:** Não configure um gateway padrão em nenhuma interface da intranet.|Para configurar o servidor do DirectAccess para acessar todas as sub-redes na rede IPv4 interna, execute este procedimento:<br/><br/>-Lista os espaços de endereço IPv4 para todos os locais na sua intranet.<br/>– Use o **rota adicionar -p** ou o**netsh interface ipv4 Adicionar rota** comando para adicionar os espaços de endereço IPv4 como rotas estáticas na tabela de roteamento IPv4 do servidor DirectAccess.|  
    |Internet IPv6 e intranet IPv6|Configure o seguinte:<br/><br/>-Use a configuração de endereço que é fornecida por seu ISP.<br/>– Use o **Route Print** comando para garantir que existe uma rota IPv6 padrão, e ele está apontando para o roteador do ISP na tabela de roteamento IPv6.<br/>-Determine se os roteadores do ISP e da intranet estiver usando as preferências de roteador padrão descritas no RFC 4191 e usando uma preferência padrão mais alta que os roteadores de intranet local.<br/>    Se as duas situações forem verdadeiras, nenhuma outra configuração para a rota padrão será necessária. A preferência mais alta para o roteador do ISP assegura que a rota IPv6 padrão ativa do servidor DirectAccess aponte para a Internet IPv6.<br/><br/>Como o servidor do DirectAccess é um roteador IPv6, caso você tenha uma infraestrutura IPv6 nativa, a interface de Internet também poderá acessar os controladores de domínio na intranet. Nesse caso, adicione filtros de pacote ao controlador de domínio na rede de perímetro para evitar a conectividade com o endereço IPv6 da interface voltada para Internet do servidor do DirectAccess.|Configure o seguinte:<br/><br/>– Se você não estiver usando os níveis de preferência padrão, você pode configurar as interfaces da intranet usando o comando a seguir**netsh interface ipv6 definir InterfaceIndex ignoredefaultroutes = habilitado**.<br/>    Esse comando garante que as rotas padrão adicionais que apontam para os roteadores da intranet não sejam acrescentadas à tabela de roteamento de IPv6. Você pode ver o índice de interface das interfaces da intranet usando o seguinte comando: **netsh interface ipv6 show interface**.|Se você possui uma intranet IPv6, execute o seguinte procedimento para configurar o servidor do DirectAccess para chegar a todos os locais IPv6:<br/><br/>-Lista os espaços de endereço IPv6 para todos os locais na sua intranet.<br/>– Use o **netsh interface ipv6 Adicionar rota** comando para adicionar os espaços de endereço IPv6 como rotas estáticas na tabela de roteamento IPv6 do servidor DirectAccess.|  
    |Internet IPv4 e intranet IPv6|O servidor do DirectAccess encaminha o tráfego de rota padrão IPv6 para o adaptador Microsoft 6to4 para uma retransmissão 6to4 na Internet IPv4. Você pode configurar o servidor do DirectAccess para o endereço IPv4 do adaptador Microsoft 6to4 usando o seguinte comando: `netsh interface ipv6 6to4 set relay name=<ipaddress> state=enabled`.|||  
  
    > [!NOTE]  
    > - Se um endereço IPv4 público tiver sido atribuído a um cliente do DirectAccess, ele usará tecnologia de transição 6to4 para se conectar à intranet. Se um endereço IPv4 privado for atribuído, ele usará Teredo. Se o cliente do DirectAccess não puder se conectar ao servidor do DirectAccess com 6to4 ou Teredo, ele usará IP-HTTPS.  
    > - Para usar o Teredo, você precisa configurar dois endereços IP consecutivos no adaptador de rede voltado para o exterior.  
    > - Você não poderá usar o Teredo se o servidor do DirectAccess possuir somente um adaptador de rede.  
    > - Os computadores cliente IPv6 nativos podem se conectar ao servidor do DirectAccess pelo IPv6 nativo, sem a necessidade de tecnologia de transição.  
  
### <a name="112-plan-ipv6-intranet-connectivity"></a>1.1.2 Planejar conectividade da intranet IPv6

O IPv6 é necessário para gerenciar clientes remotos do DirectAccess. O IPv6 permite que os servidores de gerenciamento DirectAccess conectem-se aos clientes DirectAccess localizados na Internet para fins de gerenciamento remoto.  
  
> [!NOTE]  
> - Não é necessário usar IPv6 na rede para dar suporte a conexões iniciadas pelos computadores cliente do DirectAccess para recursos IPv4 na rede da sua organização. NAT64/DNS64 é usado para esta finalidade.  
> - Se você não estiver gerenciando clientes DirectAccess remotos, não precisará implantar IPv6.  
> - O ISATAP (Intra-Site Automatic Tunnel Addressing Protocol) não tem suporte em implantações do DirectAccess.  
> - Ao usar IPv6, você pode habilitar consultas de registros de recursos do IPv6 (AAAA) do host para o DNS64 usando o seguinte comando do Windows PowerShell:   **Set-NetDnsTransitionConfiguration -OnlySendAQuery $false**.  
  
### <a name="113-plan-for-force-tunneling"></a>1.1.3 Planejar a criação de túneis à força

Contando com o IPv6 e a NRPT (Tabela de Políticas de Resolução de Nomes) por padrão, os clientes do DirectAccess separam os tráfegos da intranet e da Internet da seguinte maneira:  
  
- O nome DNS (Sistema de Nomes de Domínio) consulta FQDNs (nomes de domínio totalmente qualificados) da intranet e todo o tráfego da intranet é trocado nos túneis criados com o servidor do DirectAccess ou diretamente com os servidores da intranet. O tráfego da intranet dos clientes DirectAccess é o tráfego IPv6.  
  
- O nome DNS consulta FQDNs que correspondam às regras de isenção ou que não correspondam ao namespace da intranet e todo o tráfego para os servidores da Internet é trocado na interface física, que está conectada à Internet. O tráfego da Internet dos clientes DirectAccess é geralmente o tráfego IPv4.  
  
Por outro lado, algumas implementações de VPN (rede virtual privada) de acesso remoto, inclusive do cliente da VPN, enviam por padrão todo o tráfego de intranet e Internet pela conexão de VPN de acesso remoto. O tráfego limitado pela Internet é roteado pelo servidor da VPN para os servidores proxy Web IPv4 da intranet, para ter acesso aos recursos IPv4 da Internet. É possível separar o tráfego de intranet e Internet para clientes VPN de acesso remoto usando túneis divididos. Isso requer configurar a tabela de roteamento IP nos clientes VPN para que o tráfego de locais na intranet seja enviado pela conexão de VPN, enquanto para todos os outros locais o tráfego é enviado usando a interface física conectada à Internet.  
  
É possível configurar clientes DirectAccess para enviar todo o tráfego pelos túneis para o servidor DirectAccess com a criação de túneis à força. Quando a criação de túneis à força estiver configurada, os clientes do DirectAccess detectarão que estão na Internet e removerão a rota padrão IPv4. Com a exceção do tráfego de sub-rede local, todo o tráfego enviado pelo cliente DirectAccess é um tráfego IPv6 que passa por túneis para o servidor DirectAccess.  
  
> [!IMPORTANT]  
> Se você estiver pensando em usar a criação de túneis à força ou em adicioná-la no futuro, implante uma configuração de dois túneis. Por questões de segurança, não há suporte para a criação de túneis à força na configuração de um único túnel.  
  
A habilitação da criação de túneis à força possui estas consequências:  
  
- Os clientes DirectAccess usam apenas o protocolo IP-HTTPS para obter conectividade IPv6 com o servidor DirectAccess pela Internet IPv4.  
  
- Os únicos locais que um cliente DirectAccess pode acessar por padrão, com tráfego IPv4, são aqueles em sua sub-rede local. Qualquer outro tipo de tráfego enviado pelos aplicativos e serviços executados no cliente do DirectAccess é tráfego IPv6 enviado pela conexão do DirectAccess. Portanto, os aplicativos compatíveis somente com IPv4 no cliente DirectAccess não podem ser usados para acessar os recursos da Internet, exceto aqueles na sub-rede local.  
  
> [!IMPORTANT]  
> Para a criação de túneis à força com DNS64 e NAT64, é necessário implementar a conectividade da Internet IPv6. Uma maneira de fazer isso é permitir que o prefixo IP-HTTPS possa ser roteado globalmente, para que o ipv6.msftncsi.com possa ser contatado por IPv6 e a resposta do servidor de Internet para clientes IP-HTTPS possa retornar pelo servidor do DirectAccess.  
>
> Como isso não é viável na maioria dos casos, a melhor opção é criar servidores NCSI virtuais dentro das redes corporativas, dessa forma:  
>
> 1. Adicionar uma entrada NRPT para ipv6.msftncsi.com e resolvê-la no DNS64 para um site interno (que pode ser um site IPv4).  
> 2. Adicionar uma entrada NRPT para dns.msftncsi.com e resolvê-la em um servidor DNS corporativo para retornar o registro do recurso do host IPv6 (AAAA) fd3e:4f5a:5b81::1. (Usar o DNS64 para enviar apenas as consultas de registro de recurso do host [A] para o FQDN pode não funcionar, pois ele é configurado somente em implantações IPv4, devendo por isso ser configurado para resolução diretamente no DNS corporativo.)  
  
## <a name="12-plan-firewall-requirements"></a>1.2 Planejar requisitos de firewall

Se o servidor do DirectAccess estiver atrás de um firewall de borda, as seguintes exceções para tráfego de Acesso Remoto serão necessárias quando o servidor do DirectAccess estiver na Internet IPv4:  
  
- Tráfego de Teredo-usuário porta de destino 3544 do protocolo de datagrama (UDP) entrada e a porta de origem UDP 3544 de saída.  
  
- Protocolo 41 de entrada e saída do tráfego IP 6to4.  
  
- Porta de destino 443 do IP-HTTPS-protocolo TCP (Transmission Control) e a porta de origem TCP 443 de saída.  
  
- Se você implantar o Acesso Remoto com um único adaptador de rede e instalar o servidor de local de rede no servidor do DirectAccess, também deverá haver uma exceção para a porta TCP 62000.  
  
    > [!NOTE]  
    > Esta exceção está no servidor do DirectAccess e todas as demais exceções estão no firewall da borda.  
  
Para tráfego Teredo e 6to4, essas exceções devem ser aplicadas para os endereços IPv4 públicos consecutivos voltados para a Internet no servidor do DirectAccess. Para IP-HTTPS, as exceções precisam ser aplicadas no endereço registrado no servidor DNS público.  
  
As exceções a seguir serão necessárias para o tráfego de Acesso Remoto quando o servidor do DirectAccess estiver em Internet IPv6:  
  
- Protocolo IP ID 50  
  
- Entrada da porta de destino UDP 500 e saída da porta de origem UDP 500  
  
- Tráfego ICMPv6 de entrada e saída (somente usando Teredo)  
  
Ao usar firewalls adicionais, aplique as seguintes exceções do firewall de rede interna no tráfego de Acesso Remoto:  
  
- Protocolo ISATAP 41 de entrada e saída  
  
- TCP/UDP para todo o tráfego IPv4 e IPv6  
  
- ICMP para todo o tráfego IPv4 e IPv6 (somente usando Teredo)  
  
## <a name="13-plan-certificate-requirements"></a>1.3 Planejar requisitos de certificado

Existem três cenários que pedem certificados ao implantar um único servidor do DirectAccess:  
  
- [1.3.1 planejar certificados de computador para autenticação IPsec](#131-plan-computer-certificates-for-ipsec-authentication)  
  
    Os requisitos de certificado para o IPsec incluem: um certificado de computador usado pelos computadores cliente do DirectAccess ao estabelecerem a conexão IPsec entre o cliente e o servidor do DirectAccess e um certificado de computador usado pelos servidores do DirectAccess para estabelecer conexões IPsec com clientes do DirectAccess.  
  
    O uso desses certificados IPsec não é obrigatório para o DirectAccess no Windows Server 2012. Como alternativa, o servidor do DirectAccess pode agir como um proxy Kerberos para realizar a autenticação IPsec sem a necessidade de certificados. Se o protocolo Kerberos for usado, ele funciona por SSL, e o proxy do Kerberos usa o certificado configurado para IP-HTTPS para esta finalidade. Alguns cenários empresariais (inclusive implantação em múltiplos sites e autenticação de cliente por OTP [senha única]) requerem o uso de autenticação de certificado e não do protocolo Kerberos.  
  
-   [1.3.2 planejar certificados para IP-HTTPS](#132-plan-certificates-for-ip-https)  
  
    Quando você configurar o Acesso Remoto, o servidor do DirectAccess será configurado automaticamente para atuar como um ouvinte IP-HTTPS. O site IP-HTTPS precisa de um certificado de site, e os computadores cliente também devem poder contatar o site da CRL (lista de certificados revogados) do certificado.  
  
-   [1.3.3 planejar certificados de site para o servidor de local de rede](#133-plan-website-certificates-for-the-network-location-server)  
  
    O servidor de local da rede é um site usado para detectar se os computadores cliente encontram-se na rede corporativa. O servidor local da rede precisa de um certificado de site. Clientes do DirectAccess devem poder contatar o site da CRL para o certificado.  
  
Os requisitos de autoridade de certificação (AC) de cada cenário são resumidos na tabela a seguir.  
  
|Autenticação IPsec|Servidor IP-HTTPS|Servidor de local da rede|  
|------------|----------|--------------|  
|Uma CA interna é necessária para emitir certificados de computador para o servidor do DirectAccess e clientes para a autenticação IPsec quando você não usa o proxy do Kerberos para autenticação|AC interna:<br/><br/>você pode usar uma AC interna para emitir o certificado de IP-HTTPS; contudo, deverá verificar se o ponto de distribuição de CRL está disponível externamente|AC interna:<br/><br/>Você pode usar uma AC interna para emitir o certificado de site do servidor de local de rede. Certifique-se de que o ponto de distribuição da CRL tenha alta disponibilidade da rede interna.|  
||Certificado autoassinado:<br/><br/>Você pode usar um certificado autoassinado para o servidor IP-HTTPS, contudo deverá certificar-se de que o ponto de distribuição da CRL esteja disponível externamente.<br/><br/>Um certificado autoassinado não pode ser usado em implantações em múltiplos sites.|Certificado autoassinado:<br/><br/>Você pode usar um certificado autoassinado para o site do servidor de local de rede.<br/><br/>Um certificado autoassinado não pode ser usado em implantações em múltiplos sites.|  
||**Recomendado**<br/><br/>AC pública:<br/><br/>É recomendável usar uma AC pública para emitir o certificado IP-HTTPS. Isso garante que o ponto de distribuição da CRL esteja disponível externamente.|  
  
### <a name="131-plan-computer-certificates-for-ipsec-authentication"></a>1.3.1 Planejar certificados de computador para autenticação IPsec  
Se você estiver usando uma autenticação IPsec baseada em certificados, o servidor e os clientes do DirectAccess precisarão obter um certificado de computador. A maneira mais simples de instalar os certificados é configurar um registro automático baseado em políticas de grupo para certificados de computador. Isso garante que todos os membros do domínio obtenham um certificado de uma AC corporativa. Se você não tiver uma AC corporativa configurada na sua organização, consulte [serviços de certificados do Active Directory](https://technet.microsoft.com/library/cc770357.aspx).  
  
Esse certificado possui os seguintes requisitos:  
  
-   O certificado deverá ter um EKU (uso estendido de chave) de autenticação do cliente.  
  
-   O certificado do cliente e o do servidor devem estar encadeados no mesmo certificado raiz. Esse certificado raiz deve ser escolhido nas definições de configuração do DirectAccess.  
  
### <a name="132-plan-certificates-for-ip-https"></a>1.3.2 Planejar certificados para IP-HTTPS  
O servidor do DirectAccess age como um ouvinte do IP-HTTPS, e você deve instalar manualmente um certificado de site HTTPS no servidor. Leve em conta as seguintes considerações ao planejar:  
  
-   É recomendável usar uma AC pública, para que a lista de revogação de certificados (CRLs) esteja prontamente disponível.  
  
-   No campo **Assunto**, especifique o endereço IPv4 no adaptador da Internet do servidor do DirectAccess ou FQDN da URL IP-HTTPS (o endereço ConnectTo). Se o servidor do DirectAccess encontrar-se atrás de um dispositivo NAT, o nome ou endereço público do dispositivo NAT deverá ser especificado.  
  
-   O nome comum do certificado deverá ser corresponder ao nome do site IP-HTTPS.  
  
-   No campo **Uso Avançado de Chave**, use o OID (identificador de objeto) da autenticação do servidor.  
  
-   No campo **Pontos de Distribuição da Lista de Certificados Revogados**, especifique um ponto de distribuição da CRL que seja acessível aos clientes do DirectAccess conectados à Internet.  
  
-   O certificado IP-HTTPS deve ter uma chave privada.  
  
-   O certificado IP-HTTPS deve ser importado diretamente para o repositório pessoal.  
  
-   Os certificados IP-HTTPS podem ter curingas no nome.  
  
Se você planejar usar o IP-HTTPS em uma porta não padrão, execute o procedimento a seguir no servidor do DirectAccess:  
  
1.  Remova a associação do certificado existente para 0.0.0.0:443 e substitua-a pela associação de certificado da porta desejada. Neste exemplo, usamos a porta 44500. Antes de excluir a associação do certificado, exiba e copie **appid**.  
  
    1.  Para excluir a associação do certificado, digite:  
  
        ```  
        netsh http delete ssl ipport=0.0.0.0:443  
        ```  
  
    2.  Para adicionar uma nova associação de certificado, digite:  
  
        ```  
        netsh http add ssl ipport=0.0.0.0:44500 certhash=<use the thumbprint from the DirectAccess server SSL cert> appid=<use the appid from the binding that was deleted>  
        ```  
  
2.  Para modificar a URL do IP-HTTPS no servidor, digite:  
  
    ```  
    Netsh int http set int url=https://<DirectAccess server name (for example server.contoso.com)>:44500/IPHTTPS  
    ```  
  
    ```  
    Net stop iphlpsvc & net start iphlpsvc  
    ```  
  
3.  Alterar a reserva de URL para kdcproxy.  
  
    1.  Para excluir uma reserva de URL existente, digite:  
  
        ```  
        netsh http del urlacl url=https://+:443/KdcProxy/  
        ```  
  
    2.  Para adicionar uma nova reserva de URL, digite:  
  
        ```  
        netsh http add urlacl url=https://+:44500/KdcProxy/ sddl=D:(A;;GX;;;NS)  
        ```  
  
4.  Adicionar a configuração para fazer kppsvc ouvir na porta não padrão. Para adicionar a entrada de registro, digite:  
  
    ```  
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\KPSSVC\Settings /v HttpsUrlGroup /t REG_MULTI_SZ /d +:44500 /f  
    ```  
  
5.  Para reiniciar o serviço kdcproxy no controlador de domínio, digite:  
  
    ```  
    net stop kpssvc & net start kpssvc  
    ```  
  
Para usar IP-HTTPS em uma porta não padrão, realize as etapas a seguir no controlador de domínio:  
  
1.  Modifique a configuração do IP-HTTPS no GPO do cliente.  
  
    1.  Abra o editor de políticas de grupo.  
  
    2.  Navegue para Configuração do computador =>Políticas=>Modelos Administrativos=> Rede=>Configurações de TCPIP=>Tecnologias de transição IPv6.  
  
    3.  Abra a definição de estado do IP-HTTPS e altere a URL para **https://<nome do servidor DirectAccess (por exemplo, server.contoso.com)>:44500/IPHTTPS**.  
  
    4.  Clique em **Aplicar**.  
  
2.  Modifique as configurações de cliente de proxy do Kerberos no GPO do cliente.  
  
    1.  No editor de políticas de grupo, navegue para Configuração do computador=>Políticas=>Modelos Administrativos=>Sistema=>Kerberos => Especificar servidores proxy KDC para os clientes do Kerberos.  
  
    2.  Abra a definição de estado do IPHTTPS e altere a URL para **https://<nome do servidor DirectAccess (por exemplo, server.contoso.com)>:44500/IPHTTPS**.  
  
    3.  Clique em **Aplicar**.  
  
3.  Modifique as configurações da política IPsec do cliente para usar ComputerKerb e UserKerb.  
  
    1.  No Editor de política de grupo, navegue para Configurações do computador=>Políticas=> Configurações do Windows=> Configurações de segurança=> Firewall do Windows com segurança avançada.  
  
    2.  Clique em **Regras de Segurança de Conexão** e clique duas vezes em **Regra IPsec**.  
  
    3.  Na guia **Autenticação**, clique em **Avançado**.  
  
    4.  Para Auth1: remova o método de autenticação existente e substitua por ComputerKerb. Para Auth2: remova o método de autenticação existente e substitua-por UserKerb.  
  
    5.  Clique em **Aplicar** e em **OK**.  
  
Para concluir o processo manual para usar uma porta não padrão para IP-HTTPS, execute **gpupdate /force** nos computadores cliente e no servidor do DirectAccess.  
  
### <a name="133-plan-website-certificates-for-the-network-location-server"></a>1.3.3 Planejar certificados de site para o servidor de local da rede  
Ao planejar seu site de servidor de local de rede, considere o seguinte:  
  
-   No cmapo **Assunto**, especifique o endereço IP da interface da intranet do servidor de local de rede do FQDN da URL de rede local.  
  
-   No campo **Uso Avançado de Chave**, use o OID da Autenticação do Servidor.  
  
-   No campo **Pontos de Distribuição da Lista de Certificados Revogados**, use um ponto de distribuição da CRL que seja acessível pelos clientes DirectAccess conectados à intranet. Esse ponto de distribuição da CRL não deverá ser acessível de fora da rede interna.  
  
-   Se você estiver planejando ou configurando posteriormente uma implantação em múltiplos sites ou em cluster, o nome do certificado não deverá ser igual ao nome interno de qualquer servidor do DirectAccess adicionado à implantação.  
  
    > [!NOTE]  
    > Assegure-se de que os certificados IP-HTTPS e do servidor de local de rede tenham um **Nome do Requerente**. Se o certificado não tiver um **Nome do Requerente**, mas sim um **Nome Alternativo**, ele não será aceito pelo Assistente de Acesso Remoto.  
  
## <a name="14-plan-dns-requirements"></a>1.4 Planejar os requisitos de DNS  
Esta seção explica os requisitos de DNS para as solicitações de clientes e os servidores de infraestrutura do DirectAccess em uma implantação de Acesso Remoto. Ela inclui as seguintes subseções:  
  
-   [1.4.1 planejar requisitos de servidor DNS](#141-plan-for-dns-server-requirements)  
  
-   [1.4.2 planejar a resolução de nomes locais](#142-plan-for-local-name-resolution)  
  
**Solicitações de cliente do DirectAccess**  
  
O DNS é usado para resolver solicitações de computadores cliente do DirectAccess não localizados na rede interna (ou corporativa). Os clientes do DirectAccess tentam conectar-se ao servidor de local da rede do DirectAccess para determinar se encontram-se na Internet ou na rede da intranet.  
  
-   Se a conexão for efetuada com êxito, os clientes serão identificados como localizados na rede interna, o DirectAccess não será usado e as solicitações do cliente serão resolvidas usando o servidor DNs configurado no adaptador da rede do computador cliente.  
  
-   Quando a conexão não é bem-sucedida, entende-se que os clientes encontram-se na Internet, de forma que os clientes do DirectAccess usarão o nome na NRPT (Tabela de Políticas de Resolução de Nomes) para determinar qual servidor DNS deve ser usado ao resolver solicitações de nome.  
  
Você pode especificar que os clientes devem usar o DirectAccess DNS64 para resolver nomes ou um servidor DNS interno alternativo. Ao realizar a resolução do nome, a NRPT é usada pelos clientes do DirectAccess para identificar como lidar com uma solicitação. Os clientes solicitam um FQDN ou nome de rótulo único, como <https://internal>. Se um nome de rótulo único for solicitado, um sufixo do DNs é agregado para criar um FQDN. Se a consulta DNS for correspondente a uma entrada da NRPT e o DNS64 ou um servidor DNS na rede interna for especificado para a entrada, a consulta será enviada para resolução de nome usando o servidor especificado. Se houver uma correspondência, mas nenhum servidor DNS tiver sido especificado, isso indicará uma exceção à regra, e a resolução de nome normal será aplicada.  
  
> [!NOTE]  
> Quando um novo sufixo for adicionado à NRPT no console de Gerenciamento de Acesso Remoto, os servidores DNS padrão do sufixo poderão ser descobertos automaticamente, clicando em **Detectar**.  
  
A detecção automática funciona da seguinte maneira:  
  
-   Se a rede corporativa for baseada em IPv4 ou então usar IPv4 e IPv6, o endereço padrão é o endereço do DNS64 do adaptador interno no servidor do DirectAccess.  
  
-   Se a rede corporativa for baseada em IPv6, o endereço padrão será o endereço IPv6 dos servidores DNS na rede corporativa.  
  
**Servidores de infraestrutura**  
  
-   **Servidor de local de rede**  
  
    Os clientes DirectAccess tentam acessar o servidor de local de rede para determinar se estão em uma rede interna. Os clientes na rede interna devem poder resolver o nome do servidor de local de rede, mas não deverão resolver o nome quando se encontrarem na Internet. Para garantir que isso ocorra, o FQDN do servidor do local de rede é adicionado, por padrão, a uma regra de exceção do NRPT. Quando você configurar o Acesso Remoto, as seguintes regras também serão criadas automaticamente:  
  
    -   Uma regra de sufixo de DNS para o domínio da rede ou nome de domínio do servidor do DirectAccess e os endereços IPv6 que correspondem aos endereços DNS64. Em redes corporativas somente de IPv6, os servidores DNS da intranet são configurados no servidor do DirectAccess. Por exemplo, se o servidor do DirectAccess for membro do domínio corp.contoso.com, é criada uma regra para o sufixo de DNS corp.contoso.com.  
  
    -   Uma regra de isenção para o FQDN do servidor de local de rede. Por exemplo, se a URL de servidor de local de rede é <https://nls.corp.contoso.com>, uma regra de isenção é criada para nls.corp.contoso.com do FQDN.  
  
-   **Servidor IP-HTTPS**  
  
    O servidor do DirectAccess age como ouvinte do IP-HTTPS usando um certificado de servidor para autenticar clientes IP-HTTPS. O nome do IP-HTTPS deve ser resolvível pelos clientes DirectAccess que usam servidores DNS públicos.  
  
-   **Verificação de revogação de CRL**  
  
    O DirectAccess usa a verificação de revogação de certificados para a conexão IP-HTTPS entre os clientes e o servidor do DirectAccess, e também para a conexão baseada em HTPPS entre o cliente do DirectAccess e o servidor de local de rede. Em ambos os casos, os clientes do DirectAccess devem poder resolver e acessar o local do ponto de distribuição da CRL.  
  
-   **ISATAP**  
  
    O ISATAP permite que computadores corporativos obtenham o endereço IPv6 e encapsula pacotes IPv6 com um cabeçalho IPv4. Ele é usado pelo servidor do DirectAccess para fornecer conectividade IPv6 aos hosts ISATAP na intranet. Em um ambiente de rede IPv6 não nativo, o servidor do DirectAccess configura a si mesmo automaticamente como roteador ISATAP.  
  
    Como o DirectAccess não oferece mais suporte ao ISATAP, você precisa garantir que seus servidores DNS estejam configurados para não responder a consultas ISATAP. Por padrão, o serviço Servidor DNS bloqueia a resolução de nomes para o nome ISATAP por meio da lista global de consultas não autorizadas do DNS. Não remova o nome ISATAP da lista global de consultas não autorizadas do DNS.  
  
-   **Verificadores de conectividade**  
  
    O Acesso Remoto cria uma sonda da Web padrão usada pelos computadores cliente do DirectAccess para verificar a conectividade com a rede interna. Para garantir que a sonda funcione como esperado, os nomes a seguir devem ser registrados manualmente no DNS:  
  
    -   **DirectAccess-webprobehost**-deve resolver o endereço IPv4 interno do servidor DirectAccess ou o endereço IPv6 em um ambiente somente com IPv6.  
  
    -   **DirectAccess-corpconnectivityhost**-deve resolver para o endereço do host local (loopback). Os registros de host (A) e recurso (AAAA) a seguir deverão ser criados: um registro de recurso (A) de host com valor 127.0.0.1 e outro (AAAA) com valor construído a partir do prefixo NAT64 com os últimos 32 bits como 127.0.0.1. O prefixo NAT64 pode ser recuperando ao executar o comando do Windows PowerShell **get-netnattransitionconfiguration**.  
  
        > [!NOTE]  
        > Isso é válido somente em um ambiente com IPv4 apenas. Em um ambiente de IPv4 com IPv6, ou somente de IPv6, somente o registro de recurso de host (AAAA) deve ser criado com o endereço IP do loopback ::1.  
  
    Você pode criar verificadores de conectividade adicionais usando outros endereços da Web em HTTP ou usando **ping**. Uma entrada de DNS deverá existir para cada verificador de conectividade.  
  
### <a name="141-plan-for-dns-server-requirements"></a>1.4.1 Planejar os requisitos do servidor DNS  
A seguir estão os requisitos para o DNS na implantação do DirectAccess.  
  
-   Para clientes DirectAccess, você deve usar um servidor DNS que está executando o Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008 ou qualquer outro servidor DNS que dá suporte a IPv6.  
  
    > [!NOTE]  
    > Não é recomendável que você use servidores DNS que estejam executando o Windows Server 2003, quando estiver implantando o DirectAccess. Embora os servidores DNS do Windows Server 2003 ofereçam suporte a registros IPv6, o Windows Server 2003 não tem mais o suporte da Microsoft. Além disso, você não deve implantar o DirectAccess se os controladores de domínio estiverem executando o Windows Server 2003 devido a um problema com o Serviço de replicação de arquivos. Para obter mais informações, consulte [configurações sem suporte do DirectAccess](https://technet.microsoft.com/library/dn464274.aspx).  
  
-   Use um servidor DNS com suporte a atualizações dinâmicas. Você pode usar servidores DNS que não dão suporte a atualizações dinâmicas, porém será necessário atualizar manualmente as entradas nesses servidores.  
  
-   O FQDN para os pontos de distribuição de CRL acessível pela Internet deve ser resolvível usando os servidores DNS da Internet. Por exemplo, se URL <https://crl.contoso.com/crld/corp-DC1-CA.crl> está no **pontos de distribuição da CRL** campo do certificado IP-HTTPS do servidor DirectAccess, você deve garantir que o FQDN CRL.contoso.com possa ser resolvido usando servidores DNS da Internet.  
  
### <a name="142-plan-for-local-name-resolution"></a>1.4.2 Planejar a resolução do nome local  
Ao planejar a resolução de nome local, considere as questões a seguir:  
  
**NRPT**  
  
Pode ser necessário criar regras NRPT adicionais nos seguintes casos:  
  
-   Se você precisar adicionar mais sufixos DNS ao namespace da intranet.  
  
-   Se o FQDN (nome de domínio totalmente qualificado) dos pontos de distribuição da CRL for baseado no namespace da intranet, você deverá adicionar regras de isenção para os FQDNs dos pontos de distribuição da CRL.  
  
-   Se tiver um ambiente DNS com dupla personalidade, deverá adicionar regras de isenção para os nomes dos recursos cuja versão de Internet deva ser acessada pelos clientes do DirectAccess localizados na Internet, em vez da versão da intranet.  
  
-   Se você estiver redirecionando o tráfego para um site externo através dos servidores proxy Web, o site externo estará disponível somente na intranet e usará os endereços dos seus servidores proxy Web para permitir solicitações de entrada. Nesse caso, adicione uma regra de exceção para o FQDN do site externo e especifique que a regra usa seu servidor proxy Web em vez de os endereços IPv6 dos servidores DNs da intranet.  
  
    Por exemplo, se você está testando um site externo chamado test.contoso.com, este nome não pode ser resolvido pelos servidores DNS da Internet, mas o servidor proxy Web da Contoso sabe como resolver o nome e direcionar as solicitações do site para o servidor Web externo. Para evitar que os usuários que não estiverem na intranet da Contoso acessem o site, o site externo permitirá solicitações somente do endereço da Internet IPv4 do proxy Web da Contoso. Portanto, os usuários da intranet podem acessar o site, pois estão usando o proxy Web da Contoso, mas os usuários do DirectAccess não podem, já que não estão usando esse proxy. Com a configuração de uma regra de isenção de NRPT para test.contoso.com que usar o proxy Web da Contoso, as solicitações de página da Web para test.contoso.com serão roteadas para o servidor proxy Web da intranet na Internet IPv4.  
  
**Nomes de rótulo único**  
  
Único de nomes de rótulo, tal como <https://paycheck>, às vezes, são usados para servidores da intranet. Se um nome de rótulo único for solicitado e uma lista de pesquisa de sufixos DNS estiver configurada os sufixos DNS na lista serão agregados ao nome de rótulo único. Por exemplo, quando um usuário em um computador que é um membro do domínio corp.contoso.com digitar <https://paycheck> no navegador da web, o FQDN é criado como o nome é paycheck.corp.contoso.com. Por padrão, o sufixo agregado é o sufixo de DNS primário do computador cliente.  
  
> [!NOTE]  
> Em um cenário de namespace não contíguo (em que um ou mais computadores de domínio possuem um sufixo DNS diferente do domínio do Active Directory ao qual o computador pertence), é necessário garantir que a lista de pesquisa seja personalizada para incluir todos os sufixos necessários. Por padrão, o Assistente de Acesso Remoto configurará o nome do DNS do Active Directory como o sufixo de DNS primário do cliente. Certifique-se de adicionar o sufixo de DNS usado pelos clientes para a resolução de nome.  
  
Se múltiplos domínios e WINS (Windows Internet Name Service) forem implantados na sua organização e você está se conectando remotamente, nomes únicos podem ser resolvidos da seguinte maneira:  
  
-   Implantar uma zona de pesquisa direta do WINS no DNS. Ao tentar resolver computername.dns.zone1.corp.contoso.com, a solicitação é direcionada para o servidor do WINS que está usando somente computername. O cliente pensa que está emitindo um registro de recurso de host (A) de DNS regular, mas esta é na verdade uma solicitação NetBIOS. Para saber mais, confira Gerenciando uma zona de pesquisa direta.  
  
-   Adicionar um sufixo DNS, por exemplo o dns.zone1.corp.contoso.com, ao GPO padrão de políticas de domínio.  
  
**DNS com partição de rede**  
  
O DNS com partição de rede é o uso do mesmo domínio DNS para resolução de nomes da intranet e da Internet.  
  
Para implantações de DNS com partição de rede, você deve listar os FQDNs duplicados na Internet e intranet e decidir quais recursos o cliente DirectAccess deve alcance a intranet ou a versão de Internet. Para cada nome que corresponda a um recurso cuja versão da Internet você deseja que os clientes DirectAccess acessem, é necessário adicionar o FQDN correspondente como uma regra de isenção à NRPT para os clientes DirectAccess.  
  
Em um ambiente de DNS com partição de rede, se você desejar que as duas versões do recurso estejam disponíveis, configure os recursos da intranet com nomes alternativos que não sejam duplicatas dos nomes usados na Internet e instrua seus usuários para que usem o nome alternativo quando estiverem na intranet. Por exemplo, configure o nome alternativo www.internal.contoso.com para o nome interno www.contoso.com.  
  
Em um ambiente de DNS sem partição de rede, o namespace da Internet é diferente do namespace da intranet. Por exemplo, a Contoso Corporation usa contoso.com na Internet e corp.contoso.com na intranet. Como todos os recursos da intranet usam o sufixo DNS corp.contoso.com, a regra da NRPT para corp.contoso.com roteia todas as consultas de nome DNS por recursos de intranet para servidores DNS da intranet. As consultas de nome DNS por nomes com o sufixo contoso.com não correspondem à regra de namespace corp.contoso.com da intranet na NRPT e são enviadas aos servidores DNS da Internet. Com uma implantação de DNS com partição de rede, não é necessário fazer configurações adicionais na NRPT, pois não há duplicação dos FQDNs dos recursos da Internet e da intranet. Os clientes do DirectAccess podem acessar os recursos da Internet e da intranet da organização.  
  
**Comportamento da resolução de nomes locais para clientes DirectAccess**  
  
Se um nome não pode ser resolvido com DNS para resolver o nome na sub-rede local, o serviço cliente DNS no Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows 8 e Windows 7 pode usar a resolução de nomes locais, com o nome do Link-Local Multicast R esolution (LLMNR) e o NetBIOS nos protocolos TCP/IP.  
  
Em geral, a resolução de nomes locais é necessária para a conectividade ponto a ponto, quando o computador está localizado em redes privadas, como redes domésticas com uma única sub-rede. Quando o serviço Cliente DNS executa uma resolução de nomes locais para os nomes de servidor da intranet e o computador está conectado a uma sub-rede compartilhada na Internet, usuários mal-intencionados podem capturar LLMNR e NetBIOS nas mensagens TCP/IP para determinar os nomes de servidor da intranet. Na página de DNS do Assistente de Instalação do Servidor de Infraestrutura, configure o comportamento da resolução de nomes locais de acordo com os tipos de respostas recebidas dos servidores DNS da intranet. As seguintes opções estão disponíveis:  
  
-   **Usar a resolução de nome local se o nome não existir no DNS**. Esta opção é a mais segura, pois o cliente do DirectAccess executará a resolução de nomes locais somente para os nomes de servidor que não puderem ser resolvidos pelos servidores DNS da intranet. Se os servidores DNS da intranet puderem ser acessados, os nomes dos servidores da intranet serão resolvidos. Se os servidores DNS da intranet não puderem ser acessados ou se houver outros tipos de erros de DNS, não haverá uma perda dos nomes do servidor da intranet para a sub-rede por meio da resolução de nomes locais.  
  
-   **Usar a resolução de nome local se o nome não existir no DNS ou os servidores do DNS estiverem inacessíveis quando o computador cliente está em uma rede privada (recomendado)** . Esta opção é recomendada, pois permite o uso da resolução de nomes locais em uma rede privada somente quando os servidores DNS da intranet não forem acessíveis.  
  
-   **Usar a resolução de nome local para qualquer tipo de erro de resolução do DNS (menos segura)** . Esta opção é a menos segura, pois pode haver uma perda dos nomes dos servidores da rede da intranet para a sub-rede local por meio da resolução de nomes locais.  
  
## <a name="15-plan-the-network-location-server"></a>1.5 Planejar o servidor de local de rede  
O servidor de local da rede é um site usado para detectar se os clientes DirectAccess encontram-se na rede corporativa. Clientes na rede corporativa não usam o DirectAccess para acessar recursos internos, conectando-se diretamente.  
  
O site do servidor de local de rede pode ser hospedado no servidor do DirectAccess ou em outro servidor da sua organização. Se você hospedar o servidor do local de rede no servidor do DirectAccess, o site será criado automaticamente ao instalar a função de servidor para Acesso Remoto. Se você hospedar o servidor de local de rede na sua organização executando um sistema operacional Windows, deverá garantir que o ISS (Serviços de Informações da Internet) esteja instalado neste servidor e que o site seja criado. O DirectAccess não define as configurações em um servidor de local de rede remoto.  
  
Garanta que o site do servidor de local de rede cumpra os seguintes requisitos:  
  
-   Ele é um site que possui um certificado de servidor HTTPS.  
  
-   Os computadores cliente do DirectAccess devem confiar na AC que emitiu o certificado do servidor para o site do servidor do local de rede.  
  
-   Os computadores cliente do DirectAccess na rede interna devem poder resolver o nome do site do servidor do local de rede.  
  
-   O site do servidor do local de rede deve apresentar alta disponibilidade para os computadores da rede interna.  
  
-   O servidor do local de rede não deve ser acessível aos computadores cliente do DirectAccess pela Internet.  
  
-   O certificado do servidor deverá ser verificado em comparação com uma CRL.  
  
### <a name="151-plan-certificates-for-the-network-location-server"></a>1.5.1 Planejar certificados para o servidor de local da rede  
Ao obter o certificado de site para usar com o servidor de local de rede, considere o seguinte:  
  
1.  No campo **Assunto**, especifique o endereço IP da interface da intranet do servidor de local de rede do FQDN da URL de rede local.  
  
2.  No campo **Uso Avançado de Chave**, use o OID da Autenticação do Servidor.  
  
3.  No campo **Pontos de Distribuição da Lista de Certificados Revogados**, use um ponto de distribuição da CRL que seja acessível pelos clientes DirectAccess conectados à intranet. Esse ponto de distribuição da CRL não deverá ser acessível de fora da rede interna.  
  
### <a name="152-plan-dns-for-the-network-location-server"></a>1.5.2 Planejar DNS para o servidor de local da rede  
Os clientes DirectAccess tentam acessar o servidor de local de rede para determinar se estão em uma rede interna. Os clientes na rede interna devem poder resolver o nome do servidor de local de rede, mas não deverão resolver o nome quando se encontrarem na Internet. Para garantir que isso ocorra, o FQDN do servidor do local de rede é adicionado, por padrão, a uma regra de exceção do NRPT.  
  
## <a name="16-plan-management-servers"></a>1.6 Planejar os servidores de gerenciamento  
Os clientes DirectAccess iniciam comunicações com os servidores de gerenciamento que oferecem serviços como o Windows Update e atualizações de antivírus. Os clientes do DirectAccess também usam o protocolo Kerberos para contatar os controladores de domínio para autenticação antes de acessarem a rede interna. Durante o gerenciamento dos clientes DirectAccess, os servidores de gerenciamento comunicam-se com os computadores cliente para realizar funções de gerenciamento como avaliações de inventário de software ou hardware. O Acesso Remoto pode descobrir alguns servidores de gerenciamento automaticamente, incluindo:  
  
-   Domínio controladores – detecção automática de controladores de domínio é executada para todos os domínios na mesma floresta que os computadores cliente e servidor DirectAccess.  
  
-   System Center Configuration Manager servidores-descoberta automática de servidores do System Center Configuration Manager é executada para todos os domínios na mesma floresta que os computadores cliente e servidor DirectAccess.  
  
Os controladores de domínio e os servidores do System Center Configuration Manager são automaticamente detectados na primeira fez que o DirectAccess é configurado. Controladores de domínio detectados não são exibidos no console do, mas as configurações podem ser recuperadas usando o cmdlet do Windows PowerShell **Get-DAMgmtServer – Type All**. Se os servidores do controlador de domínio ou do System Center Configuration Manager forem modificados, clicar em **Atualizar Servidores de Gerenciamento** no console de Gerenciamento de Acesso Remoto atualizará a lista de servidores de gerenciamento.  
  
**Requisitos do servidor de gerenciamento**  
  
-   Os servidores de gerenciamento devem ser acessíveis por meio do primeiro túnel (infraestrutura). Quando você configurar o Acesso Remoto, os servidores adicionados à lista de servidores de gerenciamento poderão ser automaticamente acessados por esse túnel.  
  
-   Os servidores de gerenciamento que iniciam conexões para os clientes do DirectAccess devem dar suporte completo ao IPv6, com um endereço IPv6 nativo ou usando um atribuído por ISATAP.  
  
## <a name="17-plan-active-directory-domain-services"></a>1.7 Planejar Serviços de Domínio Active Directory  
Esta seção explica como o DirectAccess usa o AD DS (Serviços de Domínio Active Directory) e inclui as seguintes subseções:  
  
-   [1.7.1 planejar a autenticação de cliente](#171-plan-client-authentication)  
  
-   [1.7.2 planejar domínios múltiplos](#172-plan-multiple-domains)  
  
O DirectAccess usa objetos de política do AD DS e o grupo do Active Directory (GPOs) da seguinte maneira:  
  
-   **Autenticação**  
  
    O AD DS é usado para autenticação. O túnel de infraestrutura usa a autenticação NTLMv2 para a conta do computador conectada ao servidor do DirectAccess, conta esta que deve estar listada em um domínio do Active Directory. O túnel da intranet usa a autenticação Kerberos para que o usuário crie o segundo túnel.  
  
-   **Objetos de diretiva de grupo**  
  
    O DirectAccess reúne as definições de configurações nos GPOs que são aplicadas aos servidores, clientes e servidores de aplicativo internos do DirectAccess.  
  
-   **Grupos de segurança**  
  
    O DirectAccess usa grupos de segurança para coletar e identificar os computadores cliente do DirectAccess. Os GPOs são aplicados ao grupo de segurança necessário.  
  
-   **Políticas IPsec estendidas**  
  
    O DirectAccess pode usar a autenticação e criptografia IPsec entre os clientes e o servidor do DirectAccess. Você pode estender a autenticação e criptografia do cliente para os servidores internos de aplicativo especificados. Para tal, adicione os servidores de aplicativo desejados a um grupo de segurança.  
  
**Requisitos do AD DS**  
  
Ao planejar o AD DS para uma implantação de DirectAccess, considere os requisitos a seguir:  
  
-   Pelo menos um controlador de domínio deve ser instalado com o sistema operacional Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008.  
  
    Se o controlador de domínio estiver em uma rede de perímetro (estando, portanto, acessível do adaptador de rede voltado para a Internet do servidor do DirectAccess), será necessário evitar que o servidor do DirectAccess acesse-o adicionando filtros de pacote no controlador de domínio, impedindo a conectividade com o endereço IP do adaptador de Internet.  
  
-   O servidor do DirectAccess deve ser um membro do domínio.  
  
-   Os clientes do DirectAccess devem ser membros do domínio. Os clientes podem pertencer a:  
  
    -   Qualquer domínio na mesma floresta que o servidor do DirectAccess.  
  
    -   Qualquer domínio que tenha uma relação de confiança bidirecional com o domínio do servidor do DirectAccess.  
  
    -   Qualquer domínio em uma floresta que tenha uma relação de confiança bidirecional com a floresta à qual o domínio do DirectAccess pertence.  
  
> [!NOTE]  
> -   O servidor DirectAccess não pode ser um controlador de domínio.  
> -   O controlador de domínio do AD DS usado para o DirectAccess não pode estar acessível do adaptador externo da Internet do servidor do DirectAccess (isto é, o adaptador não deverestar no perfil de domínio no Firewall do Windows).  
  
### <a name="171-plan-client-authentication"></a>1.7.1 Planejar a autenticação do cliente  
O DirectAccess permite escolher entre usar certificados para autenticação IPsec de computadores ou usar um proxy Kerberos integrado que é autenticado com nomes de usuário e senhas.  
  
Ao escolher usar credenciais Ad DS para autenticação, o DirectAccess usa um túnel de segurança que usa o Kerberos de computador para a primeira autenticação e o Kerberos de usuário para a segunda. Com esse modo de autenticação, o DirectAccess usa um único túnel de segurança que fornece acesso ao servidor DNS, ao controlador de domínio e a outros servidores na rede interna.  
  
Com a autenticação de dois fatores ou a Proteção de Acesso à Rede, o DirectAccess usa dois túneis de segurança. O Assistente de Instalação de Acesso Remoto configura as regras de segurança de conexão do Firewall do Windows com Segurança Avançada que especificam o uso dos seguintes tipos de credenciais durante a negociação das associações de segurança do IPsec para os túneis do servidor do DirectAccess:  
  
-   O túnel de infraestrutura usa as credenciais do Kerberos de computador na primeira autenticação e as do Kerberos de usuário para a segunda.  
  
-   O túnel de intranet usa as credenciais de certificado do computador para a primeira autenticação e as do Kerberos de usuário para a segunda.  
  
Quando o DirectAccess escolher permitir o acesso aos clientes que executam o Windows 7 ou em uma implantação multissite, ele usa dois túneis de segurança. O Assistente de Instalação de Acesso Remoto configura as regras de segurança de conexão do Firewall do Windows com Segurança Avançada que especificam o uso dos seguintes tipos de credenciais durante a negociação das associações de segurança do IPsec para os túneis do servidor do DirectAccess:  
  
-   O túnel de infraestrutura usa as credenciais de certificado do computador para a primeira autenticação e as de NTLMv2 para a segunda. As credenciais NTLMv2 forçam o uso do AuthIP (protocolo Authenticated IP) e dão acesso a um servidor DNS e a um controlador de domínio antes que o cliente do DirectAccess possa usar as credenciais Kerberos para o túnel da intranet.  
  
-   O túnel de intranet usa as credenciais de certificado do computador para a primeira autenticação e as do Kerberos de usuário para a segunda.  
  
### <a name="172-plan-multiple-domains"></a>1.7.2 Planejar domínios múltiplos  
A lista de servidores de gerenciamento deverá incluir controladores de domínio de todos os domínios que contém grupos de segurança que incluem computadores cliente do DirectAccess. Ela deve conter todos os domínios que possuem contas de usuário que podem usar computadores configurados como clientes do DirectAccess. Isso garante que todos os usuários não localizados no mesmo domínio que o computador cliente usado sejam autenticados com um controlador de domínio no domínio do usuário. Isso é ocorre automaticamente se o domínio estiver na mesma floresta.  
  
> [!NOTE]  
> Se houver computadores nos grupos de segurança usados para computadores cliente ou servidores de aplicativo em diferentes florestas, os controladores de domínio de tais florestas não são detectados automaticamente. Você pode executar a tarefa **Atualizar Servidores de Gerenciamento** no console de Gerenciamento de Acesso Remoto para detectar esses controladores de domínio.  
  
Quando possível, sufixos comuns de nome de domínio devem ser adicionados à NRPT (Tabela de Políticas de Resolução de Nomes) durante a implantação do Acesso Remoto. Por exemplo, se houvesse dois domínios, domain1.corp.contoso.com e domain2.corp.contoso.com, em vez de adicionar duas entradas na NRPT, você poderia adicionar uma entrada de sufixo de DNS comum, cujo sufixo de nome de domínio seria corp.contoso.com. Isso ocorre automaticamente para domínios da mesma raiz, mas aqueles que não estão na mesma raiz devem ser adicionados manualmente.  
  
Se o WINS (Serviço de Cadastramento na Internet do Windows) for implantado em um ambiente de múltiplos domínios, você deverá implantar uma zona de pesquisa direta no DNS. Para obter mais informações, consulte **único de nomes de rótulo** na [1.4.2 planejar a resolução de nomes locais](#142-plan-for-local-name-resolution) seção no início deste documento.  
  
## <a name="18-plan-group-policy-objects"></a>1.8 Planejar objetos de política de grupo  
Esta seção explica a função dos GPOs (Objetos de Política de Grupo) na sua infraestrutura de Acesso Remoto e inclui as seguintes subseções:  
  
-   [1.8.1 Configurar GPOs criados automaticamente](#181-configure-automatically-created-gpos)  
  
-   [1.8.2 Configurar GPOs criados manualmente](#182-configure-manually-created-gpos)  
  
-   [1.8.3 gerenciar GPOs em um ambiente de controlador de múltiplos domínios](#183-manage-gpos-in-a-multi-domain-controller-environment)  
  
-   [1.8.4 gerenciar GPOs de acesso remoto com permissões limitadas](#184-manage-remote-access-gpos-with-limited-permissions)  
  
-   [1.8.5 recuperação de um GPO excluído](#185-recover-from-a-deleted-gpo)  
  
As definições do DirectAccess ajustadas com a configuração do Acesso Remoto são coletadas nos GPOs. Os tipos de GPOs a seguir são populados com as configurações do DirectAccess, e são distribuídas da seguinte forma:  
  
-   **GPO do cliente do DirectAccess**  
  
    Este GPO contém configurações do cliente, inclusive configurações da tecnologia de transição IPv6, entradas da NRPT e regras de segurança de conexão do Firewall do Windows com Segurança Avançada. O GPO é aplicado a grupos de segurança específicos para os computadores cliente.  
  
-   **GPO do servidor DirectAccess**  
  
    Este GPO contém definições de configurações do DirectAccess aplicadas a qualquer servidor configurado como servidor do DirectAccess na implantação. Ele contém também as regras de segurança de conexão do Firewall do Windows com Segurança Avançada.  
  
-   **GPO de servidores de aplicativo**  
  
    Este GPO contém configurações para servidores de aplicativos específicos, para os quais são estendidos a autenticação e criptografia dos clientes do DirectAccess. Se a autenticação e a criptografia não forem estendidas, este GPO não será usado.  
  
Os GPOs podem ser configurados de duas maneiras:  
  
-   **Automaticamente**-você pode especificar que eles são criados automaticamente. Um nome padrão é especificado para cada GPO.  
  
-   **Manualmente**-você pode usar GPOs predefinidos pelo administrador do Active Directory.  
  
> [!NOTE]  
> Depois de o DirectAccess ser configurado para usar GPOs específicos, não poderá ser configurado para usar GPOs diferentes.  
  
Seja em GPOs configurados automática ou manualmente, será necessário adicionar uma política para detecção de links lentos se os clientes usarem redes 3G. O caminho para **política: Configurar a detecção de vínculo lento de diretiva de grupo** é: **Configuração do Computador / Políticas / Modelos Administrativos / Sistema / Política de Grupo**.  
  
> [!CAUTION]  
> Use o procedimento a seguir para fazer o backup de todos os GPOs de Acesso Remoto antes de executar cmdlets do DirectAccess: [Fazer backup e restaurar a configuração de acesso remoto](https://go.microsoft.com/fwlink/?LinkID=257928).  
  
Se as permissões corretas (listadas nas seções a seguir) para vinculação de GPOs não existirem, um aviso é mostrado. A operação de Acesso Remoto continuará, porém não ocorrerá a vinculação. Se o aviso for emitido, não será possível criar links automaticamente, mesmo se as permissões forem adicionadas posteriormente. Em vez disso, o administrador precisará criar os links manualmente.  
  
### <a name="181-configure-automatically-created-gpos"></a>1.8.1 Configurar GPOs criados automaticamente  
Leve em conta as considerações a seguir ao usar GPOs criados automaticamente.  
  
GPOs criados automaticamente são aplicados de acordo com o local e o parâmetro de destino do link, da seguinte maneira:  
  
-   Para os GPOs de servidor do DirectAccess, o parâmetro de local e o parâmetro de link apontam para o domínio que contém o servidor do DirectAccess.  
  
-   Quando os GPOs de cliente e do servidor de aplicativos são criados, o local é definido para um único domínio no qual o GPO será criado. O nome do GPO é pesquisado em cada domínio e é preenchido nas configurações do DirectAccess, se existir. O destino do link é definido para a raiz do domínio no qual o GPO foi criado. Um GPO é criado para cada domínio que contém computadores cliente ou servidores de aplicativo, e o GPO é vinculado à raiz dos seus respectivos domínios.  
  
Ao usar GPOs criados automaticamente para aplicação das configurações do DirectAccess, o administrador de Acesso Remoto precisará das seguintes permissões:  
  
-   Permissões de criação de GPO em cada domínio  
  
-   Permissões de links para todas as raízes de domínio de cliente escolhidas  
  
-   Permissões de links para as raízes de domínio de GPO do servidor  
  
Além dessas, são necessárias as seguintes permissões:  
  
-   As permissões de segurança para criar, editar, excluir e modificar são necessárias para os GPOs.  
  
-   Recomendamos que o administrador de Acesso Remoto possua permissões de leitura de GPO para cada domínio necessário. Isso permite que o Acesso Remoto confirme que não existam GPOs com nomes duplicados ao criá-los.  
  
### <a name="182-configure-manually-created-gpos"></a>1.8.2 Configurar GPOs criados manualmente  
Leve em conta as seguintes considerações ao usar GPOs criados manualmente:  
  
-   Os GPOs devem existir antes de o Assistente de Instalação de Acesso Remoto ser executado.  
  
-   Para aplicar as configurações do DirectAccess, o administrador do Acesso Remoto precisará de permissões completas de GPO (permissões de segurança para editar, excluir e modificar) para os GPOs criados manualmente.  
  
-   Uma pesquisa é feita em todo o domínio em busca de um link para o GPO. Se o GPO não estiver vinculado ao domínio, um link é criado automaticamente na raiz do domínio. Se as permissões necessárias para criar o link não estiverem disponíveis, será emitido um aviso.  
  
### <a name="183-manage-gpos-in-a-multi-domain-controller-environment"></a>1.8.3 Gerenciar GPOs em um ambiente de controlador de múltiplos domínios  
Cada GPO é gerenciado por um controlador de domínio específico, como mostrado a seguir:  
  
-   O GPO de servidor é gerenciado por um dos controladores de domínio no site do Active Directory associado ao servidor. Se os controladores de domínio no site forem somente de leitura, o GPO do servidor será gerenciado pelo controlador de domínio habilitado para gravação mais próximo do servidor do DirectAccess.  
  
-   Os GPOs de cliente e servidor de aplicativos são gerenciados pelo controlador de domínio executado como controlador de domínio primário.  
  
Se você desejar modificar as configurações de GPO, considere o seguinte:  
  
-   No GPO de servidor, para identificar qual controlador de domínio está associado ao servidor do DirectAccess, execute **nltest /dsgetdc: /writable** em um prompt de comandos com privilégios elevados no servidor do DirectAccess.  
  
-   Por padrão, ao fazer alterações com cmdlets em rede do Windows PowerShell ou pelo Console de Gerenciamento de Política de Grupo, o controlador de domínio que estiver agindo como o controlador de domínio primário será usado.  
  
Se você modificar as configurações em um controlador de domínio que não estiver associado ao servidor do DirectAccess (para o GPO de servidor) ou ao controlador de domínio primário (para os GPOs de cliente e servidor de aplicativos), considere também o seguinte:  
  
-   Antes de modificar as configurações, certifique-se de que o controlador de domínio seja replicado com um GPO atualizado e faça backup das suas configurações de GPO. Para obter mais informações, consulte [fazer backup e restaurar a configuração de acesso remoto](https://go.microsoft.com/fwlink/?LinkID=257928). Se o GPO não estiver atualizado, poderão ocorrer conflitos de mesclagem durante a replicação, o que pode corromper a configuração do Acesso Remoto.  
  
-   Depois de modificar as configurações, você deve aguardar as alterações serem replicadas para os controladores de domínio associados aos GPOs. Não faça outras alterações usando o console de Gerenciamento de Acesso Remoto ou cmdlets do PowerShell de Acesso Remoto até que a replicação tenha sido concluída. Se um GPO for editado em dois controladores de domínio antes da conclusão da replicação, poderão ocorrer conflitos de mesclagem, o que pode corromper as configurações do Acesso Remoto.  
  
Outra opção é alterar as configurações padrão usando a caixa de diálogo **Alterar Controlador de Domínio** no Console de Gerenciamento de Política de Grupo ou o cmdlet **Open-NetGPO** do Windows PowerShell, para que as alterações usem o controlador de domínio que você especificar.  
  
-   Para fazer isso no Console de Gerenciamento de Política de Grupo, clique com o botão direito no domínio ou contêiner dos sites e clique em **Alterar Controlador de Domínio**.  
  
-   Para fazer isso no Windows PowerShell, especifique o parâmetro **DomainController** para o cmdlet **Open-NetGPO**. Por exemplo, para habilitar os perfis privados e públicos no Firewall do Windows em um GPO chamado domain1\DA_Server_GPO _Europe usando um controlador de domínio chamado europe-dc.corp.contoso.com, digite o seguinte:  
  
    ```powershell
    $gpoSession = Open-NetGPO -PolicyStore "domain1\DA_Server_GPO _Europe" -DomainController "europe-dc.corp.contoso.com"  
    Set-NetFirewallProfile -GpoSession $gpoSession -Name @("Private","Public") -Enabled True  
    Save-NetGPO -GpoSession $gpoSession  
    ```  
  
### <a name="184-manage-remote-access-gpos-with-limited-permissions"></a>1.8.4 Gerenciar GPOs de Acesso Remoto com permissões limitadas  
Para gerenciar uma implantação de Acesso Remoto, o administrador do Acesso Remoto precisará de permissões completas de GPO (permissões de segurança para editar, excluir e modificar) para os GPOs usados na implantação. Isso é necessário porque o console de Gerenciamento de Acesso Remoto e os módulos de Acesso Remoto do PowerShell leem e gravam a configuração nos GPOs de Acesso Remoto (isto é, GPOs de cliente, servidor e servidor de aplicativos).  
  
Em muitas organizações, o administrador do domínio, encarregado das operações do GPO não é a mesma pessoa que o administrador de Acesso Remoto, encarregado da configuração do Acesso Remoto. Essas organizações podem ter políticas que impeçam que o administrador de Acesso Remoto tenha permissões completas sobre os GPOs no domínio. Também pode ser necessário que o administrador de domínio revise a configuração de políticas antes de aplicá-la a qualquer computador do domínio.  
  
Para acomodar tais requisitos, o administrador do domínio deverá criar duas cópias de cada GPO: de preparo e de produção. O administrador de Acesso Remoto receberá permissões completas sobre os GPOs de preparo. O administrador de Acesso Remoto especifica os GPOs de preparo no console de Gerenciamento de Acesso Remoto e nos cmdlets do Windows PowerShell como os GPOs usados para a implantação do Acesso Remoto. Isso permite que o administrador de Acesso Remoto leia e modifique a configuração de Acesso Remoto conforme necessário.  
  
O administrador do domínio deverá assegurar que os GPOs de preparo não estejam vinculados a nenhum escopo de gerenciamento no domínio e que o administrador de Acesso Remoto não possua permissões de vinculação de GPO no domínio. Isso garante que as alterações realizadas pelo administrador de Acesso Remoto nos GPOs de preparo não entrem em vigor nos computadores do domínio.  
  
O administrador do domínio vincula os GPOs em produção ao escopo de gerenciamento adequado e aplica as devidas filtragens de segurança. Isso garante que as alterações nesses GPOs sejam aplicadas aos computadores no domínio (computadores cliente, servidores do DirectAccess e servidores de aplicativos). O administrador de Acesso Remoto não receberá nenhuma permissão sobre os GPOs de produção.  
  
Quando são realizadas alterações nos GPOs de preparação, o administrador de domínio pode revisar a configuração da política nesses GPOs para assegurar que esteja de acordo com os requisitos de segurança da organização. O administrador do domínio pode então exportar as configurações dos GPOs em preparação usando o recurso de backup, importando depois as configurações para os GPOs de produção correspondente, que serão aplicados então aos computadores do domínio.  
  
O diagrama a seguir mostra esta configuração.  
  
![Gerenciar GPOs de acesso remoto](../../../media/Step-1-Plan-the-DirectAccess-Infrastructure/DA_Plan_Advanced_Step1_GPOS.png)  
  
### <a name="185-recover-from-a-deleted-gpo"></a>1.8.5 recuperação de um GPO excluído  
Se um GPO de cliente, do servidor do DirectAccess ou do servidor de aplicativos for excluído acidentalmente e não houver backup disponível, será necessário remover as definições de configuração e reconfigurá-las. Se um backup estiver disponível, você poderá usá-lo para restaurar o GPO.  
  
O console de Gerenciamento de Acesso Remoto exibirá a seguinte mensagem de erro: **Não é possível encontrar o GPO (nome do GPO)** . Para remover as definições de configuração, siga essas etapas:  
  
1.  Execute o cmdlet **Uninstall-remoteaccess** do Windows PowerShell.  
  
2.  Abra o console de Gerenciamento de Acesso Remoto.  
  
3.  Você verá uma mensagem de erro indicando que o GPO não foi encontrado. Clique em **Remover definições de configuração**. Depois de concluído, o servidor será restaurado para um estado não configurado.  
  
## <a name="next-steps"></a>Próximas etapas  
  
-   [Etapa 2: Planejar as implantações do DirectAccess](da-adv-plan-s2-deployments.md)  
  


