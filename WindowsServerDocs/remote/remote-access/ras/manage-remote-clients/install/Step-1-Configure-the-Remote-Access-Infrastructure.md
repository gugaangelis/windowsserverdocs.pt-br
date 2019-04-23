---
title: Etapa 1 configurar a infraestrutura de acesso remoto
description: Este tópico faz parte do guia de clientes do DirectAccess de gerenciar remotamente no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0e7d1f5b-c939-47ca-892f-5bb285027fbc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dc08d89f7d84b5435e97ed5ed77eb72d003b0c84
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888567"
---
# <a name="step-1-configure-the-remote-access-infrastructure"></a>Etapa 1 configurar a infraestrutura de acesso remoto

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

**Observação:** O Windows Server 2012 reúne o DirectAccess e o RRAS (Serviço de Roteamento e Acesso Remoto) em uma única função de Acesso Remoto.  
  
Este tópico descreve como configurar a infraestrutura necessária para uma implantação avançada do acesso remoto usando um único servidor de acesso remoto em um ambiente misto de IPv4 e IPv6. Antes de começar as etapas de implantação, certifique-se de que você tenha concluído as etapas de planejamento descritas em [etapa 1: Planejar a infraestrutura de acesso remoto](../plan/Step-1-Plan-the-Remote-Access-Infrastructure.md).  
  
|Tarefa|Descrição|  
|----|--------|  
|Definir configurações de rede do servidor|Definir as configurações de rede do servidor no servidor de Acesso Remoto.|  
|Configurar o roteamento da rede corporativa|Configurar o roteamento da rede corporativa para verificar se o tráfego está devidamente roteado.|  
|Configurar firewalls|Configurar firewalls adicionais, se necessário.|  
|Configurar autoridades de certificação (CAs) e certificados|Configure uma autoridade de certificação (CA), se necessário e quaisquer outros modelos de certificado necessários na implantação.|  
|Configurar o servidor DNS|Definir as configurações de DNS para o servidor de Acesso Remoto.|  
|Configurar o Active Directory|Ingressar computadores cliente e o servidor de acesso remoto para o domínio do Active Directory.|  
|Configurar GPOs|Configure objetos de diretiva de grupo (GPOs) para a implantação, se necessário.|  
|Configurar os grupos de segurança|Configurar os grupos de segurança que conterão os computadores cliente do DirectAccess, além de qualquer outro grupo de segurança pedido pela implantação.|  
|Configurar o servidor de local de rede|Configurar o servidor de local de rede, inclusive instalar o certificado do site desse servidor.|  
  
