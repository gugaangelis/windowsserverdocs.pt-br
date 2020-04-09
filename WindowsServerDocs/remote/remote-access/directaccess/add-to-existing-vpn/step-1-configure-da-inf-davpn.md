---
title: Etapa 1 configurar a infraestrutura do DirectAccess
description: Este tópico faz parte do guia adicionar o DirectAccess a uma implantação de VPN (acesso remoto) existente para o Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 5dc529f7-7bc3-48dd-b83d-92a09e4055c4
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d40a7bff2b726f1fa1ee768c677e0f9fa1658c6b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858019"
---
# <a name="step-1-configure-the-directaccess-infrastructure"></a>Etapa 1 configurar a infraestrutura do DirectAccess

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

Este tópico descreve como configurar a infraestrutura requerida para habilitar o DirectAccess em uma implantação da VPN existente. Antes de iniciar as etapas de implantação, verifique se você concluiu as etapas de planejamento descritas na [etapa 1: planejar a infraestrutura do DirectAccess](Step-1-Plan-DirectAccess-Infrastructure.md).  
  
|{1&gt;Tarefa&lt;1}|Descrição|  
|----|--------|  
|Definir configurações de rede do servidor|Definir as configurações de rede do servidor no servidor de Acesso Remoto.|  
|Configurar o roteamento da rede corporativa|Configurar o roteamento da rede corporativa para verificar se o tráfego está devidamente roteado.|  
|Configurar firewalls|Configurar firewalls adicionais, se necessário.|  
|Configurar autoridades de certificação (CAs) e certificados|O Assistente para Habilitar o DirectAccess configura um proxy Kerberos interno que permite autenticação usando nomes de usuário e senhas. Ele também configura um certificado IP-HTTPS no servidor de Acesso Remoto.|  
|Configurar o servidor DNS|Definir as configurações de DNS para o servidor de Acesso Remoto.|  
|Configurar o Active Directory|Ingressar os computadores cliente ao domínio do Active Directory.|  
|Configurar GPOs|Configurar os GPOs para implantação, se necessário.|  
|Configurar os grupos de segurança|Configurar os grupos de segurança que conterão os computadores cliente do DirectAccess e quaisquer outros grupos de segurança requeridos na implantação.|  
|Configurar o servidor de local de rede|O Assistente para Habilitar o DirectAccess configura o servidor de local da rede no servidor DirectAccess.|  
  
## <a name="configure-server-network-settings"></a><a name="ConfigNetworkSettings"></a>Definir configurações de rede do servidor  
As configurações de interface de rede a seguir são requeridas para uma implantação de servidor único em um ambiente com IPv4 e IPv6. Todos os endereços IP são configurados usando **Alterar configurações do adaptador** na **Central de Rede e Compartilhamento do Windows**.  
  
-   Topologia de borda  
  
    -   Um endereço público estático IPv4 ou IPv6 voltado para a Internet.  
  
    -   Um único endereço estático interno IPv4 ou IPv6.  
  
-   Atrás do dispositivo NAT (dois adaptadores de rede)  
  
    -   Um único endereço estático interno IPv4 ou IPv6 voltado para a rede.  
  
-   Atrás do dispositivo NAT (um adaptador de rede)  
  
    -   Um único endereço estático IPv4 ou IPv6.  
  
> [!NOTE]  
> Se o servidor de Acesso Remoto tiver dois adaptadores de rede (um classificado no perfil do domínio e outro em um perfil público/privado), porém uma topologia de NIC único for usada, veja as recomendações a seguir:  
>   
> 1.  Certifique-se que o 2º NIC também está classificado no perfil do domínio - Recomendado.  
> 2.  Se o 2º NIC não puder ser configurado para o perfil do domínio por alguma razão, a política de IPsec do DirectAccess deve ser manualmente estendida a todos os perfis usando os seguintes comandos do Windows PowerShell:  
>   
>     ```  
>     $gposession = Open-NetGPO -PolicyStore <Name of the server GPO>  
>     Set-NetIPsecRule -DisplayName <Name of the IPsec policy> -GPOSession $gposession -Profile Any  
>     Save-NetGPO -GPOSession $gposession  
>     ```  
  
## <a name="configure-routing-in-the-corporate-network"></a><a name="ConfigRouting"></a>Configurar o roteamento na rede corporativa  
Configure o roteamento na rede corporativa da seguinte forma:  
  
