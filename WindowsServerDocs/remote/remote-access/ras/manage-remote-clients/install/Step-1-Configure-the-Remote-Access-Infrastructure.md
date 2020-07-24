---
title: Etapa 1 configurar a infraestrutura de acesso remoto
description: Este tópico faz parte do guia gerenciar clientes DirectAccess remotamente no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 0e7d1f5b-c939-47ca-892f-5bb285027fbc
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 075d34a80abef25136f272ec530d693694e04799
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86965458"
---
# <a name="step-1-configure-the-remote-access-infrastructure"></a>Etapa 1 configurar a infraestrutura de acesso remoto

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

**Observação:** o Windows Server 2012 reúne o DirectAccess e o RRAS (Serviço de Roteamento e Acesso Remoto) em uma única função de Acesso Remoto.  
  
Este tópico descreve como configurar a infraestrutura necessária para uma implantação de acesso remoto avançada usando um único servidor de acesso remoto em um ambiente misto de IPv4 e IPv6. Antes de iniciar as etapas de implantação, verifique se você concluiu as etapas de planejamento descritas na [etapa 1: planejar a infraestrutura de acesso remoto](../plan/Step-1-Plan-the-Remote-Access-Infrastructure.md).  
  
|Tarefa|Descrição|  
|----|--------|  
|Definir configurações de rede do servidor|Definir as configurações de rede do servidor no servidor de Acesso Remoto.|  
|Configurar o roteamento da rede corporativa|Configurar o roteamento da rede corporativa para verificar se o tráfego está devidamente roteado.|  
|Configurar firewalls|Configurar firewalls adicionais, se necessário.|  
|Configurar autoridades de certificação (CAs) e certificados|Configure uma autoridade de certificação (CA), se necessário, e quaisquer outros modelos de certificado necessários na implantação.|  
|Configurar o servidor DNS|Definir as configurações de DNS para o servidor de Acesso Remoto.|  
|Configurar o Active Directory|Ingresse computadores cliente e o servidor de acesso remoto para o domínio Active Directory.|  
|Configurar GPOs|Configure objetos de Política de Grupo (GPOs) para a implantação, se necessário.|  
|Configurar os grupos de segurança|Configurar os grupos de segurança que conterão os computadores cliente do DirectAccess, além de qualquer outro grupo de segurança pedido pela implantação.|  
|Configurar o servidor de local de rede|Configurar o servidor de local de rede, inclusive instalar o certificado do site desse servidor.|  
  