> [!NOTE]  
> Este tópico inclui cmdlets do Windows PowerShell de exemplo que podem ser usados para automatizar alguns dos procedimentos descritos. Para obter mais informações, consulte [Usando cmdlets](https://go.microsoft.com/fwlink/p/?linkid=230693).  
  
## <a name="BKMK_ConfigNetworkSettings"></a>Definir configurações de rede do servidor  
Dependendo se você optar por colocar o servidor de acesso remoto na borda ou atrás de um dispositivo de tradução de endereço de rede (NAT), as seguintes configurações de endereço de interface de rede são necessárias para uma implantação de servidor único em um ambiente com IPv4 e IPv6. Todos os endereços IP são configurados usando **Alterar configurações do adaptador** na **Central de Rede e Compartilhamento do Windows**.  
  
**Topologia de borda**:  
  
Requer o seguinte:  
  
-   Dois para a Internet públicos estáticos IPv4 ou IPv6 endereços consecutivos.  
  
    > [!NOTE]  
    > Dois endereços IPv4 públicos consecutivos são necessários para o Teredo. Se não estiver usando o Teredo, você poderá configurar um endereço IPv4 estático público individual.  
  
-   Um único endereço estático interno IPv4 ou IPv6.  
  
**Por trás do dispositivo NAT (dois adaptadores de rede)**:  
  
Requer um único voltado para a rede endereço estático interno IPv4 ou IPv6.  
  
**Por trás do dispositivo NAT (um adaptador de rede)**:  
  
Requer um único endereço IPv4 ou IPv6 estático.  
  
Se o servidor de acesso remoto tem dois adaptadores de rede (uma para o perfil de domínio e outro para um perfil público ou privado), mas se você estiver usando uma topologia de adaptador de rede único, a recomendação é da seguinte maneira:  
  
1.  Certifique-se de que o segundo adaptador de rede também está classificado no perfil de domínio.  
  
2.  Se o segundo adaptador de rede não pode ser configurado para o perfil de domínio por algum motivo, a política de IPsec do DirectAccess deve ser manualmente estendida a todos os perfis usando o seguinte comando do Windows PowerShell:  
  
    ```  
    $gposession = Open-NetGPO -PolicyStore <Name of the server GPO>  
    Set-NetIPsecRule -DisplayName <Name of the IPsec policy> -GPOSession $gposession -Profile Any  
    Save-NetGPO -GPOSession $gposession  
    ```  
  
    Os nomes das políticas de IPsec para usar nesse comando são **DirectAccess-DaServerToInfra** e **DirectAccess-DaServerToCorp**.  
  
## <a name="BKMK_ConfigRouting"></a>Configurar o roteamento na rede corporativa  
Configure o roteamento na rede corporativa da seguinte forma:  
  
-   Quando o IPv6 nativo for implantado na organização, adicione uma rota para que os roteadores no tráfego IPv6 da rota de rede interna retorne pelo servidor de Acesso Remoto.  
  
-   Configure manualmente as rotas IPv4 e IPv6 da organização nos servidores de Acesso Remoto. Adicione uma rota publicada para que todo o tráfego com um (/ 48) prefixo IPv6 é encaminhado para a rede interna. Além disso, para tráfego IPv4, adicione rotas explícitas para que o tráfego IPv4 seja encaminhado para a rede interna.  
  
## <a name="BKMK_ConfigFirewalls"></a>Configurar firewalls  
Dependendo das configurações de rede que você escolheu, ao usar firewalls adicionais na sua implantação, aplique as seguintes exceções de firewall para tráfego de acesso remoto:  
  
### <a name="remote-access-server-on-ipv4-internet"></a>Servidor de acesso remoto na Internet IPv4  
Aplique as seguintes exceções de firewall voltado para a Internet para o tráfego de acesso remoto quando o servidor de acesso remoto estiver na Internet IPv4:  
  
-   **Tráfego Teredo**  
  
    Porta de destino de protocolo de datagrama (UDP) do usuário 3544 entrada e a porta de origem UDP 3544 de saída. Aplica essa isenção para ambos os para a Internet endereços IPv4 públicos consecutivos no servidor de acesso remoto.  
  
-   **tráfego 6to4**  
  
    Protocolo IP 41 de entrada e saída. Aplica essa isenção para ambos os para a Internet endereços IPv4 públicos consecutivos no servidor de acesso remoto.  
  
-   **Tráfego IP-HTTPS**  
  
    Porta de destino Transmission Control Protocol (TCP) 443 e a porta de origem TCP 443 de saída. Quando o servidor de Acesso Remoto tem um único adaptador de rede e o servidor de local de rede está no servidor de acesso remoto, a porta TCP 62000 também é necessária. Aplica essas isenções somente para o endereço para o qual resolve o nome externo do servidor.  
  
    > [!NOTE]  
    > Essa isenção é configurada no servidor de acesso remoto. Todas as outras isenções são configuradas no firewall de borda.  
  
### <a name="remote-access-server-on-ipv6-internet"></a>Servidor de acesso remoto na Internet IPv6  
Aplique as seguintes exceções de firewall voltado para a Internet para o tráfego de acesso remoto quando o servidor de acesso remoto estiver na Internet IPv6:  
  
-   Protocolo IP 50  
  
-   Entrada da porta de destino UDP 500 e saída da porta de origem UDP 500.  
  
-   Protocolo ICMP para IPv6 (ICMPv6) tráfego de entrada e saído - somente para implementações Teredo.  
  
### <a name="remote-access-traffic"></a>Tráfego de acesso remoto  
Aplique as seguintes exceções de firewall de rede interna para o tráfego de acesso remoto:  
  
-   ISATAP: Protocolo 41 de entrada e de saída  
  
-   TCP/UDP para todo o tráfego IPv4 ou IPv6  
  
-   ICMP para todo o tráfego IPv4 ou IPv6  
  
## <a name="BKMK_ConfigCAs"></a>Configurar autoridades de certificação e certificados  
Com o acesso remoto no Windows Server 2012, você escolha entre usar certificados para autenticação de computador ou usar uma autenticação de Kerberos interna que usa nomes de usuário e senhas. Você também deve configurar um certificado IP-HTTPS no servidor de acesso remoto. Esta seção explica como configurar esses certificados.  
  
Para obter informações sobre como configurar uma infraestrutura de chave pública (PKI), consulte [serviços de certificados do Active Directory](https://technet.microsoft.com/library/cc770357.aspx).  
  
### <a name="BKMK_ConfigIPsec"></a>Configurar a autenticação IPsec  
É necessário um certificado no servidor de acesso remoto e todos os clientes do DirectAccess para que eles possam usar a autenticação IPsec. O certificado deve ser emitido por uma CA (autoridade de certificação interna). Servidores de acesso remoto e os clientes DirectAccess devem confiar na AC que emite os certificados intermediários e raiz.  
  
##### <a name="to-configure-ipsec-authentication"></a>Para configurar a autenticação IPsec  
  
1.  Na AC interna, decida se usará o modelo de certificado de computador padrão ou se criará um novo modelo de certificado conforme descrito em [criando modelos de certificado](https://technet.microsoft.com/library/cc731705.aspx).  
  
    > [!NOTE]  
    > Se você criar um novo modelo, ele deve ser configurado para autenticação de cliente.  
  
2.  Implante o modelo de certificado, se necessário. Para obter mais informações, consulte [implantando modelos de certificado](https://technet.microsoft.com/library/cc770794.aspx).  
  
3.  Configure o modelo para registro automático, se necessário.  
  
4.  Configure o registro automático de certificado, se necessário. Para obter mais informações, consulte [configurar o registro automático de certificados](https://technet.microsoft.com/library/cc731522.aspx).  
  
### <a name="BKMK_ConfigCertTemp"></a>Configurar modelos de certificado  
Quando você usa uma CA interna para emitir certificados, você deve configurar modelos de certificado para o certificado IP-HTTPS e o certificado de site do servidor de local de rede.  
  
##### <a name="to-configure-a-certificate-template"></a>Para configurar um modelo de certificado  
  
1.  Na AC interna, crie um modelo de certificado conforme descrito em [Criando modelos de certificado](https://technet.microsoft.com/library/cc731705.aspx).  
  
2.  Implante o modelo de certificado conforme descrito em [Deploying Certificate Templates (Implantando modelos de certificado)](https://technet.microsoft.com/library/cc770794.aspx).  
  
Depois de preparar seus modelos, você pode usá-los para configurar os certificados. Consulte os procedimentos a seguir para obter detalhes:  
  
-   [Configurar o certificado IP-HTTPS](#BKMK_IPHTTPS)  
  
-   [Configurar o servidor de local de rede](#BKMK_ConfigNLS)  
  
### <a name="BKMK_IPHTTPS"></a>Configurar o certificado IP-HTTPS  
O Acesso Remoto requer um certificado IP-HTTPS para autenticar conexões IP-HTTPS para o servidor de Acesso Remoto. Existem três opções de certificado para o certificado IP-HTTPS:  
  
-   **Público**  
  
    Fornecido por terceiros.  
  
-   **privado**  
  
    O certificado baseia-se no modelo de certificado que você criou na [configurar modelos de certificado](assetId:///6a5ec5c1-d653-47b1-a567-cc485004e7bc#ConfigCertTemp). Ele requer, um ponto de distribuição de CRL (lista) de revogação de certificado que é acessível a partir de um FQDN resolvível publicamente.  
  
-   **Autoassinado**  
  
    Esse certificado exige um ponto de distribuição da CRL que seja acessível a partir de um FQDN resolvível publicamente.  
  
    > [!NOTE]  
    > Certificados autoassinados não podem ser usados em implantações multissite.  
  
Verifique se o certificado do site usado para autenticação IP-HTTPS atende aos seguintes requisitos:  
  
-   O nome de assunto do certificado deve ser o nome de domínio totalmente qualificado (FQDN) externamente resolvível da URL IP-HTTPS (o endereço ConnectTo) que é usado apenas para as conexões de IP-HTTPS do servidor de acesso remoto.  
  
-   O nome comum do certificado deverá ser corresponder ao nome do site IP-HTTPS.  
  
-   No campo assunto, especifique o endereço IPv4 do adaptador externo do servidor de acesso remoto ou o FQDN da URL IP-HTTPS.  
  
-   Para o **uso avançado de chave** campo, use o identificador de objeto (OID) de autenticação do servidor.  
  
-   No campo **Pontos de Distribuição da Lista de Certificados Revogados**, especifique um ponto de distribuição da CRL que seja acessível aos clientes do DirectAccess conectados à Internet.  
  
-   O certificado IP-HTTPS deve ter uma chave privada.  
  
-   O certificado IP-HTTPS deve ser importado diretamente para o repositório pessoal.  
  
-   Os certificados IP-HTTPS podem ter curingas no nome.  
  
##### <a name="to-install-the-ip-https-certificate-from-an-internal-ca"></a>Para instalar o certificado IP-HTTPS de uma AC interna  
  
1.  No servidor de Acesso Remoto: Sobre o **inicie** tela, digite**mmc.exe**, e pressione ENTER.  
  
2.  No console do MMC, no menu **Arquivo**, clique em **Adicionar/Remover Snap-in**.  
  
3.  Na caixa de diálogo **Adicionar ou Remover Snap-ins**, clique em **Certificados**, **Adicionar** e **Conta de computador**. Em seguida, clique em **Avançar**, **Computador Local**, **Concluir** e **OK**.  
  
4.  Na árvore de console do snap-in Certificados, abra **Certificados (Computador Local)\Pessoal\Certificados**.  
  
5.  Clique com botão direito **certificados**, aponte para **todas as tarefas**, clique em **Solicitar novo certificado**e, em seguida, clique em **Avançar** duas vezes...  
  
6.  Sobre o **solicitar certificados** , marque a caixa de seleção para o modelo de certificado que você criou na configuração de modelos de certificado e, se necessário, em **mais informações são necessárias para se registrar isso certificado**.  
  
7.  Na caixa de diálogo **Propriedades do Certificado**, na guia **Assunto**, na área **Nome do requerente**, em **Tipo**, selecione **Nome Comum**.  
  
8.  Na **valor**, especifique o endereço IPv4 do adaptador externo do servidor de acesso remoto ou o FQDN da URL IP-HTTPS e, em seguida, clique em **Add**.  
  
9. Na área **Nome alternativo**, em **Tipo**, selecione **DNS**.  
  
10. Na **valor**, especifique o endereço IPv4 do adaptador externo do servidor de acesso remoto ou o FQDN da URL IP-HTTPS e, em seguida, clique em **Add**.  
  
11. Na guia **Geral**, em **Nome amigável**, você pode digitar um nome que ajudará a identificar o certificado.  
  
12. Na guia **Extensões**, ao lado de **Uso de Chave Estendida**, clique na seta e verifique se a Autenticação do Servidor está na lista **Opções selecionadas**.  
  
13. Clique em **OK**, **Registrar** e em **Concluir**.  
  
14. No painel de detalhes do snap-in de certificados, verifique se que o novo certificado foi registrado com a finalidade de autenticação do servidor.  
  
## <a name="BKMK_ConfigDNS"></a>Configurar o servidor DNS  
Você deve configurar manualmente uma entrada DNS para o site do servidor de local da rede interna da sua implantação.  
  
### <a name="NLS_DNS"></a>Para adicionar a investigação web e servidor do local de rede  
  
1.  No servidor DNS da rede interna: Sobre o **inicie** tela, digite**Dnsmgmt. msc**, e pressione ENTER.  
  
2.  No painel esquerdo do console **Gerenciador DNS**, expanda a zona de pesquisa direta para o seu domínio. Clique com botão direito no domínio e, em seguida, clique em **novo Host (A ou AAAA)**.  
  
3.  No **novo Host** na caixa de **nome (usa nome do domínio pai se deixado em branco)** , digite o nome DNS para o site de servidor de local de rede (esse é o nome que os clientes do DirectAccess usam para se conectar ao servidor de local de rede). No **endereço IP** caixa, digite o endereço IPv4 do servidor de local de rede e clique em **Adicionar Host**e, em seguida, clique em **Okey**.  
  
4.  No **novo Host** na caixa de **nome (usa nome do domínio pai se deixado em branco)** , digite o nome DNS da investigação da web (o nome padrão da investigação da web é directaccess-webprobehost). Na caixa **Endereço IP**, digite o endereço IPv4 da sonda da web e clique em **Adicionar Host**.  
  
5.  Repita esse processo para o directaccess-corpconnectivityhost e quaisquer verificadores de conectividade criados manualmente. No **DNS** caixa de diálogo, clique em **Okey**.  
  
6.  Clique em **Concluído**.  
  
![Windows PowerShell](../../../../media/Step-1-Configure-the-Remote-Access-Infrastructure/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
```  
Add-DnsServerResourceRecordA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv4Address <network_location_server_IPv4_address>  
Add-DnsServerResourceRecordAAAA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv6Address <network_location_server_IPv6_address>  
```  
  
Você também deve configurar entradas DNS para o seguinte:  
  
-   **O servidor IP-HTTPS**  
  
    Os clientes do DirectAccess devem ser capazes de resolver o nome DNS do servidor de acesso remoto da Internet.  
  
-   **Verificação de revogação de CRL**  
  
    O DirectAccess usa a verificação de revogação de certificado para a conexão IP-HTTPS entre os clientes DirectAccess e o servidor de acesso remoto e para a conexão baseada em HTTPS entre o cliente DirectAccess e o servidor de local de rede. Em ambos os casos, os clientes do DirectAccess devem poder resolver e acessar o local do ponto de distribuição da CRL.  
  
-   **ISATAP**  
  
    Intrasite Automatic Tunnel Addressing Protocol (ISATAP) usa os túneis para permitir que os clientes DirectAccess se conectar ao servidor de acesso remoto pela Internet IPv4, encapsulando pacotes IPv6 em um cabeçalho IPv4. Ele é usado pelo Acesso Remoto para fornecer conectividade IPv6 aos hosts ISATAP na intranet. Em um ambiente de rede de IPv6 não nativo, o servidor de acesso remoto se configura automaticamente como um roteador ISATAP. É necessário suporte à resolução para o nome ISTAPA.  
  
## <a name="BKMK_ConfigAD"></a>Configurar o Active Directory  
O servidor de Acesso Remoto e todos os computadores cliente do DirectAccess devem ser ingressados em um domínio do Active Directory. Os computadores cliente do DirectAccess devem ser membros de um dos seguintes tipos de domínio:  
  
-   Domínios pertencentes à mesma floresta que o servidor de Acesso Remoto.  
  
-   Domínios pertencentes a florestas com relações de confiança bidirecional com a floresta do servidor de Acesso Remoto.  
  
-   Domínios com relação de confiança bidirecional com o domínio do servidor de Acesso Remoto.  
  
#### <a name="to-join-the-remote-access-server-to-a-domain"></a>Para ingressar o servidor do DirectAccess em um domínio  
  
1.  No Gerenciador do Servidor, clique em **Servidor Local**. No painel de detalhes, clique no link ao lado do **Nome do computador**.  
  
2.  No **propriedades do sistema** caixa de diálogo, clique em **nome do computador** guia e, em seguida, clique em **alteração**.  
  
3.  No **nome do computador** , digite o nome do computador, se você também estiver alterando o nome do computador ao ingressar o servidor ao domínio. Sob **membro**, clique em **domínio**e, em seguida, digite o nome do domínio ao qual você deseja ingressar o servidor, (por exemplo, corp.contoso.com) e, em seguida, clique em **Okey**.  
  
4.  Quando você for solicitado um nome de usuário e senha, insira o nome de usuário e a senha de um usuário com permissões para ingressar computadores ao domínio e, em seguida, clique em **Okey**.  
  
5.  Quando você vir uma caixa de diálogo de boas-vindas ao domínio, clique em **OK**.  
  
6.  Quando você for solicitado a reiniciar o computador, clique em **OK**.  
  
7.  Na caixa de diálogo **Propriedades do Sistema** , clique em **Fechar**.  
  
8.  Quando for solicitado a reiniciar o computador, clique em **Reiniciar Agora**.  
  
#### <a name="to-join-client-computers-to-the-domain"></a>Para ingressar computadores cliente no domínio  
  
1.  Sobre o **inicie** tela, digite**explorer.exe**, e pressione ENTER.  
  
2.  Clique com o botão direito do mouse no ícone Computador e em **Propriedades**.  
  
3.  Na página **Sistema**, clique em **Configurações avançadas do sistema**.  
  
4.  Na caixa de diálogo **Propriedades do Sistema**, na guia **Nome do Computador**, clique em **Alterar**.  
  
5.  No **nome do computador** , digite o nome do computador, se você também estiver alterando o nome do computador ao ingressar o servidor ao domínio. Em **Membro de**, clique em **Domínio**, digite o nome do domínio no qual deseja ingressar o servidor (por exemplo, corp.contoso.com) e clique em **OK**.  
  
6.  Quando você for solicitado um nome de usuário e senha, insira o nome de usuário e a senha de um usuário com permissões para ingressar computadores ao domínio e, em seguida, clique em **Okey**.  
  
7.  Quando você vir uma caixa de diálogo de boas-vindas ao domínio, clique em **OK**.  
  
8.  Quando você for solicitado a reiniciar o computador, clique em **OK**.  
  
9. No **propriedades do sistema** caixa de diálogo, clique em Fechar.  
  
10. Clique em **Reiniciar Agora** quando solicitado.  
  
![Windows PowerShell](../../../../media/Step-1-Configure-the-Remote-Access-Infrastructure/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandos * * *  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
> [!NOTE]  
> Depois de inserir o comando a seguir, você deve fornecer as credenciais de domínio.  
  
```  
Add-Computer -DomainName <domain_name>  
Restart-Computer  
```  
  
## <a name="BKMK_ConfigGPOs"></a>Configurar GPOs  
Para implantar acesso remoto, você precisará de um mínimo de dois objetos de diretiva de grupo. Um objeto de diretiva de grupo contém configurações para o servidor de acesso remoto, e o outro contém configurações para computadores cliente do DirectAccess. Quando você configurar o acesso remoto, o assistente cria automaticamente os objetos de diretiva de grupo necessários. No entanto, se sua organização impor uma convenção de nomenclatura, ou você não tenha as permissões necessárias para criar ou editar objetos de diretiva de grupo, eles devem ser criados antes de configurar o acesso remoto.  
  
Para criar objetos de diretiva de grupo, consulte [criar e editar um objeto de diretiva de grupo](https://technet.microsoft.com/library/cc754740.aspx).  
  
Um administrador pode vincular manualmente os objetos de diretiva de grupo do DirectAccess a uma unidade organizacional (UO). Considere o seguinte:  
  
1.  Vincule os GPOs criados às UOs respectivas antes de configurar o DirectAccess.  
  
2.  Ao configurar o DirectAccess, especifique um grupo de segurança para os computadores cliente.  
  
3.  Os GPOs são configurados automaticamente, independentemente de se o administrador tem permissões para vincular GPOs ao domínio.  
  
4.  Se os GPOs já estiverem vinculados a uma unidade Organizacional, os links não serão removidos, mas eles não estão vinculados ao domínio.  
  
5.  Para GPOs do servidor, a unidade Organizacional deve conter o servidor computador objeto; caso contrário, o GPO será vinculado à raiz do domínio.  
  
6.  Se a unidade Organizacional não tiver sido vinculada anteriormente, executando o Assistente de instalação do DirectAccess, depois que a configuração for concluída, o administrador pode vincular os GPOs do DirectAccess às UOs necessárias e remover o link para o domínio.  
  
    Para obter mais informações, consulte [vincular a um objeto de diretiva de grupo](https://technet.microsoft.com/library/cc732979.aspx).  
  
> [!NOTE]  
> Se um objeto de diretiva de grupo foi criado manualmente, é possível que o objeto de diretiva de grupo não estará disponível durante a configuração do DirectAccess. O objeto de diretiva de grupo pode não ter sido replicado para o controlador de domínio mais próximo ao computador de gerenciamento. O administrador pode aguardar a replicação concluir ou forçar a replicação.  
  
## <a name="BKMK_ConfigSGs"></a>Configurar grupos de segurança  
As configurações do DirectAccess que estão contidas no objeto de diretiva de grupo do computador de cliente são aplicadas somente aos computadores que são membros dos grupos de segurança que você especificar ao configurar o acesso remoto.  
  
### <a name="Sec_Group"></a>Para criar um grupo de segurança para clientes do DirectAccess  
  
1.  Sobre o **inicie** tela, digite**DSA. msc**, e pressione ENTER.  
  
2.  No console **Usuários e Computadores do Active Directory**, no painel esquerdo, expanda o domínio que conterá o grupo de segurança, clique com o botão direito do mouse em **Usuários**, aponte para **Novo** e clique em **Grupo**.  
  
3.  Na caixa de diálogo **Novo Objeto – Grupo**, em **Nome do grupo**, digite o nome do grupo de segurança.  
  
4.  Em **Escopo do grupo**, clique em **Global** e, em **Tipo de grupo**, clique em **Segurança** e em **OK**.  
  
5.  Clique duas vezes o grupo de segurança de computadores do cliente DirectAccess e, na **propriedades** caixa de diálogo, clique o **membros** guia.  
  
6.  Na guia **Membros** , clique em **Adicionar**.  
  
7.  Na caixa de diálogo **Selecionar Usuários, Contatos, Computadores ou Contas de Serviço**, escolha os computadores cliente que desejar habilitar para o DirectAccess e clique em **OK**.  
  
![Windows PowerShell](../../../../media/Step-1-Configure-the-Remote-Access-Infrastructure/PowerShellLogoSmall.gif)**comandos equivalentes do Windows PowerShell**  
  
O seguinte cmdlet ou cmdlets do Windows PowerShell executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, mesmo que possa aparecer quebra em várias linhas aqui devido a restrições de formatação.  
  
```  
New-ADGroup -GroupScope global -Name <DirectAccess_clients_group_name>  
Add-ADGroupMember -Identity DirectAccess_clients_group_name -Members <computer_name>  
```  
  
## <a name="BKMK_ConfigNLS"></a>Configurar o servidor de local de rede  
O servidor de local de rede deve estar em um servidor com alta disponibilidade, e ele precisa de um certificado válido de Secure Sockets Layer (SSL) que é confiável para os clientes do DirectAccess.  
  
> [!NOTE]  
> Se o site de servidor de local de rede estiver localizado no servidor de acesso remoto, um site será criado automaticamente quando você configura o acesso remoto e está associado ao certificado do servidor que você fornecer.  
  
Existem duas opções de certificado para o certificado do servidor de local de rede:  
  
-   **privado**  
  
    > [!NOTE]  
    > O certificado baseia-se no modelo de certificado que você criou na [configurar modelos de certificado](assetId:///6a5ec5c1-d653-47b1-a567-cc485004e7bc#ConfigCertTemp).  
  
-   **Autoassinado**  
  
    > [!NOTE]  
    > Certificados autoassinados não podem ser usados em implantações multissite.  
  
Se você usar um certificado privado ou um certificado autoassinado, exigem o seguinte:  
  
-   Um certificado do site usado para o servidor de local de rede. A entidade do certificado deve ser a URL do servidor de local de rede.  
  
-   Um ponto de distribuição da CRL que tenha alta disponibilidade na rede interna.  
  
#### <a name="to-install-the-network-location-server-certificate-from-an-internal-ca"></a>Para instalar o certificado de servidor de local de rede de uma AC interna  
  
1.  No servidor que hospedará o site do servidor de local de rede: Sobre o **inicie** tela, digite**mmc.exe**, e pressione ENTER.  
  
2.  No console do MMC, no menu **Arquivo**, clique em **Adicionar/Remover Snap-in**.  
  
3.  Na caixa de diálogo **Adicionar ou Remover Snap-ins**, clique em **Certificados**, **Adicionar** e **Conta de computador**. Em seguida, clique em **Avançar**, **Computador Local**, **Concluir** e **OK**.  
  
4.  Na árvore de console do snap-in Certificados, abra **Certificados (Computador Local)\Pessoal\Certificados**.  
  
5.  Clique com botão direito **certificados**, aponte para **todas as tarefas**, clique em **Solicitar novo certificado**e, em seguida, clique em **Avançar** duas vezes.  
  
6.  Sobre o **solicitar certificados** , marque a caixa de seleção para o modelo de certificado que você criou na configuração de modelos de certificado e, se necessário, em **mais informações são necessárias para se registrar isso certificado**.  
  
7.  Na caixa de diálogo **Propriedades do Certificado**, na guia **Assunto**, na área **Nome do requerente**, em **Tipo**, selecione **Nome Comum**.  
  
8.  Em **Valor**, insira o FQDN do site do servidor de local de rede e clique em **Adicionar**.  
  
9. Na área **Nome alternativo**, em **Tipo**, selecione **DNS**.  
  
10. Em **Valor**, insira o FQDN do site do servidor de local de rede e clique em **Adicionar**.  
  
11. Na guia **Geral**, em **Nome amigável**, você pode digitar um nome que ajudará a identificar o certificado.  
  
12. Clique em **OK**, **Registrar** e em **Concluir**.  
  
13. No painel de detalhes do snap-in de certificados, verifique se o que novo certificado foi registrado com a finalidade de autenticação do servidor.  
  
#### <a name="to-configure-the-network-location-server"></a>Para configurar o servidor de local de rede  
  
1.  Configure um site em um servidor de alta disponibilidade. O site não precisa de nenhum conteúdo, mas ao testá-lo, você poderá definir uma página padrão que apresente uma mensagem quando o cliente se conectar.  
  
    Essa etapa não será necessária se o site de servidor de local de rede estiver hospedado no servidor de acesso remoto.  
  
2.  Associar um certificado de servidor HTTPS ao site. O nome comum do certificado deve corresponder ao nome do site do servidor de local de rede. Assegure-se de que os clientes do DirectAccess confiem na AC emissora.  
  
    Essa etapa não será necessária se o site de servidor de local de rede estiver hospedado no servidor de acesso remoto.  
  
3.  Configure um site da CRL que hass alta disponibilidade na rede interna.  
  
    Os pontos de distribuição da CRL podem ser acessados por meio de:  
  
    -   Servidores Web que usam uma URL baseada em HTTP, como: https://crl.corp.contoso.com/crld/corp-APP1-CA.crl  
  
    -   Servidores de arquivos que são acessados por meio de um caminho universal naming convention (UNC), como \\\crl.corp.contoso.com\crld\corp-APP1-CA.crl  
  
    Se o ponto de distribuição da CRL interno é acessível somente por IPv6, você deve configurar um Firewall do Windows com a regra de segurança de conexão de segurança avançada. Isso isenta o proteção de IPsec do espaço de endereço IPv6 da sua intranet para os endereços IPv6 dos pontos de distribuição da CRL.  
  
4.  Certifique-se de que os clientes DirectAccess na rede interna podem resolver o nome do servidor de local de rede e que os clientes DirectAccess na Internet não é possível resolver o nome.  
  
## <a name="BKMK_Links"></a>Consulte também  
  
-   [Etapa 2: Configurar o servidor de acesso remoto](Step-2-Configure-the-Remote-Access-Server.md)