-   Quando o IPv6 nativo for implantado na organização, adicione uma rota para que os roteadores no tráfego IPv6 da rota de rede interna retorne pelo servidor de Acesso Remoto.  
  
-   Configure manualmente as rotas IPv4 e IPv6 da organização nos servidores de Acesso Remoto. Adicione uma rota publicada para que todo o tráfego com um prefixo IPv6 (/48) da organização seja encaminhado à rede interna. Além disso, para tráfego IPv4, adicione rotas explícitas para que o tráfego IPv4 seja encaminhado para a rede interna.  
  
## <a name="configure-firewalls"></a><a name="ConfigFirewalls"></a>Configurar firewalls  
Ao usar firewalls adicionais na sua implantação, aplique as seguintes exceções de firewall voltado para a Internet do tráfego de Acesso Remoto quando o servidor de Acesso Remoto está na Internet IPv4:  
  
-   tráfego 6to4-protocolo IP 41 de entrada e saída.  
  
-   IP-HTTPS-protocolo TCP porta de destino 443 e saída da porta de origem TCP 443. Quando o servidor de Acesso Remoto tem um único adaptador de rede e o servidor de local de rede está no servidor de acesso remoto, a porta TCP 62000 também é necessária.  
  
Ao usar firewalls adicionais, aplique as seguintes exceções de firewall voltado para a Internet do tráfego de Acesso Remoto quando o servidor de Acesso Remoto está na Internet IPv6:  
  
-   Protocolo IP 50  
  
-   Entrada da porta de destino UDP 500 e saída da porta de origem UDP 500.  
  
Ao usar firewalls adicionais, aplique as seguintes exceções do firewall de rede interna no tráfego de Acesso Remoto:  
  
-   ISATAP-protocolo 41 de entrada e saída  
  
-   TCP/UDP para todo o tráfego IPv4/IPv6  
  
## <a name="configure-cas-and-certificates"></a><a name="ConfigCAs"></a>Configurar CAs e certificados  
O Assistente para Habilitar o DirectAccess configura um proxy Kerberos interno que permite autenticação usando nomes de usuário e senhas. Ele também configura um certificado IP-HTTPS no servidor de Acesso Remoto.  
  
### <a name="configure-certificate-templates"></a><a name="ConfigCertTemp"></a>Configurar modelos de certificado  
Ao usar uma AC interna para emitir certificados, é necessário configurar um modelo de certificado para o certificado IP-HTTPS e o certificado de site do servidor de local de rede.  
  
##### <a name="to-configure-a-certificate-template"></a>Para configurar um modelo de certificado  
  
