---
title: Etapa 1 configurar a infraestrutura avançada do DirectAccess
description: Este tópico faz parte do guia implantar um único servidor DirectAccess com as configurações avançadas do Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 43abc30a-300d-4752-b845-10a6b9f32244
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d90005faeb16d81294b498c53cb5119b855f5294
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86959288"
---
# <a name="step-1-configure-advanced-directaccess-infrastructure"></a>Etapa 1 configurar a infraestrutura avançada do DirectAccess

>Aplica-se a: Windows Server 2012 R2, Windows Server 2012

Este tópico descreve como configurar a infraestrutura necessária a uma implantação avançada do Acesso Remoto que utiliza um servidor individual do DirectAccess em um ambiente misto de IPv4 e IPv6. Antes de começar as etapas de implantação, verifique se você concluiu as etapas de planejamento descritas em [planejar uma implantação avançada do DirectAccess](../../../remote-access/directaccess/single-server-advanced/Plan-an-Advanced-DirectAccess-Deployment.md).  
  
|Tarefa|Descrição|  
|----|--------|  
|1.1 Definir as configurações de rede do servidor|Definir as configurações de rede do servidor do DirectAccess.|  
|1.2 Configurar a criação de túneis à força|Configurar a criação de túneis à força.|  
|1.3 Configurar o roteamento na rede corporativa|Configurar o roteamento na rede corporativa.|  
|1.4 Configurar firewalls|Configurar firewalls adicionais, se necessário.|  
|1.5 Configurar ACs e certificados|Configurar uma AC (autoridade de certificação), se necessário, e qualquer outro modelo de certificado pedido pela implantação.|  
|1.6 Configurar o servidor DNS|Definir as configurações de DNS (Sistema de Nomes de Domínio) do servidor do DirectAccess.|  
|1.7 Configurar o Active Directory|Unir computadores cliente e o servidor do DirectAccess no domínio do Active Directory.|  
|1.8 Configurar GPOs|Configurar os GPOs para implantação, se necessário.|  
|1.9 Configurar grupos de segurança|Configurar os grupos de segurança que conterão os computadores cliente do DirectAccess, além de qualquer outro grupo de segurança pedido pela implantação.|  
|1.10 Configurar o servidor de local de rede|Configurar o servidor de local de rede, inclusive instalar o certificado do site desse servidor.|  
  