> [!NOTE]  
> Este tópico inclui cmdlets do Windows PowerShell de exemplo que podem ser usados para automatizar alguns dos procedimentos descritos. Para obter mais informações, confira [Usando os Cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="configure-server-network-settings"></a><a name="BKMK_ConfigNetworkSettings"></a>Definir configurações de rede do servidor  
Dependendo de se você decidir posicionar o servidor de acesso remoto na borda ou atrás de um dispositivo NAT (conversão de endereços de rede), as seguintes configurações de endereço de interface de rede são necessárias para uma implantação de servidor único em um ambiente com IPv4 e IPv6. Todos os endereços IP são configurados usando **Alterar configurações do adaptador** na **Central de Rede e Compartilhamento do Windows**.  
  
**Topologia de borda**:  
  
Requer o seguinte:  
  
-   Dois endereços IPv4 ou IPv6 estáticos públicos consecutivos voltados para a Internet.  
  
    > [!NOTE]  
    > Dois endereços IPv4 públicos consecutivos são necessários para o Teredo. Se não estiver usando o Teredo, você poderá configurar um endereço IPv4 estático público individual.  
  
-   Um único endereço estático interno IPv4 ou IPv6.  
  
**Por trás do dispositivo NAT (dois adaptadores de rede)**:  
  
Requer um único endereço IPv4 ou IPv6 estático voltado para a rede interna.  
  
**Por trás do dispositivo NAT (um adaptador de rede)**:  
  
Requer um único endereço IPv4 ou IPv6 estático.  
  
Se o servidor de acesso remoto tiver dois adaptadores de rede (um para o perfil de domínio e outro para um perfil público ou privado), mas você estiver usando uma única topologia de adaptador de rede, a recomendação será a seguinte:  
  
1.  Verifique se o segundo adaptador de rede também está classificado no perfil de domínio.  
  
2.  Se o segundo adaptador de rede não puder ser configurado para o perfil de domínio por qualquer motivo, a política IPsec do DirectAccess deverá ser manualmente definida como escopo para todos os perfis usando o seguinte comando do Windows PowerShell:  
  
    ```  
    $gposession = Open-NetGPO -PolicyStore <Name of the server GPO>  
    Set-NetIPsecRule -DisplayName <Name of the IPsec policy> -GPOSession $gposession -Profile Any  
    Save-NetGPO -GPOSession $gposession  
    ```  
  
    Os nomes das diretivas IPsec a serem usadas neste comando são **DirectAccess-DaServerToInfra** e **DirectAccess-DaServerToCorp**.  
  
## <a name="configure-routing-in-the-corporate-network"></a><a name="BKMK_ConfigRouting"></a>Configurar o roteamento da rede corporativa  
Configure o roteamento na rede corporativa da seguinte forma:  
  
-   Quando o IPv6 nativo for implantado na organização, adicione uma rota para que os roteadores no tráfego IPv6 da rota de rede interna retorne pelo servidor de Acesso Remoto.  
  
-   Configure manualmente as rotas IPv4 e IPv6 da organização nos servidores de Acesso Remoto. Adicione uma rota publicada para que todo o tráfego com um prefixo (/48) IPv6 seja encaminhado para a rede interna. Além disso, para tráfego IPv4, adicione rotas explícitas para que o tráfego IPv4 seja encaminhado para a rede interna.  
  
## <a name="configure-firewalls"></a><a name="BKMK_ConfigFirewalls"></a>Configurar firewalls  
Dependendo das configurações de rede escolhidas, quando você usar firewalls adicionais em sua implantação, aplique as seguintes exceções de firewall para o tráfego de acesso remoto:  
  
### <a name="remote-access-server-on-ipv4-internet"></a>Servidor de acesso remoto na Internet IPv4  
Aplique as seguintes exceções de firewall voltadas para a Internet para tráfego de acesso remoto quando o servidor de acesso remoto estiver na Internet IPv4:  
  
-   **Tráfego Teredo**  
  
    Porta de destino UDP (User Datagram Protocol) 3544 de entrada e porta de origem UDP 3544 de saída. Aplique essa isenção para os endereços IPv4 públicos consecutivos voltados para a Internet no servidor de acesso remoto.  
  
-   **tráfego de 6to4**  
  
    Protocolo IP 41 de entrada e saída. Aplique essa isenção para os endereços IPv4 públicos consecutivos voltados para a Internet no servidor de acesso remoto.  
  
-   **Tráfego IP-HTTPS**  
  
    Porta de destino TCP (Transmission Control Protocol) 443 e saída da porta de origem TCP 443. Quando o servidor de Acesso Remoto tem um único adaptador de rede e o servidor de local de rede está no servidor de acesso remoto, a porta TCP 62000 também é necessária. Aplique essas isenções apenas para o endereço para o qual o nome externo do servidor é resolvido.  
  
    > [!NOTE]  
    > Essa isenção está configurada no servidor de acesso remoto. Todas as outras isenções são configuradas no firewall do Edge.  
  
### <a name="remote-access-server-on-ipv6-internet"></a>Servidor de acesso remoto na Internet IPv6  
Aplique as seguintes exceções de firewall voltadas para a Internet para tráfego de acesso remoto quando o servidor de acesso remoto estiver na Internet IPv6:  
  
-   Protocolo IP 50  
  
-   Entrada da porta de destino UDP 500 e saída da porta de origem UDP 500.  
  
-   Entrada e saída de tráfego do protocolo de mensagens de controle da Internet para IPv6 (ICMPv6) – somente para implementações de Teredo.  
  
### <a name="remote-access-traffic"></a>Tráfego de acesso remoto  
Aplique as seguintes exceções de firewall de rede interna para o tráfego de acesso remoto:  
  
-   ISATAP: protocolo 41 de entrada e saída  
  
-   TCP/UDP para todo o tráfego IPv4 ou IPv6  
  
-   ICMP para todo o tráfego IPv4 ou IPv6  
  
## <a name="configure-cas-and-certificates"></a><a name="BKMK_ConfigCAs"></a>Configurar autoridades de certificação (CAs) e certificados  
Com o acesso remoto no Windows Server 2012, você deve escolher entre usar certificados para autenticação do computador ou usar uma autenticação Kerberos interna que usa nomes de usuário e senhas. Você também deve configurar um certificado IP-HTTPS no servidor de acesso remoto. Esta seção explica como configurar esses certificados.  
  
Para obter informações sobre como configurar uma PKI (infraestrutura de chave pública), consulte [Active Directory serviços de certificados](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770357(v=ws.10)).  
  
### <a name="configure-ipsec-authentication"></a><a name="BKMK_ConfigIPsec"></a>Configurar a autenticação IPsec  
Um certificado é necessário no servidor de acesso remoto e em todos os clientes DirectAccess para que eles possam usar a autenticação IPsec. O certificado deve ser emitido por uma autoridade de certificação interna (CA). Os servidores de acesso remoto e os clientes do DirectAccess devem confiar na AC que emite os certificados raiz e intermediário.  
  
##### <a name="to-configure-ipsec-authentication"></a>Para configurar a autenticação IPsec  
  
1.  Na AC interna, decida se você usará o modelo de certificado do computador padrão ou se criará um novo modelo de certificado, conforme descrito em [criando modelos de certificado](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731705(v=ws.10)).  
  
    > [!NOTE]  
    > Se você criar um novo modelo, ele deverá ser configurado para autenticação de cliente.  
  
2.  Implante o modelo de certificado, se necessário. Para obter mais informações, consulte [Deploying Certificate Templates (Implantando modelos de certificado)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770794(v=ws.10)).  
  
3.  Configure o modelo para registro automático, se necessário.  
  
4.  Configure o registro automático de certificado, se necessário. Para obter mais informações, consulte [Configurar o registro automático de certificado](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731522(v=ws.11)).  
  
### <a name="configure-certificate-templates"></a><a name="BKMK_ConfigCertTemp"></a>Configurar modelos de certificado  
Ao usar uma autoridade de certificação interna para emitir certificados, você deve configurar modelos de certificado para o certificado IP-HTTPS e o certificado de site do servidor de local de rede.  
  
##### <a name="to-configure-a-certificate-template"></a>Para configurar um modelo de certificado  
  
1.  Na AC interna, crie um modelo de certificado conforme descrito em [Criando modelos de certificado](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731705(v=ws.10)).  
  
2.  Implante o modelo de certificado conforme descrito em [Deploying Certificate Templates (Implantando modelos de certificado)](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770794(v=ws.10)).  
  
Depois de preparar seus modelos, você pode usá-los para configurar os certificados. Consulte os procedimentos a seguir para obter detalhes:  
  
-   [Configurar o certificado IP-HTTPS](#BKMK_IPHTTPS)  
  
-   [Configurar o servidor de local de rede](#BKMK_ConfigNLS)  
  
### <a name="configure-the-ip-https-certificate"></a><a name="BKMK_IPHTTPS"></a>Configurar o certificado IP-HTTPS  
O Acesso Remoto requer um certificado IP-HTTPS para autenticar conexões IP-HTTPS para o servidor de Acesso Remoto. Existem três opções de certificado para o certificado IP-HTTPS:  
  
-   **Pública**  
  
    Fornecido por terceiros.  
  
-   **Privada**  
  
    O certificado é baseado no modelo de certificado que você criou na [configuração de modelos de certificado](assetId:///6a5ec5c1-d653-47b1-a567-cc485004e7bc#ConfigCertTemp). Ele requer um ponto de distribuição de CRL (lista de certificados revogados) que possa ser acessado por um FQDN que pode ser resolvido publicamente.  
  
-   **Autoassinado**  
  
    Esse certificado requer um ponto de distribuição de CRL acessível de um FQDN que pode ser resolvido publicamente.  
  
    > [!NOTE]  
    > Certificados autoassinados não podem ser usados em implantações multissite.  
  
Verifique se o certificado do site usado para autenticação IP-HTTPS atende aos seguintes requisitos:  
  
-   O nome da entidade do certificado deve ser o FQDN (nome de domínio totalmente qualificado) resolvido externamente da URL IP-HTTPS (o endereço connectto) que é usado somente para conexões IP-HTTPS do servidor de acesso remoto.  
  
-   O nome comum do certificado deverá ser corresponder ao nome do site IP-HTTPS.  
  
-   No campo assunto, especifique o endereço IPv4 do adaptador voltado para o externo do servidor de acesso remoto ou o FQDN da URL IP-HTTPS.  
  
-   Para o campo **uso avançado de chave** , use o identificador de objeto de autenticação de servidor (OID).  
  
-   No campo **Pontos de Distribuição da Lista de Certificados Revogados**, especifique um ponto de distribuição da CRL que seja acessível aos clientes do DirectAccess conectados à Internet.  
  
-   O certificado IP-HTTPS deve ter uma chave privada.  
  
-   O certificado IP-HTTPS deve ser importado diretamente para o repositório pessoal.  
  
-   Os certificados IP-HTTPS podem ter curingas no nome.  
  
##### <a name="to-install-the-ip-https-certificate-from-an-internal-ca"></a>Para instalar o certificado IP-HTTPS de uma AC interna  
  
1.  No servidor de acesso remoto: na tela **Iniciar** , digite**mmc.exe**e pressione Enter.  
  
2.  No console do MMC, no menu **Arquivo** , clique em **Adicionar/Remover Snap-in**.  
  
3.  Na caixa de diálogo **Adicionar ou Remover Snap-ins**, clique em **Certificados**, **Adicionar** e **Conta de computador**. Em seguida, clique em **Avançar**, **Computador Local**, **Concluir** e **OK**.  
  
4.  Na árvore de console do snap-in Certificados, abra **Certificados (Computador Local)\Pessoal\Certificados**.  
  
5.  Clique com o botão direito do mouse em **certificados**, aponte para **todas as tarefas**, clique em **solicitar novo certificado**e, em seguida, clique em **Avançar** duas vezes..  
  
6.  Na página **solicitar certificados** , marque a caixa de seleção do modelo de certificado que você criou na configuração de modelos de certificado e, se necessário, clique em **mais informações são necessárias para se registrar nesse certificado**.  
  
7.  Na caixa de diálogo **Propriedades do Certificado**, na guia **Assunto**, na área **Nome do requerente**, em **Tipo**, selecione **Nome Comum**.  
  
8.  Em **valor**, especifique o endereço IPv4 do adaptador voltado para o externo do servidor de acesso remoto ou o FQDN da URL IP-HTTPS e clique em **Adicionar**.  
  
9. Na área **Nome alternativo**, em **Tipo**, selecione **DNS**.  
  
10. Em **valor**, especifique o endereço IPv4 do adaptador voltado para o externo do servidor de acesso remoto ou o FQDN da URL IP-HTTPS e clique em **Adicionar**.  
  
11. Na guia **Geral**, em **Nome amigável**, você pode digitar um nome que ajudará a identificar o certificado.  
  
12. Na guia **Extensões**, ao lado de **Uso de Chave Estendida**, clique na seta e verifique se a Autenticação do Servidor está na lista **Opções selecionadas**.  
  
13. Clique em **OK**, **Registrar** e em **Concluir**.  
  
14. No painel de detalhes do snap-in certificados, verifique se o novo certificado foi registrado com a finalidade pretendida da autenticação do servidor.  
  
## <a name="configure-the-dns-server"></a><a name="BKMK_ConfigDNS"></a>Configurar o servidor DNS  
Você deve configurar manualmente uma entrada DNS para o site do servidor de local da rede interna da sua implantação.  
  
### <a name="to-add-the-network-location-server-and-web-probe"></a><a name="NLS_DNS"></a>Para adicionar o servidor de local de rede e investigação da Web  
  
1.  No servidor DNS da rede interna: na tela **Iniciar** , digite**DNSMGMT. msc**e pressione Enter.  
  
2.  No painel esquerdo do console **Gerenciador DNS**, expanda a zona de pesquisa direta para o seu domínio. Clique com o botão direito do mouse no domínio e clique em **novo host (A ou aaaa)**.  
  
3.  Na caixa de diálogo **novo host** , na caixa **nome (usa o nome de domínio pai se estiver em branco)** , insira o nome DNS para o site do servidor de local de rede (esse é o nome que os clientes DirectAccess usam para se conectar ao servidor de local de rede). Na caixa **endereço IP** , digite o endereço IPv4 do servidor de local de rede e clique em **Adicionar host**e em **OK**.  
  
4.  Na caixa de diálogo **novo host** , na caixa **nome (usa o nome de domínio pai se estiver em branco)** , insira o nome DNS para a investigação da Web (o nome da investigação da Web padrão é DirectAccess-webprobehost). Na caixa **Endereço IP**, digite o endereço IPv4 da sonda da web e clique em **Adicionar Host**.  
  
5.  Repita esse processo para o directaccess-corpconnectivityhost e quaisquer verificadores de conectividade criados manualmente. Na caixa de diálogo **DNS** , clique em **OK**.  
  
6.  Clique em **Concluído**.  
  
![](../../../../media/Step-1-Configure-the-Remote-Access-Infrastructure/PowerShellLogoSmall.gif)***<em>Comandos equivalentes</em> do Windows PowerShell***  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
```  
Add-DnsServerResourceRecordA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv4Address <network_location_server_IPv4_address>  
Add-DnsServerResourceRecordAAAA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv6Address <network_location_server_IPv6_address>  
```  
  
Você também deve configurar entradas DNS para o seguinte:  
  
-   **O servidor IP-HTTPS**  
  
    Os clientes do DirectAccess devem ser capazes de resolver o nome DNS do servidor de acesso remoto da Internet.  
  
-   **Verificação de revogação de CRL**  
  
    O DirectAccess usa a verificação de revogação de certificado para a conexão IP-HTTPS entre clientes DirectAccess e o servidor de acesso remoto e para a conexão baseada em HTTPS entre o cliente DirectAccess e o servidor de local de rede. Em ambos os casos, os clientes do DirectAccess devem poder resolver e acessar o local do ponto de distribuição da CRL.  
  
-   **ISATAP**  
  
    O protocolo ISATAP usa túneis para permitir que clientes DirectAccess se conectem ao servidor de acesso remoto pela Internet IPv4, encapsulando pacotes IPv6 dentro de um cabeçalho IPv4. Ele é usado pelo Acesso Remoto para fornecer conectividade IPv6 aos hosts ISATAP na intranet. Em um ambiente de rede IPv6 não nativo, o servidor de acesso remoto se configura automaticamente como um roteador ISATAP. É necessário suporte à resolução para o nome ISTAPA.  
  
## <a name="configure-active-directory"></a><a name="BKMK_ConfigAD"></a>Configurar o Active Directory  
O servidor de Acesso Remoto e todos os computadores cliente do DirectAccess devem ser ingressados em um domínio do Active Directory. Os computadores cliente do DirectAccess devem ser membros de um dos seguintes tipos de domínio:  
  
-   Domínios pertencentes à mesma floresta que o servidor de Acesso Remoto.  
  
-   Domínios pertencentes a florestas com relações de confiança bidirecional com a floresta do servidor de Acesso Remoto.  
  
-   Domínios com relação de confiança bidirecional com o domínio do servidor de Acesso Remoto.  
  
#### <a name="to-join-the-remote-access-server-to-a-domain"></a>Para ingressar o servidor do DirectAccess em um domínio  
  
1.  No Gerenciador do Servidor, clique em **Servidor Local**. No painel de detalhes, clique no link ao lado do **Nome do computador**.  
  
2.  No **propriedades do sistema** caixa de diálogo, clique em **nome do computador** guia e, em seguida, clique em **alteração**.  
  
3.  Na caixa **nome do computador** , digite o nome do computador se você também estiver alterando o nome do computador ao ingressar o servidor no domínio. Em **membro de**, clique em **domínio**e digite o nome do domínio ao qual você deseja ingressar no servidor (por exemplo, Corp.contoso.com) e clique em **OK**.  
  
4.  Quando for solicitado um nome de usuário e uma senha, insira o nome de usuário e a senha de um usuário com permissões para ingressar computadores no domínio e clique em **OK**.  
  
5.  Quando você vir uma caixa de diálogo de boas-vindas ao domínio, clique em **OK**.  
  
6.  Quando você for solicitado a reiniciar o computador, clique em **OK**.  
  
7.  Na caixa de diálogo **Propriedades do Sistema**, clique em **Fechar**.  
  
8.  Quando for solicitado a reiniciar o computador, clique em **Reiniciar Agora**.  
  
#### <a name="to-join-client-computers-to-the-domain"></a>Para ingressar computadores cliente no domínio  
  
1.  Na tela **Iniciar** , digite**explorer.exe**e pressione Enter.  
  
2.  Clique com o botão direito do mouse no ícone Computador e em **Propriedades**.  
  
3.  Na página **Sistema**, clique em **Configurações avançadas do sistema**.  
  
4.  Na caixa de diálogo **Propriedades do Sistema**, na guia **Nome do Computador**, clique em **Alterar**.  
  
5.  Na caixa **nome do computador** , digite o nome do computador se você também estiver alterando o nome do computador ao ingressar o servidor no domínio. Em **Membro de**, clique em **Domínio**, digite o nome do domínio no qual deseja ingressar o servidor (por exemplo, corp.contoso.com) e clique em **OK**.  
  
6.  Quando for solicitado um nome de usuário e uma senha, insira o nome de usuário e a senha de um usuário com permissões para ingressar computadores no domínio e clique em **OK**.  
  
7.  Quando você vir uma caixa de diálogo de boas-vindas ao domínio, clique em **OK**.  
  
8.  Quando você for solicitado a reiniciar o computador, clique em **OK**.  
  
9. Na caixa de diálogo **Propriedades do sistema** , clique em fechar.  
  
10. Clique em **Reiniciar Agora** quando solicitado.  
  
![](../../../../media/Step-1-Configure-the-Remote-Access-Infrastructure/PowerShellLogoSmall.gif)***<em>Comandos equivalentes</em> do Windows PowerShell***  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
> [!NOTE]  
> Você deve fornecer credenciais de domínio depois de inserir o comando a seguir.  
  
```  
Add-Computer -DomainName <domain_name>  
Restart-Computer  
```  
  
## <a name="configure-gpos"></a><a name="BKMK_ConfigGPOs"></a>Configurar GPOs  
Para implantar o acesso remoto, você precisa de um mínimo de dois objetos Política de Grupo. Um objeto Política de Grupo contém configurações para o servidor de acesso remoto e um contém configurações para computadores cliente do DirectAccess. Quando você configura o acesso remoto, o assistente cria automaticamente os objetos de Política de Grupo necessários. No entanto, se sua organização aplicar uma Convenção de nomenclatura ou se você não tiver as permissões necessárias para criar ou editar objetos Política de Grupo, eles deverão ser criados antes de configurar o acesso remoto.  
  
Para criar Política de Grupo objetos, consulte [criar e editar um objeto política de grupo](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc754740(v=ws.11)).  
  
Um administrador pode vincular manualmente os objetos de Política de Grupo do DirectAccess a uma UO (unidade organizacional). Considere o seguinte:  
  
1.  Vincule os GPOs criados às respectivas UOs antes de configurar o DirectAccess.  
  
2.  Ao configurar o DirectAccess, especifique um grupo de segurança para os computadores cliente.  
  
3.  Os GPOs são configurados automaticamente, independentemente de se o administrador tiver permissões para vincular os GPOs ao domínio.  
  
4.  Se os GPOs já estiverem vinculados a uma UO, os links não serão removidos, mas eles não serão vinculados ao domínio.  
  
5.  Para GPOs de servidor, a UO deve conter o objeto de computador de servidor-caso contrário, o GPO será vinculado à raiz do domínio.  
  
6.  Se a UO não tiver sido vinculada anteriormente executando o assistente de instalação do DirectAccess, depois que a configuração for concluída, o administrador poderá vincular os GPOs do DirectAccess às UOs necessárias e remover o link para o domínio.  
  
    Para obter mais informações, consulte [Estabelecer um vínculo de um objeto de política de grupo](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732979(v=ws.11)).  
  
> [!NOTE]  
> Se um objeto Política de Grupo foi criado manualmente, é possível que o objeto Política de Grupo não esteja disponível durante a configuração do DirectAccess. O objeto Política de Grupo pode não ter sido replicado para o controlador de domínio mais próximo do computador de gerenciamento. O administrador pode aguardar a replicação concluir ou forçar a replicação.  
  
## <a name="configure-security-groups"></a><a name="BKMK_ConfigSGs"></a>Configurar os grupos de segurança  
As configurações do DirectAccess que estão contidas no computador cliente Política de Grupo objeto são aplicadas somente a computadores que são membros dos grupos de segurança que você especificar ao configurar o acesso remoto.  
  
### <a name="to-create-a-security-group-for-directaccess-clients"></a><a name="Sec_Group"></a>Para criar um grupo de segurança para os clientes do DirectAccess  
  
1.  Na tela **Iniciar** , digite**DSA. msc**e pressione Enter.  
  
2.  No console **Usuários e Computadores do Active Directory**, no painel esquerdo, expanda o domínio que conterá o grupo de segurança, clique com o botão direito do mouse em **Usuários**, aponte para **Novo** e clique em **Grupo**.  
  
3.  Na caixa de diálogo **Novo Objeto – Grupo**, em **Nome do grupo**, digite o nome do grupo de segurança.  
  
4.  Em **Escopo do grupo**, clique em **Global** e, em **Tipo de grupo**, clique em **Segurança** e em **OK**.  
  
5.  Clique duas vezes no grupo de segurança computadores cliente DirectAccess e, na caixa de diálogo **Propriedades** , clique na guia **Membros** .  
  
6.  Na guia **Membros**, clique em **Adicionar**.  
  
7.  Na caixa de diálogo **Selecionar Usuários, Contatos, Computadores ou Contas de Serviço**, escolha os computadores cliente que desejar habilitar para o DirectAccess e clique em **OK**.  
  
![](../../../../media/Step-1-Configure-the-Remote-Access-Infrastructure/PowerShellLogoSmall.gif)**Comandos equivalentes** do Windows PowerShell  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
```  
New-ADGroup -GroupScope global -Name <DirectAccess_clients_group_name>  
Add-ADGroupMember -Identity DirectAccess_clients_group_name -Members <computer_name>  
```  
  
## <a name="configure-the-network-location-server"></a><a name="BKMK_ConfigNLS"></a>Configurar o servidor de local de rede  
O servidor de local de rede deve estar em um servidor com alta disponibilidade e precisa de um certificado de protocolo SSL (SSL) válido que seja confiável para os clientes do DirectAccess.  
  
> [!NOTE]  
> Se o site do servidor do local de rede estiver localizado no servidor de acesso remoto, um site será criado automaticamente quando você configurar o acesso remoto e ele estiver associado ao certificado do servidor que você fornecer.  
  
Existem duas opções de certificado para o certificado do servidor de local de rede:  
  
-   **Privada**  
  
    > [!NOTE]  
    > O certificado é baseado no modelo de certificado que você criou na [configuração de modelos de certificado](assetId:///6a5ec5c1-d653-47b1-a567-cc485004e7bc#ConfigCertTemp).  
  
-   **Autoassinado**  
  
    > [!NOTE]  
    > Certificados autoassinados não podem ser usados em implantações multissite.  
  
Se você usar um certificado privado ou um certificado autoassinado, ele exigirá o seguinte:  
  
-   Um certificado do site usado para o servidor de local de rede. A entidade do certificado deve ser a URL do servidor de local de rede.  
  
-   Um ponto de distribuição de CRL que tem alta disponibilidade na rede interna.  
  
#### <a name="to-install-the-network-location-server-certificate-from-an-internal-ca"></a>Para instalar o certificado de servidor de local de rede de uma AC interna  
  
1.  No servidor que hospedará o site do servidor de local de rede: na tela **Iniciar** , digite**mmc.exe**e pressione Enter.  
  
2.  No console do MMC, no menu **Arquivo** , clique em **Adicionar/Remover Snap-in**.  
  
3.  Na caixa de diálogo **Adicionar ou Remover Snap-ins**, clique em **Certificados**, **Adicionar** e **Conta de computador**. Em seguida, clique em **Avançar**, **Computador Local**, **Concluir** e **OK**.  
  
4.  Na árvore de console do snap-in Certificados, abra **Certificados (Computador Local)\Pessoal\Certificados**.  
  
5.  Clique com o botão direito do mouse em **certificados**, aponte para **todas as tarefas**, clique em **solicitar novo certificado**e, em seguida, clique em **Avançar** duas vezes.  
  
6.  Na página **solicitar certificados** , marque a caixa de seleção do modelo de certificado que você criou na configuração de modelos de certificado e, se necessário, clique em **mais informações são necessárias para se registrar nesse certificado**.  
  
7.  Na caixa de diálogo **Propriedades do Certificado**, na guia **Assunto**, na área **Nome do requerente**, em **Tipo**, selecione **Nome Comum**.  
  
8.  Em **Valor**, insira o FQDN do site do servidor de local de rede e clique em **Adicionar**.  
  
9. Na área **Nome alternativo**, em **Tipo**, selecione **DNS**.  
  
10. Em **Valor**, insira o FQDN do site do servidor de local de rede e clique em **Adicionar**.  
  
11. Na guia **Geral**, em **Nome amigável**, você pode digitar um nome que ajudará a identificar o certificado.  
  
12. Clique em **OK**, **Registrar** e em **Concluir**.  
  
13. No painel de detalhes do snap-in certificados, verifique se o novo certificado foi registrado com a finalidade pretendida da autenticação do servidor.  
  
#### <a name="to-configure-the-network-location-server"></a>Para configurar o servidor de local de rede  
  
1.  Configure um site em um servidor de alta disponibilidade. O site não precisa de nenhum conteúdo, mas ao testá-lo, você poderá definir uma página padrão que apresente uma mensagem quando o cliente se conectar.  
  
    Essa etapa não será necessária se o site do servidor do local de rede estiver hospedado no servidor de acesso remoto.  
  
2.  Associar um certificado de servidor HTTPS ao site. O nome comum do certificado deve corresponder ao nome do site do servidor de local de rede. Assegure-se de que os clientes do DirectAccess confiem na AC emissora.  
  
    Essa etapa não será necessária se o site do servidor do local de rede estiver hospedado no servidor de acesso remoto.  
  
3.  Configure um site de CRL que Hass alta disponibilidade na rede interna.  
  
    Os pontos de distribuição da CRL podem ser acessados por meio de:  
  
    -   Servidores Web que usam uma URL baseada em HTTP, como:https://crl.corp.contoso.com/crld/corp-APP1-CA.crl  
  
    -   Servidores de arquivos que são acessados por meio de um caminho UNC (Convenção de nomenclatura universal), como \\ \crl.Corp.contoso.com\crld\corp-App1-ca.CRL  
  
    Se o ponto de distribuição interno da CRL for alcançável somente por IPv6, você deverá configurar uma regra de segurança de conexão do firewall do Windows com segurança avançada. Isso isenta a proteção IPsec do espaço de endereço IPv6 da sua intranet para os endereços IPv6 dos pontos de distribuição da CRL.  
  
4.  Verifique se os clientes DirectAccess na rede interna podem resolver o nome do servidor de local de rede e se os clientes DirectAccess na Internet não podem resolver o nome.  
  
## <a name="see-also"></a><a name="BKMK_Links"></a>Confira também  
  
-   [Etapa 2: Configurar o servidor de acesso remoto](Step-2-Configure-the-Remote-Access-Server.md)