1.  Na AC interna, crie um modelo de certificado conforme descrito em [Criando modelos de certificado](https://technet.microsoft.com/library/cc731705.aspx).  
  
2.  Implante o modelo de certificado conforme descrito em [Deploying Certificate Templates (Implantando modelos de certificado)](https://technet.microsoft.com/library/cc770794.aspx).  
  
### <a name="configure-the-ip-https-certificate"></a>Configurar o certificado IP-HTTPS  
O Acesso Remoto requer um certificado IP-HTTPS para autenticar conexões IP-HTTPS para o servidor de Acesso Remoto. Existem três opções de certificado para o certificado IP-HTTPS:  
  
-   **Público**-fornecido por um terceiro.  
  
    Um certificado usado para autenticação IP-HTTPS. Caso o nome da entidade do certificado não seja um curinga, ele deverá ser a URL de FQDN resolvível externamente, usada somente para conexões IP-HTTPS do servidor de Acesso Remoto.  
  
-   **Particular**-os itens a seguir são necessários, se ainda não existirem:  
  
    -   Um certificado de site usado para autenticação IP-HTTPS. A entidade do certificado deve ser um FQDN (nome de domínio totalmente qualificado) externamente resolvível que pode ser obtido da Internet.  
  
    -   Um ponto de distribuição da lista de revogação de certificados (CRL) que está acessível por um FQDN resolvível publicamente.  
  
-   **Autoassinado**-os itens a seguir são necessários, se ainda não existirem:  
  
    > [!NOTE]  
    > Certificados autoassinados não podem ser usados em implantações multissite.  
  
    -   Um certificado de site usado para autenticação IP-HTTPS. A entidade do certificado deve ser um FQDN externamente resolvível que pode ser obtido da Internet.  
  
    -   Um ponto de distribuição de CRL que pode ser obtido de um FQDN (nome de domínio totalmente qualificado) resolvível publicamente.  
  
Verifique se o certificado do site usado para autenticação IP-HTTPS atende aos seguintes requisitos:  
  
-   O nome comum do certificado deverá ser corresponder ao nome do site IP-HTTPS.  
  
-   No campo Assunto, especifique o endereço IPv4 do adaptador externo do servidor de Acesso Remoto ou FQDN da URL IP-HTTPS.  
  
-   No campo Uso Avançado de Chave, use o OID (identificador de objeto) da Autenticação do Servidor.  
  
-   No campo Pontos de Distribuição da Lista de Certificados Revogados, especifique um ponto de distribuição da CRL que seja acessível pelos clientes do DirectAccess conectados à Internet.  
  
-   O certificado IP-HTTPS deve ter uma chave privada.  
  
-   O certificado IP-HTTPS deve ser importado diretamente para o repositório pessoal.  
  
-   Os certificados IP-HTTPS podem conter curingas no nome.  
  
##### <a name="to-install-the-ip-https-certificate-from-an-internal-ca"></a>Para instalar o certificado IP-HTTPS de uma AC interna  
  
1.  No servidor de acesso remoto: na tela **Iniciar** , digite**MMC. exe**e pressione Enter.  
  
2.  No console do MMC, no menu **Arquivo**, clique em **Adicionar/Remover Snap-in**.  
  
3.  Na caixa de diálogo **Adicionar ou Remover Snap-ins**, clique em **Certificados**, **Adicionar**, **Conta do computador**, **Avançar**, **Computador local**, **Concluir** e, por fim, em **OK**.  
  
4.  Na árvore de console do snap-in Certificados, abra **Certificados (Computador Local)\Pessoal\Certificados**.  
  
5.  Clique com o botão direito do mouse em **Certificados**, aponte para **Todas as Tarefas** e clique em **Solicitar Novo Certificado**.  
  
6.  Clique em **Avançar** duas vezes.  
  
7.  Na página **solicitar certificados** , marque a caixa de seleção do modelo de certificado e, se necessário, clique em **mais informações são necessárias para se registrar nesse certificado**.  
  
8.  Na caixa de diálogo **Propriedades do Certificado**, na guia **Assunto**, na área **Nome do requerente**, em **Tipo**, selecione **Nome Comum**.  
  
9. Em **Valor**, especifique o endereço IPv4 no adaptador externo do servidor de Acesso Remoto ou FQDN da URL IP-HTTPS e clique em **Adicionar**.  
  
10. Na área **Nome alternativo**, em **Tipo**, selecione **DNS**.  
  
11. Em **Valor**, especifique o endereço IPv4 no adaptador externo do servidor de Acesso Remoto ou FQDN da URL IP-HTTPS e clique em **Adicionar**.  
  
12. Na guia **Geral**, em **Nome amigável**, você pode digitar um nome que ajudará a identificar o certificado.  
  
13. Na guia **Extensões**, ao lado de **Uso de Chave Estendida**, clique na seta e verifique se a Autenticação do Servidor está na lista **Opções selecionadas**.  
  
14. Clique em **OK**, **Registrar** e em **Concluir**.  
  
15. No painel de detalhes do snap-in Certificados, verifique se o novo certificado foi registrado com Finalidades de Autenticação de Servidor.  
  
## <a name="configure-the-dns-server"></a><a name="ConfigDNS"></a>Configurar o servidor DNS  
Você deve configurar manualmente uma entrada DNS para o site do servidor de local da rede interna da sua implantação.  
  
### <a name="to-create-the-network-location-server-and-web-probe-dns-records"></a><a name="NLS_DNS"></a>Para criar o servidor de local de rede e os registros DNS de investigação da Web  
  
1.  No servidor DNS da rede interna: na tela **Iniciar** , digite * * DNSMGMT. msc * * e pressione Enter.  
  
2.  No painel esquerdo do console **Gerenciador DNS**, expanda a zona de pesquisa direta para o seu domínio. Clique com o botão direito no domínio e clique em **Novo Host (A ou AAAA)** .  
  
3.  Na caixa de diálogo **Novo Host**, na caixa **Nome (usa o nome de domínio pai se deixado em branco)** , digite o nome de DNS para o site do servidor de local de rede (este é o nome que os clientes do DirectAccess usam para conectar ao servidor de local de rede). Na caixa **Endereço IP**, digite o endereço IPv4 no servidor de local de rede e clique em **Adicionar Host**. Na caixa de diálogo **DNS**, clique em **OK**.  
  
4.  Na caixa de diálogo **Novo Host**, na caixa **Nome (usa o nome de domínio pai se deixado em branco)** , digite o nome do DNS para a sonda da web (o nome para a sonda da web é directaccess-webprobehost). Na caixa **Endereço IP**, digite o endereço IPv4 da sonda da web e clique em **Adicionar Host**. Repita esse processo para directaccess-corpconnectivityhost e para todos os verificadores de conectividade criados manualmente. Na caixa de diálogo **DNS**, clique em **OK**.  
  
5.  Clique em **Concluído**.  

![](../../../media/Step-1-Configure-the-DirectAccess-Infrastructure_3/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows</em> PowerShell***  
  
O cmdlet ou cmdlets do Windows PowerShell a seguir executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, embora eles apareçam com quebra de linha em várias linhas aqui devido a restrições de formatação.  
  
```  
Add-DnsServerResourceRecordA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv4Address <network_location_server_IPv4_address>  
Add-DnsServerResourceRecordAAAA -Name <network_location_server_name> -ZoneName <DNS_zone_name> -IPv6Address <network_location_server_IPv6_address>  
```  
  
Você também deve configurar entradas DNS para o seguinte:  
  
-   **O servidor IP-HTTPS**-os clientes DirectAccess devem ser capazes de resolver o nome DNS do servidor de acesso remoto da Internet.  
  
-   **Verificação de revogação de CRL**– o DirectAccess usa a verificação de revogação de certificado para a conexão IP-HTTPS entre clientes DirectAccess e o servidor de acesso remoto e para a conexão baseada em https entre o cliente DirectAccess e o servidor de local de rede. Em ambos os casos, os clientes do DirectAccess devem poder resolver e acessar o local do ponto de distribuição da CRL.  
  
## <a name="configure-active-directory"></a><a name="ConfigAD"></a>Configurar Active Directory  
O servidor de Acesso Remoto e todos os computadores cliente do DirectAccess devem ser ingressados em um domínio do Active Directory. Os computadores cliente do DirectAccess devem ser membros de um dos seguintes tipos de domínio:  
  
-   Domínios pertencentes à mesma floresta que o servidor de Acesso Remoto.  
  
-   Domínios pertencentes a florestas com relações de confiança bidirecional com a floresta do servidor de Acesso Remoto.  
  
-   Domínios com relação de confiança bidirecional com o domínio do servidor de Acesso Remoto.  
  
#### <a name="to-join-client-computers-to-the-domain"></a>Para ingressar computadores cliente no domínio  
  
1.  Na tela **Iniciar** , digite **Explorer. exe**e pressione Enter.  
  
2.  Clique com o botão direito do mouse no ícone Computador e em **Propriedades**.  
  
3.  Na página **Sistema**, clique em **Configurações avançadas do sistema**.  
  
4.  Na caixa de diálogo **Propriedades do Sistema**, na guia **Nome do Computador**, clique em **Alterar**.  
  
5.  Em **Nome do computador**, digite o nome do computador se você também estiver alterando o nome do computador ao ingressar o servidor no domínio. Em **Membro de**, clique em **Domínio** e digite o nome do domínio em que você deseja ingressar o servidor; por exemplo, corp.contoso.com, e clique em **OK**.  
  
6.  Quando você for solicitado a informar um nome de usuário e uma senha, digite o nome de usuário e a senha de um usuário com direitos de ingressar computadores no domínio e clique em **OK**.  
  
7.  Quando você vir uma caixa de diálogo de boas-vindas ao domínio, clique em **OK**.  
  
8.  Quando você for solicitado a reiniciar o computador, clique em **OK**.  
  
9. Na caixa de diálogo **Propriedades do Sistema**, clique em Fechar. Clique em **Reiniciar Agora** quando solicitado.  
  
![](../../../media/Step-1-Configure-the-DirectAccess-Infrastructure_3/PowerShellLogoSmall.gif)***<em>comandos equivalentes do Windows</em> PowerShell***  
  
O cmdlet ou cmdlets do Windows PowerShell a seguir executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, embora eles apareçam com quebra de linha em várias linhas aqui devido a restrições de formatação.  
  
Observe que você deve fornecer as credenciais do domínio depois de inserir o comando Add-Computer abaixo.  
  
```  
Add-Computer -DomainName <domain_name>  
Restart-Computer  
```  
  
## <a name="configure-gpos"></a><a name="ConfigGPOs"></a>Configurar GPOs  
Para implantar o acesso remoto, você precisa de um mínimo de dois objetos Política de Grupo: um Política de Grupo objeto contém configurações para o servidor de acesso remoto e um contém configurações para os computadores cliente do DirectAccess. Quando você configura o acesso remoto, o assistente cria automaticamente os objetos de Política de Grupo necessários. No entanto, se sua organização aplicar uma Convenção de nomenclatura ou se você não tiver as permissões necessárias para criar ou editar objetos Política de Grupo, eles deverão ser criados antes de configurar o acesso remoto.  
  
Para criar Política de Grupo objetos, consulte [criar e editar um objeto política de grupo](https://technet.microsoft.com/library/cc754740.aspx).  
  
> [!IMPORTANT]  
> O administrador pode vincular manualmente os objetos de Política de Grupo do DirectAccess a uma unidade organizacional usando estas etapas:  
>   
> 1.  Antes de configurar o DirectAccess, vincule os GPOs criados às respectivas Unidades Organizacionais.  
> 2.  Configure o DirectAccess, especificando um grupo de segurança para computadores cliente.  
> 3.  O administrador de acesso remoto pode ou não ter permissões para vincular os objetos de Política de Grupo ao domínio. De qualquer maneira, os Objetos de Política de Grupo serão configurados automaticamente. Se os GPOs já estiverem vinculados a uma OU, os links não serão removidos, e os GPOs não serão vinculados ao domínio. Para um GPO de servidor, a OU deverá conter o objeto de computador do servidor, ou o GPO será vinculado à raiz do domínio.  
> 4.  Se a vinculação à UO não tiver sido feita antes de executar o assistente do DirectAccess, depois que a configuração for concluída, o administrador de domínio poderá vincular os objetos do DirectAccess Política de Grupo às unidades organizacionais necessárias. O link para o domínio pode ser removido. As etapas para vincular um objeto de Política de Grupo a uma unidade organizacional podem ser encontradas [aqui](https://technet.microsoft.com/library/cc732979.aspx).  
  
> [!NOTE]  
> Se um objeto Política de Grupo tiver sido criado manualmente, será possível durante a configuração do DirectAccess que o objeto Política de Grupo não estará disponível. O objeto Política de Grupo pode não ter sido replicado para o controlador de domínio mais próximo para o computador de gerenciamento. Neste caso, o administrador pode aguardar a replicação ser concluída, ou forçá-la.  
  
## <a name="configure-security-groups"></a><a name="ConfigSGs"></a>Configurar grupos de segurança  
As configurações do DirectAccess contidas no computador cliente Política de Grupo objeto são aplicadas somente a computadores que são membros dos grupos de segurança que você especificar ao configurar o acesso remoto. Além disso, se você está usando os grupos de segurança para gerenciar os servidores de aplicativo, crie um grupo de segurança para tais servidores.  
  
### <a name="to-create-a-security-group-for-directaccess-clients"></a><a name="Sec_Group"></a>Para criar um grupo de segurança para clientes do DirectAccess  
  
1.  Na tela **Iniciar** , digite**DSA. msc**e pressione Enter. No console **Usuários e Computadores do Active Directory**, no painel esquerdo, expanda o domínio que conterá o grupo de segurança, clique com o botão direito do mouse em **Usuários**, aponte para **Novo** e clique em **Grupo**.  
  
2.  Na caixa de diálogo **Novo Objeto - Grupo**, em **Nome do grupo**, digite o nome do grupo de segurança.  
  
3.  Em **Escopo do grupo**, clique em **Global**, em **Tipo de grupo**, escolha **Segurança** e clique em **OK**.  
  
4.  Clique no grupo de segurança dos computadores cliente do DirectAccess e, na caixa de diálogo de propriedades, clique na guia **Membros**.  
  
5.  Na guia **Membros**, clique em **Adicionar**.  
  
6.  Na caixa de diálogo **Selecionar Usuários, Contatos, Computadores ou Contas de Serviço**, selecione os computadores cliente que você deseja habilitar para o DirectAccess e clique em **OK**.  
  
![](../../../media/Step-1-Configure-the-DirectAccess-Infrastructure_3/PowerShellLogoSmall.gif)**comandos equivalentes do Windows** PowerShell  
  
O cmdlet ou cmdlets do Windows PowerShell a seguir executam a mesma função que o procedimento anterior. Insira cada cmdlet em uma única linha, embora eles apareçam com quebra de linha em várias linhas aqui devido a restrições de formatação.  
  
```  
New-ADGroup -GroupScope global -Name <DirectAccess_clients_group_name>  
Add-ADGroupMember -Identity DirectAccess_clients_group_name -Members <computer_name>  
```  
  
## <a name="configure-the-network-location-server"></a><a name="ConfigNLS"></a>Configurar o servidor de local de rede  
O servidor de local de rede deve estar em um servidor com alta disponibilidade e possuir um certificado SSL válido confiável para os clientes do DirectAccess. Existem duas opções de certificado para o certificado do servidor de local de rede:  
  
-   **Particular**-os itens a seguir são necessários, se ainda não existirem:  
  
    -   Um certificado de site usado para o servidor de local de rede. A entidade do certificado deve ser a URL do servidor de local de rede.   
  
    -   Um ponto de distribuição de CRL com alta disponibilidade na rede interna.  
  
-   **Autoassinado**-os itens a seguir são necessários, se ainda não existirem:  
  
    > [!NOTE]  
    > Certificados autoassinados não podem ser usados em implantações multissite.  
  
    -   Um certificado de site usado para o servidor de local de rede. A entidade do certificado deve ser a URL do servidor de local de rede.  
  
> [!NOTE]  
> Se o site do servidor de local de rede encontrar-se no servidor de Acesso Remoto, um site será criado automaticamente ao configurar o Acesso Remoto, ligado ao certificado de servidor fornecido.  
  
#### <a name="to-install-the-network-location-server-certificate-from-an-internal-ca"></a>Para instalar o certificado de servidor de local de rede de uma AC interna  
  
1.  No servidor que hospedará o site do servidor de local de rede: na tela **Iniciar** , digite**MMC. exe**e pressione Enter.  
  
2.  No console do MMC, no menu **Arquivo**, clique em **Adicionar/Remover Snap-in**.  
  
3.  Na caixa de diálogo **Adicionar ou Remover Snap-ins**, clique em **Certificados**, **Adicionar**, **Conta do computador**, **Avançar**, **Computador local**, **Concluir** e, por fim, em **OK**.  
  
4.  Na árvore de console do snap-in Certificados, abra **Certificados (Computador Local)\Pessoal\Certificados**.  
  
5.  Clique com o botão direito do mouse em **Certificados**, aponte para **Todas as Tarefas** e clique em **Solicitar Novo Certificado**.  
  
6.  Clique em **Avançar** duas vezes.  
  
7.  Na página **solicitar certificados** , marque a caixa de seleção do modelo de certificado e, se necessário, clique em **mais informações são necessárias para se registrar nesse certificado**.  
  
8.  Na caixa de diálogo **Propriedades do Certificado**, na guia **Assunto**, na área **Nome do requerente**, em **Tipo**, selecione **Nome Comum**.  
  
9. Em **Valor**, insira o FQDN do site do servidor de local de rede e clique em **Adicionar**.  
  
10. Na área **Nome alternativo**, em **Tipo**, selecione **DNS**.  
  
11. Em **Valor**, insira o FQDN do site do servidor de local de rede e clique em **Adicionar**.  
  
12. Na guia **Geral**, em **Nome amigável**, você pode digitar um nome que ajudará a identificar o certificado.  
  
13. Clique em **OK**, **Registrar** e em **Concluir**.  
  
14. No painel de detalhes do snap-in Certificados, verifique se o novo certificado foi registrado com Finalidades de Autenticação de Servidor.  
  

  