> [!NOTE]  
> Este tópico inclui cmdlets do Windows PowerShell de exemplo que podem ser usados para automatizar alguns dos procedimentos descritos. Para obter mais informações, confira [Usando os Cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="11-configure-server-network-settings"></a><a name="ConfigNetworkSettings"></a>1.1 Definir as configurações de rede do servidor  
As seguintes configurações de interface de rede são necessárias a uma implantação de servidor único em um ambiente que usa IPv4 e IPv6. Todos os endereços IP são configurados usando **Alterar configurações do adaptador** na **Central de Rede e Compartilhamento do Windows**.  
  
**Topologia de borda**  
  
-   Dois endereços IPv4 ou IPv6 estáticos, públicos e consecutivos voltados à Internet.  
  
    > [!NOTE]  
    > Dois endereços públicos são necessários para o Teredo. Se não estiver usando o Teredo, você poderá configurar um endereço IPv4 estático público individual.  
  
-   Um endereço IPv4 ou IPv6 estático, interno e individual  
  
**Atrás de um dispositivo NAT (conversão de endereços de rede) com dois adaptadores de rede**  
  
-   Um endereço IPv4 ou IPv6 individual e estático, voltado à Internet  
  
-   Um endereço IPv4 ou IPv6 estático, interno e individual, voltado à rede  
  
**Atrás de um dispositivo NAT com um adaptador de rede**  
  
-   Um endereço IPv4 ou IPv6 estático, interno e individual, voltado à rede  
  
> [!NOTE]  
> Se um servidor do DirectAccess com dois um mais adaptadores de rede (um classificado no perfil de domínio e, o outro, em um perfil público ou privado) for configurado com uma topologia de adaptador de rede único, será recomendável o seguinte:  
>   
> -   Assegurar que o segundo adaptador de rede e todos os outros adaptadores adicionais estejam classificados no perfil de domínio.  
> -   Se o segundo adaptador de rede não puder ser configurado para o perfil de domínio, a política IPsec do DirectAccess deverá ser manualmente definida como escopo para todos os perfis usando o seguinte comando do Windows PowerShell após a configuração do DirectAccess:  
>   
>     ```  
>     $gposession = Open-NetGPO "PolicyStore <Name of the server GPO>  
>     Set-NetIPsecRule "DisplayName <Name of the IPsec policy> "GPOSession $gposession "Profile Any  
>     Save-NetGPO "GPOSession $gposession  
>     ```  
  
## <a name="12-configure-force-tunneling"></a><a name="BKMK_forcetunnel"></a>1.2 Configurar a criação de túneis à força  
A criação de túneis à força pode ser configurada por meio do Assistente de Configuração do Acesso Remoto. Ela é apresentada como uma caixa de seleção no Assistente de Configuração de Clientes Remotos. Essa configuração afeta apenas os clientes do DirectAccess. Se a VPN (rede virtual privada) estiver habilitada, os clientes da VPN usarão, por padrão, a criação de túneis à força. Os administradores podem alterar a configuração dos clientes da VPN no perfil do cliente.  
  
Marcar a caixa de seleção de criação de túneis à força resulta no seguinte:  
  
-   Habilitação da criação de túneis à força nos clientes do DirectAccess  
  
-   Adição de uma entrada **Any** à NRPT (Tabela de Políticas de Resolução de Nomes) dos clientes do DirectAccess, o que significa que todo o tráfego de DNS irá para os servidores DNS da rede interna  
  
-   Configuração dos clientes do DirectAccess para que sempre usem a tecnologia de transição IP-HTTPS  
  
Para disponibilizar os recursos da Internet aos clientes do DirectAccess que usam a criação de túneis à força, você pode usar um servidor proxy, que pode receber solicitações baseadas em IPv6 para os recursos da Internet e convertê-las em solicitações para recursos da Internet baseados em IPv4. Para configurar um servidor proxy para os recursos da Internet, você precisa modificar a entrada padrão na NRPT a fim de adicionar o servidor proxy. Você pode fazer isso usando os cmdlets de Acesso Remoto do PowerShell ou os cmdlets de DNS do PowerShell. Por exemplo, use o cmdlet de Acesso Remoto do PowerShell da seguinte forma:  
  
```  
Set-DAClientDNSConfiguration "DNSSuffix "." "ProxyServer <Name of the proxy server:port>  
```  
  
> [!NOTE]  
> Se o DirectAccess e a VPN estiverem habilitados no mesmo servidor, e se a VPN estiver em modo de criação de túneis à força e o servidor estiver implantando em uma topologia de borda ou atrás de uma NAT (com dois adaptadores de rede, um conectado ao domínio e outro a uma rede privada), o tráfego de VPN da Internet não poderá ser encaminhado por meio da interface externa do servidor do DirectAccess. Para habilitar esse cenário, as organizações devem implantar o Acesso Remoto no servidor protegido por um firewall em uma topologia de adaptador de rede individual. Como alternativa, as organizações podem usar um servidor proxy separado na rede interna para encaminhar o tráfego da Internet dos clientes da VPN.  
  
> [!NOTE]  
> Se uma organização estiver usando um proxy Web para que os clientes do DirectAccess acessem os recursos da Internet, e o proxy corporativo não conseguir lidar com os recursos da rede interna, os clientes do DirectAccess não poderão acessar os recursos internos se eles estiverem fora da intranet. Nesse cenário, para permitir que os clientes do DirectAccess acessem os recursos internos, crie manualmente entradas da NRPT para os sufixos da rede interna usando a página DNS do assistente de infraestrutura. Não aplique as configurações de proxy a esses sufixos da NRPT. Os sufixos devem ser preenchidos com as entradas padrão do servidor DNS.  
  
## <a name="13-configure-routing-in-the-corporate-network"></a><a name="ConfigRouting"></a>1.3 Configurar o roteamento na rede corporativa  
Configure o roteamento na rede corporativa da seguinte forma:  
  
-   Quando o IPv6 nativo for implantado na organização, adicione uma rota para que os roteadores da rede interna roteiem o tráfego IPv6 de volta por meio do servidor do DirectAccess.  
  
-   Configure manualmente as rotas IPv4 e IPv6 da organização nos servidores DirectAccess. Adicione uma rota publicada para que todo o tráfego com um prefixo IPv6 (/48) da organização seja encaminhado à rede interna. Para o tráfego IPv4, adicione rotas explícitas para que ele seja encaminhado à rede interna.  
  
## <a name="14-configure-firewalls"></a><a name="ConfigFirewalls"></a>1.4 Configurar firewalls  
Ao usar firewalls adicionais na sua implantação, aplique as seguintes exceções de firewall voltadas à Internet para o tráfego do Acesso Remoto quando o servidor do DirectAccess estiver na Internet IPv4:  
  
-   Tráfego de Teredo "porta de destino UDP (User Datagram Protocol) 3544 de entrada e porta de origem UDP 3544 de saída.  
  
-   tráfego 6to4 "IP Protocol 41 de entrada e saída.  
  
-   IP-HTTPS "porta de destino TCP (protocolo de controle de transmissão) 443 e saída da porta de origem TCP 443. Quando o servidor do DirectAccess tiver um só adaptador de rede e o servidor de local de rede estiver no servidor DirectAccess, a porta TCP 62000 também será necessária.  
  
    > [!NOTE]  
    > Essa isenção deve ser configurada no servidor do DirectAccess, enquanto todas as outras isenções devem ser configuradas no firewall de borda.  
  
> [!NOTE]  
> Para tráfego Teredo e 6to4, essas exceções devem ser aplicadas para os endereços IPv4 públicos consecutivos voltados para a Internet no servidor do DirectAccess. Para IP-HTTPS, as exceções precisam ser aplicadas apenas para o endereço no qual o nome do servidor público é resolvido.  
  
Ao usar firewalls adicionais, aplique as seguintes exceções de firewall voltadas à Internet para o tráfego do Acesso Remoto quando o servidor do DirectAccess estiver na Internet IPv6:  
  
-   Protocolo IP 50  
  
-   Entrada da porta de destino UDP 500 e saída da porta de origem UDP 500.  
  
-   Entrada e saída de tráfego do protocolo de mensagens de controle da Internet para IPv6 (ICMPv6) para implementações de Teredo somente.  
  
Ao usar firewalls adicionais, aplique as seguintes exceções do firewall de rede interna no tráfego de Acesso Remoto:  
  
-   ISATAP "protocolo 41 de entrada e saída  
  
-   TCP/UDP para todo o tráfego IPv4/IPv6  
  
-   ICMP para todo o tráfego IPv4/IPv6  
  
## <a name="15-configure-cas-and-certificates"></a><a name="ConfigCAs"></a>1.5 Configurar ACs e certificados  
O acesso remoto no Windows Server 2012 permite que você escolha entre usar certificados para autenticação do computador ou usar um proxy Kerberos interno que se autentica usando nomes de usuário e senhas. Você também deve configurar um certificado IP-HTTPS no servidor do DirectAccess.  
  
Para obter mais informações, consulte [Active Directory serviços de certificados](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770357(v=ws.10)).  
  
### <a name="151-configure-ipsec-authentication"></a>1.5.1 Configurar a autenticação IPsec  
Um certificado de computador é pedido no servidor do DirectAccess e em todos os clientes do DirectAccess para o uso da autenticação IPsec. O certificado deve ser emitido por uma AC interna, e os servidores e clientes do DirectAccess devem confiar na cadeia da autoridade de certificação que emite os certificados raiz e intermediários.  
  
##### <a name="to-configure-ipsec-authentication"></a>Para configurar a autenticação IPsec  
  
1.  Na AC interna, decida se usará o modelo de certificado **Computador** ou se criará um modelo de certificado conforme descrito em [Creating Certificate Templates (Criando modelos de certificado)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731705(v=ws.10)).  
  
    > [!NOTE]  
    > Se você criar um novo modelo, ele deverá ser configurado para a autenticação de cliente.  
  
2.  Implante o modelo de certificado, se necessário. Para obter mais informações, consulte [Deploying Certificate Templates (Implantando modelos de certificado)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770794(v=ws.10)).  
  
3.  Configure o modelo de certificado para o registro automático, se necessário. Para obter mais informações, consulte [Configurar o registro automático de certificado](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731522(v=ws.11)).  
  
### <a name="152-configure-certificate-templates"></a><a name="ConfigCertTemp"></a>1.5.2 Configurar modelos de certificado  
Ao usar uma AC interna para emitir certificados, é necessário configurar um modelo de certificado para o certificado IP-HTTPS e o certificado de site do servidor de local de rede.  
  
##### <a name="to-configure-a-certificate-template"></a>Para configurar um modelo de certificado  
  
1.  Na AC interna, crie um modelo de certificado conforme descrito em [Creating Certificate Templates (Criando modelos de certificado)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731705(v=ws.10)).  
  
2.  Implante o modelo de certificado conforme descrito em [Deploying Certificate Templates (Implantando modelos de certificado)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770794(v=ws.10)).  
  
### <a name="153-configure-the-ip-https-certificate"></a>1.5.3 Configurar o certificado IP-HTTPS  
O Acesso Remoto pede um certificado IP-HTTPS para autenticar as conexões IP-HTTPS com o servidor do DirectAccess. Existem três opções de certificado disponíveis para a autenticação IP-HTTPS:  
  
**Certificado público**  
  
Um certificado público é fornecido por terceiros. Se o nome da entidade do certificado não contiver caracteres curinga, ele deverá ser a URL de um FQDN (nome de domínio totalmente qualificado) que possa ser resolvido externamente, usada apenas para as conexões IP-HTTPS do servidor do DirectAccess.  
  
**Certificado privado**  
  
Se você usar um certificado privado, serão pedidos os seguintes, caso ainda não existam:  
  
-   Um certificado de site usado para a autenticação IP-HTTPS. A entidade do certificado deve ser um FQDN externamente resolvível que pode ser obtido da Internet. O certificado é baseado no modelo de certificado que você criou seguindo as instruções em 1.5.2 configurar modelos de certificado.  
  
-   Um ponto de distribuição da lista de revogação de certificados (CRL) que está acessível por um FQDN resolvível publicamente.  
  
**Certificado autoassinado**  
  
Se você usar um certificado autoassinado, serão pedidos os seguintes, caso ainda não existam:  
  
-   Um certificado de site usado para a autenticação IP-HTTPS. A entidade do certificado deve ser um FQDN externamente resolvível que pode ser obtido da Internet.  
  
-   Um ponto de distribuição de CRL acessível a um FQDN que possa ser resolvido publicamente.  
  
> [!NOTE]  
> Certificados autoassinados não podem ser usados em implantações multissite.  
  
Verifique se o certificado de site usado para a autenticação IP-HTTPS atenda aos seguintes requisitos:  
  
-   O nome comum do certificado deverá ser corresponder ao nome do site IP-HTTPS.  
  
-   No campo **Entidade**, especifique o FQDN da URL IP-HTTPS.  
  
-   No campo **Uso Avançado de Chave**, use o OID (identificador de objeto) da autenticação do servidor.  
  
-   No campo **Pontos de Distribuição da Lista de Certificados Revogados**, especifique um ponto de distribuição da CRL que seja acessível aos clientes do DirectAccess conectados à Internet.  
  
-   O certificado IP-HTTPS deve ter uma chave privada.  
  
-   O certificado IP-HTTPS deve ser importado diretamente para o repositório pessoal.  
  
-   Os certificados IP-HTTPS podem ter curingas no nome.  
  
##### <a name="to-install-the-ip-https-certificate-from-an-internal-ca"></a>Para instalar o certificado IP-HTTPS de uma AC interna  
  
1.  No servidor DirectAccess: na tela **Iniciar** , digite**mmc.exe**e pressione Enter.  
  
2.  No console do MMC, no menu **Arquivo** , clique em **Adicionar/Remover Snap-in**.  
  
3.  Na caixa de diálogo **Adicionar ou Remover Snap-ins**, clique em **Certificados**, **Adicionar** e **Conta de computador**. Em seguida, clique em **Avançar**, **Computador Local**, **Concluir** e **OK**.  
  
4.  Na árvore de console do snap-in Certificados, abra **Certificados (Computador Local)\Pessoal\Certificados**.  
  
5.  Clique com o botão direito do mouse em **Certificados**, aponte para **Todas as Tarefas** e clique em **Solicitar Novo Certificado**.  
  
6.  Clique em **Avançar** duas vezes.  
  
7.  Na página **solicitar certificados** , marque a caixa de seleção do modelo de certificado que você criou anteriormente (para obter mais informações, consulte 1.5.2 configurar modelos de certificado). Se necessário, clique em **Mais informações são necessárias para se registrar neste certificado**.  
  
8.  Na caixa de diálogo **Propriedades do Certificado**, na guia **Assunto**, na área **Nome do requerente**, em **Tipo**, selecione **Nome Comum**.  
  
9. Em **Valor**, especifique o endereço IPv4 do adaptador externo do servidor do DirectAccess, ou o FQDN da URL IP-HTTPS, e clique em **Adicionar**.  
  
10. Na área **Nome alternativo**, em **Tipo**, selecione **DNS**.  
  
11. Em **Valor**, especifique o endereço IPv4 do adaptador externo do servidor do DirectAccess, ou o FQDN da URL IP-HTTPS, e clique em **Adicionar**.  
  
12. Na guia **Geral**, em **Nome amigável**, você pode digitar um nome que ajudará a identificar o certificado.  
  
13. Na guia **Extensões**, clique na seta ao lado de **Uso Estendido de Chave** e verifique se **Autenticação do Servidor** é exibido na lista **Opções selecionadas**.  
  
14. Clique em **OK**, **Registrar** e em **Concluir**.  
  
15. No painel de detalhes do snap-in Certificados, verifique se o novo certificado foi registrado com Finalidades de Autenticação de Servidor.  
  
## <a name="16-configure-the-dns-server"></a><a name="ConfigDNS"></a>1.6 Configurar o servidor DNS  
Você deve configurar manualmente uma entrada DNS para o site do servidor de local da rede interna da sua implantação.  
  
### <a name="to-create-the-network-location-server"></a><a name="NLS_DNS"></a>Para criar o servidor de local de rede  
  
1.  No servidor DNS da rede interna: na tela **Iniciar** , digite**DNSMGMT. msc**e pressione Enter.  
  
2.  No painel esquerdo do console **Gerenciador DNS**, expanda a zona de pesquisa direta para o seu domínio. Clique com o botão direito do mouse no domínio e clique em **Novo Host (A ou AAAA)**.  
  
3.  Na caixa de diálogo **Novo Host**, na caixa **Endereço IP**:  
  
    -   Na caixa **Nome (usa domínio pai se deixado em branco)**, digite o nome DNS do site do servidor de local de rede (o nome que os clientes do DirectAccess usam para se conectarem ao servidor de local de rede).  
  
    -   Digite o endereço IPv4 ou IPv6 do servidor de local de rede, clique em **Adicionar Host** e **OK**.  
  
4.  Na caixa de diálogo **Novo Host**:  
  
    -   Na caixa **Nome (usa domínio pai se deixado em branco)**, digite o nome DNS da investigação da Web (o nome padrão da investigação da Web é **directaccess-webprobehost**).  
  
    -   Na caixa **Endereço IP**, digite o endereço IPv4 ou IPv6 da investigação da Web e clique em **Adicionar Host**.  
  
    -   Repita esse processo para **directaccess-corpconnectivityhost** e para todos os verificadores de conectividade criados manualmente.  
  
5.  Na caixa de diálogo **DNS**, clique em **OK** e **Concluído**.  
  
![](../../../media/Step-1-Configuring-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)***<em>Comandos equivalentes</em> do Windows PowerShell***  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
```  
Add-DnsServerResourceRecordA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv4Address <network_location_server_IPv4_address>  
Add-DnsServerResourceRecordAAAA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv6Address <network_location_server_IPv6_address>  
```  
  
Você também deve configurar entradas DNS para o seguinte:  
  
-   **O servidor IP-HTTPS**  
  
    Os clientes do DirectAccess devem poder resolver o nome DNS do servidor do DirectAccess da Internet.  
  
-   **Verificação de revogação de CRL**  
  
    O DirectAccess usa a verificação de revogação de certificados para a conexão IP-HTTPS entre os clientes e o servidor do DirectAccess, e também para a conexão baseada em HTPPS entre o cliente do DirectAccess e o servidor de local de rede. Em ambos os casos, os clientes do DirectAccess devem poder resolver e acessar o local do ponto de distribuição da CRL.  
  
-   **ISATAP**  
  
    O protocolo ISATAP (Intrasite Automatic Tunnel Addressing Protocol) usa a criação de túneis para permitir que os clientes do DirectAccess estabeleçam conexão com o servidor DirectAccess via Internet IPv4, encapsulando pacotes IPv6 em um cabeçalho IPv4. Ele é usado pelo Acesso Remoto para fornecer conectividade IPv6 aos hosts ISATAP na intranet. Em um ambiente de rede IPv6 não nativo, o servidor do DirectAccess configura a si mesmo automaticamente como roteador ISATAP. É necessário suporte à resolução para o nome ISTAPA.  
  
## <a name="17-configure-active-directory"></a><a name="ConfigAD"></a>1.7 Configurar o Active Directory  
O servidor e todos os computadores cliente do DirectAccess devem ser reunidos em um domínio do Active Directory. Os computadores cliente do DirectAccess devem ser membros de um dos seguintes tipos de domínio:  
  
-   Domínios que pertençam à mesma floresta do servidor do DirectAccess.  
  
-   Domínios que pertençam a florestas com uma relação de confiança bidirecional com a floresta do servidor do DirectAccess.  
  
-   Domínios que tenham uma relação de confiança de domínio bidirecional com o domínio do servidor do DirectAccess.  
  
#### <a name="to-join-the-directaccess-server-to-a-domain"></a>Para ingressar o servidor do DirectAccess em um domínio  
  
1.  No Gerenciador do Servidor, clique em **Servidor Local**. No painel de detalhes, clique no link ao lado do **Nome do computador**.  
  
2.  No **propriedades do sistema** caixa de diálogo, clique em **nome do computador** guia e, em seguida, clique em **alteração**.  
  
3.  Em **Nome do Computador**, digite o nome do computador se você também estiver alterando o nome do computador ao ingressar o servidor no domínio. Em **Membro de**, clique em **Domínio**, digite o nome do domínio no qual deseja ingressar o servidor (por exemplo, corp.contoso.com) e clique em **OK**.  
  
4.  Quando você for solicitado a informar um nome de usuário e uma senha, digite o nome de usuário e a senha de um usuário com direitos de ingressar computadores no domínio e clique em **OK**.  
  
5.  Quando visualizar uma caixa de diálogo dando as boas-vindas ao domínio, clique em **OK**.  
  
6.  Quando você for solicitado a reiniciar o computador, clique em **OK**.  
  
7.  Na caixa de diálogo **Propriedades do Sistema**, clique em **Fechar**.  
  
8.  Quando for solicitado a reiniciar o computador, clique em **Reiniciar Agora**.  
  
#### <a name="to-join-client-computers-to-the-domain"></a>Para ingressar computadores cliente no domínio  
  
1.  Na tela **Iniciar** , digite**explorer.exe**e pressione Enter.  
  
2.  Clique com o botão direito do mouse no ícone Computador e em **Propriedades**.  
  
3.  Na página **Sistema**, clique em **Configurações avançadas do sistema**.  
  
4.  Na caixa de diálogo **Propriedades do Sistema**, na guia **Nome do Computador**, clique em **Alterar**.  
  
5.  Em **Nome do computador**, digite o nome do computador se você também estiver alterando o nome do computador ao ingressar o servidor no domínio. Em **Membro de**, clique em **Domínio**, digite o nome do domínio no qual deseja ingressar o servidor (por exemplo, corp.contoso.com) e clique em **OK**.  
  
6.  Quando você for solicitado a informar um nome de usuário e uma senha, digite o nome de usuário e a senha de um usuário com direitos de ingressar computadores no domínio e clique em **OK**.  
  
7.  Quando visualizar uma caixa de diálogo dando as boas-vindas ao domínio, clique em **OK**.  
  
8.  Quando você for solicitado a reiniciar o computador, clique em **OK**.  
  
9. Na caixa de diálogo **Propriedades do Sistema**, clique em **Fechar**.  
  
10. Quando for solicitado a reiniciar o computador, clique em **Reiniciar Agora**.  
  
![](../../../media/Step-1-Configuring-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)***<em>Comandos equivalentes</em> do Windows PowerShell***  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
> [!NOTE]  
> Você deve fornecer as credenciais do domínio ao digitar o comando **Add-Computer** a seguir.  
  
```  
Add-Computer -DomainName <domain_name>  
Restart-Computer  
```  
  
## <a name="18-configure-gpos"></a><a name="ConfigGPOs"></a>1.8 Configurar GPOs  
Um mínimo de dois objetos Política de Grupo são necessários para implantar o acesso remoto:  
  
-   Um contém configurações do servidor do DirectAccess  
  
-   Outro contém configurações dos computadores cliente do DirectAccess  
  
Quando você configura o acesso remoto, o assistente cria automaticamente os objetos de Política de Grupo necessários. No entanto, se sua organização impor uma convenção de nomenclatura, você poderá digitar um nome na caixa de diálogo do GPO, no Console de Gerenciamento de Acesso Remoto. Para saber mais, confira 2.7. Resumo de configuração e GPOs alternativos. Se você tiver criado permissões, o GPO será criado. Se você não tiver as permissões necessárias para criar GPOs, eles deverão ser criados antes da configuração do Acesso Remoto.  
  
Para criar Política de Grupo objetos, consulte [criar e editar um objeto política de grupo](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754740(v=ws.11)).  
  
> [!IMPORTANT]  
> Os administradores podem vincular manualmente os objetos de Política de Grupo do DirectAccess a uma UO (unidade organizacional) seguindo estas etapas:  
>   
> 1.  Antes de configurar o DirectAccess, vincule os GPOs criados às UOs respectivas.  
> 2.  Ao configurar o DirectAccess, especifique um grupo de segurança para os computadores cliente.  
> 3.  O administrador de acesso remoto pode ou não ter permissões para vincular os objetos de Política de Grupo ao domínio. De qualquer maneira, os Objetos de Política de Grupo serão configurados automaticamente. Se os GPOs já estiverem vinculados a uma OU, os links não serão removidos, e os GPOs não serão vinculados ao domínio. Para um GPO de servidor, a OU deverá conter o objeto de computador do servidor, ou o GPO será vinculado à raiz do domínio.  
> 4.  Se você não vinculou a UO antes de executar o assistente do DirectAccess, depois que a configuração for concluída, o administrador de domínio poderá vincular os objetos de Política de Grupo do DirectAccess às UOs necessárias. O link para o domínio pode ser removido. Para obter mais informações, consulte [Estabelecer um vínculo de um objeto de política de grupo](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732979(v=ws.11)).  
  
> [!NOTE]  
> Se um objeto Política de Grupo foi criado manualmente, é possível que o objeto Política de Grupo não esteja disponível durante a configuração do DirectAccess. O objeto Política de Grupo pode não ter sido replicado para o controlador de domínio mais próximo para o computador de gerenciamento. Neste caso, o administrador pode aguardar a replicação ser concluída, ou forçá-la.  
  
### <a name="181-configure-remote-access-gpos-with-limited-permissions"></a>1.8.1 Configurar GPOs do Acesso Remoto com permissões limitadas  
Em uma implantação que usa GPOs de preparo e de produção, o administrador do domínio deve fazer o seguinte:  
  
1.  Obter a lista de GPOs pedidos pela implantação do Acesso Remoto com o administrador do Acesso Remoto. Para saber mais, confira 1.8 Planejar Objetos de Política de Grupo.  
  
2.  Para cada GPO solicitado pelo administrador do Acesso Remoto, crie um par de GPOs com nomes diferentes. O primeiro será usado como GPO de preparo e o segundo, como GPO de produção.  
  
    Para criar Política de Grupo objetos, consulte [criar e editar um objeto política de grupo](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754740(v=ws.11)).  
  
3.  Para vincular os GPOs de produção, consulte [Estabelecer um vínculo de um objeto de política de grupo](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732979(v=ws.11)).  
  
4.  Conceda ao administrador do Acesso Remoto permissões para **Editar configurações, excluir e modificar a segurança** em todos os GPOs de preparo. Para obter mais informações, consulte [Delegar permissões para um grupo ou usuário em um objeto de política de grupo](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754542(v=ws.11)).  
  
5.  Negue as permissões de administrador de acesso remoto para vincular GPOs em todos os domínios (ou verifique se o administrador de acesso remoto não tem essas permissões). Para obter mais informações, consulte [Delegar permissões para vincular objetos de política de grupo](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc755086(v=ws.11)).  
  
Quando os administradores configurarem o Acesso Remoto, eles deverão sempre especificar apenas os GPOs de preparo (e não os de produção). Isso se aplica à configuração inicial do Acesso Remoto e ao executar operações de configuração adicional quando outros GPOs forem pedidos, por exemplo, ao adicionar pontos de entrada em uma implantação multissite ou habilitar computadores cliente em domínios adicionais.  
  
Depois que o administrador do Acesso Remoto concluir todas as alterações na configuração do Acesso Remoto, o administrador do domínio deverá examinar as configurações dos GPOs de preparo e usar o procedimento a seguir para copiar essas configurações nos GPOs de produção.  
  
> [!TIP]  
> Realize o procedimento a seguir após cada alteração na configuração do Acesso Remoto.  
  
##### <a name="to-copy-settings-to-the-production-gpos"></a>Para copiar as configurações nos GPOs de produção  
  
1.  Verifique se todos os GPOs de preparo da implantação do Acesso Remoto foram replicados a todos os controladores do domínio. Isso é necessário para garantir que a configuração mais recente seja importada nos GPOs de produção. Para saber mais, confira Verificar o status da infraestrutura da política de grupo.  
  
2.  Exporte as configurações fazendo o backup de todos os GPOs de preparo da implantação do Acesso Remoto. Para obter mais informações, consulte Fazer backup de um objeto de política de grupo.  
  
3.  Para cada GPO de produção, altere os filtros de segurança para que correspondam aos do GPO de preparo correspondente. Para obter mais informações, consulte Filtrar usando grupos de segurança.  
  
    > [!NOTE]  
    > Isso é necessário porque a operação **Importar configurações** não copia o filtro de segurança do GPO de origem.  
  
4.  Para cada GPO de produção, importe as configurações do backup do GPO de preparo correspondente da seguinte forma:  
  
    1.  No Console de Gerenciamento de Política de Grupo (GPMC), expanda o nó Política de Grupo objetos na floresta e no domínio que contém o objeto de Política de Grupo de produção no qual as configurações serão importadas.  
  
    2.  Clique com o botão direito do mouse no GPO e clique em **Importar Configurações**.  
  
    3.  No **Assistente para Importar Configurações**, na página **Bem-vindo**, clique em **Avançar**.  
  
    4.  Na página **Fazer backup do GPO**, clique em **Backup**.  
  
    5.  Na caixa de diálogo **Fazer Backup do Objeto de Política de Grupo**, na caixa **Local**, digite o caminho para o local no qual desejar armazenar os backups de GPO ou clique em **Procurar** para localizar a pasta.  
  
    6.  Na caixa **Descrição**, digite uma descrição para o GPO de produção e clique em **Fazer Backup**.  
  
    7.  Quando o backup for concluído, clique em **OK** e, na página **Fazer backup do GPO**, clique em **Avançar**.  
  
    8.  Na página **Local do backup**, na caixa **Pasta de backup**, digite o caminho para o local no qual o backup do GPO de preparo correspondente tiver sido armazenado na Etapa 2 ou clique em **Procurar** para localizar a pasta. Em seguida, clique em **Avançar**.  
  
    9. Na página **GPO de Origem**, marque a caixa de seleção **Mostrar somente a versão mais recente de cada GPO** para ocultar backups mais antigos e escolha o GPO de preparo correspondente. Clique em **Configurações de Exibição** para examinar as configurações do Acesso Remoto antes de aplicá-las ao GPO de produção e, então, clique em **Avançar**.  
  
    10. Na página **Examinando Backup**, clique em **Avançar** e **Concluir**.  
  
![](../../../media/Step-1-Configuring-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)***<em>Comandos equivalentes</em> do Windows PowerShell***  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
-   Para fazer backup do GPO de cliente de preparo "configurações de cliente do DirectAccess-preparo" no domínio "corp.contoso.com" para a pasta de backup "C:\Backups \" :  
  
    ```  
    $backup = Backup-GPO "Name 'DirectAccess Client Settings - Staging' "Domain 'corp.contoso.com' "Path 'C:\Backups\'  
    ```  
  
-   Para ver a filtragem de segurança do GPO de cliente de preparo "configurações de cliente do DirectAccess-preparo" no domínio "corp.contoso.com":  
  
    ```  
    Get-GPPermission "Name 'DirectAccess Client Settings - Staging' "Domain 'corp.contoso.com' "All | ?{ $_.Permission "eq 'GpoApply'}  
    ```  
  
-   Para adicionar o grupo de segurança "Corp. contoso. com\DirectAccess clients" ao filtro de segurança do GPO do cliente de produção "configurações do cliente do DirectAccess", "no domínio" corp.contoso.com ":  
  
    ```  
    Set-GPPermission "Name 'DirectAccess Client Settings - Production' "Domain 'corp.contoso.com' "PermissionLevel GpoApply "TargetName 'corp.contoso.com\DirectAccess clients' "TargetType Group  
    ```  
  
-   Para importar as configurações do backup para o GPO de cliente de produção "configurações de cliente do DirectAccess" de produção "no domínio" corp.contoso.com ":  
  
    ```  
    Import-GPO "BackupId $backup.Id "Path $backup.BackupDirectory "TargetName 'DirectAccess Client Settings - Production' "Domain 'corp.contoso.com'  
    ```  
  
## <a name="19-configure-security-groups"></a><a name="ConfigSGs"></a>1.9 Configurar grupos de segurança  
As configurações do DirectAccess que estão contidas no computador cliente Política de Grupo objeto são aplicadas somente a computadores que são membros dos grupos de segurança que você especificar ao configurar o acesso remoto. Além disso, se você está usando os grupos de segurança para gerenciar os servidores de aplicativo, crie um grupo de segurança para tais servidores.  
  
### <a name="to-create-a-security-group-for-directaccess-clients"></a><a name="Sec_Group"></a>Para criar um grupo de segurança para os clientes do DirectAccess  
  
1.  Na tela **Iniciar** , digite**DSA. msc**e pressione Enter. No console **Usuários e Computadores do Active Directory**, no painel esquerdo, expanda o domínio que conterá o grupo de segurança, clique com o botão direito do mouse em **Usuários**, aponte para **Novo** e clique em **Grupo**.  
  
2.  Na caixa de diálogo **Novo Objeto – Grupo**, em **Nome do grupo**, digite o nome do grupo de segurança.  
  
3.  Em **Escopo do grupo**, clique em **Global** e, em **Tipo de grupo**, clique em **Segurança** e em **OK**.  
  
4.  Clique duas vezes no grupo de segurança dos computadores cliente do DirectAccess e, na caixa de diálogo de propriedades, clique na guia **Membros**.  
  
5.  Na guia **Membros**, clique em **Adicionar**.  
  
6.  Na caixa de diálogo **Selecionar Usuários, Contatos, Computadores ou Contas de Serviço**, escolha os computadores cliente que desejar habilitar para o DirectAccess e clique em **OK**.  
  
![](../../../media/Step-1-Configuring-DirectAccess-Infrastructure/PowerShellLogoSmall.gif)**Comandos equivalentes** do Windows PowerShell  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
```  
New-ADGroup -GroupScope global -Name <DirectAccess_clients_group_name>  
Add-ADGroupMember -Identity DirectAccess_clients_group_name -Members <computer_name>  
```  
  
## <a name="110-configure-the-network-location-server"></a><a name="ConfigNLS"></a>1.10 Configurar o servidor de local de rede  
O servidor de local de rede deve ter alta disponibilidade, além de um certificado SSL válido e confiável para os clientes do DirectAccess. Existem duas opções de certificado para o certificado do servidor de local de rede:  
  
-   **Certificado privado**  
  
    Esse certificado se baseia no modelo de certificado que você criou seguindo as instruções em [1.5.2 configurar modelos de certificado](#ConfigCertTemp).  
  
-   **Certificado autoassinado**  
  
    > [!NOTE]  
    > Certificados autoassinados não podem ser usados em implantações multissite.  
  
As condições a seguir são pedidas para os dois tipos de certificado, caso ainda não existam:  
  
-   Um certificado do site usado para o servidor de local de rede. A entidade do certificado deve ser a URL do servidor de local de rede.  
  
-   Um ponto de distribuição da CRL que tenha alta disponibilidade da rede interna.  
  
> [!NOTE]  
> Se o site do servidor de local de rede estiver localizado no servidor do DirectAccess, um site será criado automaticamente quando você configurar o Acesso Remoto. Esse site será ligado ao certificado do servidor que você fornecer.  
  
#### <a name="to-install-the-network-location-server-certificate-from-an-internal-ca"></a>Para instalar o certificado de servidor de local de rede de uma AC interna  
  
1.  No servidor que hospedará o site do servidor de local de rede: na tela **Iniciar** , digite**mmc.exe**e pressione Enter.  
  
2.  No console do MMC, no menu **Arquivo** , clique em **Adicionar/Remover Snap-in**.  
  
3.  Na caixa de diálogo **Adicionar ou Remover Snap-ins**, clique em **Certificados**, **Adicionar** e **Conta de computador**. Em seguida, clique em **Avançar**, **Computador Local**, **Concluir** e **OK**.  
  
4.  Na árvore de console do snap-in Certificados, abra **Certificados (Computador Local)\Pessoal\Certificados**.  
  
5.  Clique com o botão direito do mouse em **Certificados**, aponte para **Todas as Tarefas** e clique em **Solicitar Novo Certificado**.  
  
6.  Clique em **Avançar** duas vezes.  
  
7.  Na página **solicitar certificados** , marque a caixa de seleção do modelo de certificado que você criou seguindo as instruções em 1.5.2 configurar modelos de certificado. Se necessário, clique em **Mais informações são necessárias para se registrar neste certificado**.  
  
8.  Na caixa de diálogo **Propriedades do Certificado**, na guia **Assunto**, na área **Nome do requerente**, em **Tipo**, selecione **Nome Comum**.  
  
9. Em **Valor**, insira o FQDN do site do servidor de local de rede e clique em **Adicionar**.  
  
10. Na área **Nome alternativo**, em **Tipo**, selecione **DNS**.  
  
11. Em **Valor**, insira o FQDN do site do servidor de local de rede e clique em **Adicionar**.  
  
12. Na guia **Geral**, em **Nome amigável**, você pode digitar um nome que ajudará a identificar o certificado.  
  
13. Clique em **OK**, **Registrar** e em **Concluir**.  
  
14. No painel de detalhes do snap-in Certificados, verifique se o novo certificado foi registrado com Finalidades de Autenticação de Servidor.  
  
#### <a name="to-configure-the-network-location-server"></a>Para configurar o servidor de local de rede  
  
1.  Configure um site em um servidor de alta disponibilidade. O site não precisa de nenhum conteúdo, mas ao testá-lo, você poderá definir uma página padrão que apresente uma mensagem quando o cliente se conectar.  
  
    > [!NOTE]  
    > Essa etapa não será necessária se o site do servidor de local de rede estiver hospedado no servidor do DirectAccess.  
  
2.  Associar um certificado de servidor HTTPS ao site. O nome comum do certificado deve corresponder ao nome do site do servidor de local de rede. Assegure-se de que os clientes do DirectAccess confiem na AC emissora.  
  
    > [!NOTE]  
    > Essa etapa não será necessária se o site do servidor de local de rede estiver hospedado no servidor do DirectAccess.  
  
3.  Configure um site de CRL que tenha alta disponibilidade da rede interna.  
  
    Os pontos de distribuição da CRL podem ser acessados por meio de:  
  
    -   Servidores Web usando uma URL baseada em HTTP, como:https://crl.corp.contoso.com/crld/corp-APP1-CA.crl  
  
    -   Servidores de arquivos que são acessados por meio de um caminho UNC (Convenção de nomenclatura universal), como \\ \crl.Corp.contoso.com\crld\corp-App1-ca.CRL  
  
    Se o ponto interno de distribuição da CRL só estiver acessível por IPv6, você deverá configurar uma regra de segurança de conexão de Firewall do Windows com Segurança Avançada para isentar a proteção de IPsec do endereço IPv6 da sua intranet para os endereços IPv6 dos pontos de distribuição da CRL.  
  
4.  Verifique se os clientes do DirectAccess na rede interna podem resolver o nome do servidor de local de rede. Verifique se o nome não pode ser resolvido pelos clientes do DirectAccess na Internet.  
  
## <a name="next-step"></a><a name="BKMK_Links"></a>Próxima etapa  
  
-   [Etapa 2: Configurar os servidores do DirectAccess Avançado](da-adv-configure-s2-servers.md)  
  
